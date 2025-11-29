# Video Games Sales Dashboard

Interactive D3.js dashboard analyzing 40 years of video game sales data (1980-2020).

![Dashboard Preview](https://img.shields.io/badge/Charts-64-00BAFE) ![D3.js](https://img.shields.io/badge/D3.js-v7-BEFF00) ![Single File](https://img.shields.io/badge/Architecture-Single%20HTML-FFC000)

## Dataset

| Attribute | Value |
|-----------|-------|
| Source | Video Games Sales Dataset |
| Records | 16,598 games |
| Time Range | 1980-2020 |
| Regions | NA, EU, JP, Other, Global |
| Fields | Name, Platform, Year, Genre, Publisher, Sales |

### Data Schema

```
Name,Platform,Year,Genre,Publisher,NA_Sales,EU_Sales,JP_Sales,Other_Sales,Global_Sales
```

- Sales values are in millions USD
- Year has gaps (2018-2019 sparse, 2017/2020 minimal data)

---

## Architecture

### Single-File Design

The entire dashboard is contained in `index.html` (~8,500 lines):

```
index.html
├── <head>
│   ├── Meta tags & viewport
│   ├── D3.js v7 CDN import
│   └── <style> - All CSS (~500 lines)
├── <body>
│   ├── <header> - Title, stats, navigation tabs
│   ├── <main> - 8 sections with chart containers
│   └── <div id="tooltip"> - Shared tooltip element
└── <script>
    ├── Theme colors & configuration
    ├── State management
    ├── Data loading & processing
    ├── 64 chart functions
    ├── Utility functions (tooltip, helpers)
    └── Initialization & event handlers
```

### Why Single File?

- **Zero build step** - Open and run
- **Easy deployment** - Single file to host
- **Self-contained** - All dependencies via CDN
- **Portable** - Share/archive as one file

---

## Sections & Charts

### 8 Narrative Sections (64 Charts Total)

| # | Section | Focus | Charts |
|---|---------|-------|--------|
| 1 | Industry Evolution | Market trends over time | 8 |
| 2 | Regional Markets | Geographic analysis | 8 |
| 3 | Business Intelligence | Publisher performance | 8 |
| 4 | Key Insights | Top performers & patterns | 8 |
| 5 | Franchise Analysis | Major gaming franchises | 8 |
| 6 | Decade Comparison | Era-by-era analysis | 8 |
| 7 | Platform Deep Dive | Console/platform focus | 8 |
| 8 | Hidden Gems | Underrated & niche games | 8 |

### Chart Types Used

**Section 1 - Industry Evolution**
- Genre Streamgraph (stacked area)
- Platform Rankings (bump chart)
- Annual Market Heatmap (year × genre matrix)
- Market Breakdown (sunburst)
- Stacked Area Chart
- Slope Chart
- Horizon Chart
- Animated Timeline

**Section 2 - Regional Markets**
- Genre Flow (chord diagram)
- Market Share (marimekko)
- Regional DNA (radar charts)
- World View (cartogram)
- Diverging Bar Chart
- Proportional Symbol Map
- Normalized Stacked Area
- Small Multiples Line

**Section 3 - Business Intelligence**
- Publisher Network (force graph)
- Sales Distribution (beeswarm)
- Publisher Portfolio (treemap)
- Success Factors (parallel coordinates)
- Lollipop Chart
- Dumbbell Chart
- Sankey Diagram
- Box Plot

**Section 4 - Key Insights**
- Top Games (circle packing)
- Release Timeline (spiral)
- Market Waterfall
- Data Explorer (scatterplot)
- Word Cloud
- Histogram
- Dot Matrix
- Ranking Table

**Section 5 - Franchise Analysis**
- Franchise Overview (bubble chart)
- Franchise Growth (multi-line)
- Platform Distribution (stacked bar)
- Genre Matrix (heatmap)
- Release Timeline
- Regional Breakdown (radial bar)
- Quality Trajectory (connected scatter)
- Ecosystem Network

**Section 6 - Decade Comparison**
- Decade Sales (grouped bar)
- Genre Evolution (donut small multiples)
- Platform Count (slope chart)
- Publisher Shifts (butterfly chart)
- Regional Contribution (stacked bar)
- Sales Distribution (box plot)
- Naming Trends (word cloud)
- Genre Trends (area small multiples)

**Section 7 - Platform Deep Dive**
- Platform Lifespans (Gantt-style)
- Lifecycle Sales (area chart)
- Release Density (heatmap)
- Exclusive Analysis (grouped bar)
- Genre Strengths (radar)
- Generation Flow (sankey)
- Library Analysis (scatter)
- Launch Timeline

**Section 8 - Hidden Gems**
- Regional Cult Hits (strip plot)
- Japan Phenomena (scatter)
- Niche Champions (bar chart)
- Small Publisher Hits (bubble)
- Underrated Timeline
- Market Gaps (heatmap)
- Emerging Markets (lollipop)
- Discovery Table

---

## Design System

### Electric Dark Theme

```css
/* Backgrounds */
--bg-main: #10100E      /* Primary background */
--bg-elevated: #1A1A18  /* Cards, header */
--bg-accent: #242422    /* Hover states */

/* Text */
--text-primary: #FFFFE3   /* Headings */
--text-secondary: #E6E6CE /* Body text */
--text-tertiary: #B3B3A3  /* Labels, captions */

/* Accent Colors (5) */
--lime: #BEFF00    /* Primary accent */
--cyan: #00BAFE    /* Secondary accent */
--amber: #FFC000   /* Warning, highlights */
--emerald: #00DE71 /* Success, positive */
--coral: #F04E50   /* Danger, negative */
```

### Typography

- **Font**: Berkeley Mono (fallback: SF Mono, Fira Code)
- **Monospace throughout** for data-heavy display

### Layout

- **Tab Navigation**: 8 tabs with scroll progress indicators
- **2×2 Grid**: Each section displays charts in scrollable 2-column grid
- **Responsive**: Charts resize based on container dimensions

---

## Code Structure

### State Management

```javascript
const state = {
  data: [],           // Loaded CSV data
  activeSection: 0,   // Current tab index (0-7)
  initialized: {}     // Track which sections rendered
};
```

### Chart Function Pattern

All 64 charts follow this structure:

```javascript
function createChartName() {
  // 1. Get container and clear
  const container = document.getElementById('chart-id');
  container.innerHTML = '';

  // 2. Set dimensions
  const rect = container.getBoundingClientRect();
  const margin = { top: 20, right: 20, bottom: 30, left: 40 };
  const width = rect.width - margin.left - margin.right;
  const height = rect.height - margin.top - margin.bottom;

  // 3. Create SVG
  const svg = d3.select(container)
    .append('svg')
    .attr('width', rect.width)
    .attr('height', rect.height);

  const g = svg.append('g')
    .attr('transform', `translate(${margin.left},${margin.top})`);

  // 4. Process data
  const processedData = state.data.filter(...).map(...);

  // 5. Create scales
  const x = d3.scaleLinear().domain([...]).range([0, width]);
  const y = d3.scaleLinear().domain([...]).range([height, 0]);

  // 6. Draw visualization
  g.selectAll('.element')
    .data(processedData)
    .join('rect')
    ...

  // 7. Add axes
  g.append('g').call(d3.axisBottom(x));

  // 8. Add interactivity
  .on('mouseover', (event, d) => showTooltip(event, content))
  .on('mouseout', hideTooltip);
}
```

### Lazy Loading

Charts render only when their section becomes visible:

```javascript
function showSection(index) {
  state.activeSection = index;
  // Show/hide sections...

  if (!state.initialized[index]) {
    requestAnimationFrame(() => {
      initializeSection(index);
      state.initialized[index] = true;
    });
  }
}
```

### Data Processing Utilities

```javascript
// Franchise detection
const franchisePatterns = {
  'Mario': /mario|super mario/i,
  'Pokemon': /pokemon|pokémon/i,
  'Call of Duty': /call of duty|cod/i,
  // ...
};

// Decade classification
function getDecade(year) {
  if (year < 1990) return '1980s';
  if (year < 2000) return '1990s';
  if (year < 2010) return '2000s';
  return '2010s';
}

// Platform groupings
const platformTypes = {
  'Sony': ['PS', 'PS2', 'PS3', 'PS4', 'PSP', 'PSV'],
  'Nintendo': ['NES', 'SNES', 'N64', 'GC', 'Wii', 'WiiU', 'GB', 'GBA', 'DS', '3DS'],
  'Microsoft': ['XB', 'X360', 'XOne'],
  // ...
};
```

---

## Usage

### Quick Start

```bash
# Clone repository
git clone https://github.com/aref-vc/video-games-dashboard.git
cd video-games-dashboard

# Start local server (required for CSV loading)
python -m http.server 8000
# or
npx serve .

# Open browser
open http://localhost:8000
```

### Requirements

- Modern browser with ES6+ support
- Local HTTP server (for CSV loading due to CORS)
- No build tools or npm install required

### Interaction

- **Click tabs** to navigate sections
- **Hover charts** for detailed tooltips
- **Scroll within sections** to see all 8 charts
- **Tab indicators** show scroll progress

---

## File Structure

```
video-games-dashboard/
├── index.html              # Complete dashboard (HTML + CSS + JS)
├── video_games_sales.csv   # Source dataset
└── README.md               # This file
```

---

## Performance Considerations

- **Lazy loading**: Charts render on-demand when section is viewed
- **Data sampling**: Large datasets sampled for performance (e.g., top 500 for beeswarm)
- **requestAnimationFrame**: Smooth rendering without blocking UI
- **D3 join pattern**: Efficient DOM updates

---

## Browser Support

| Browser | Support |
|---------|---------|
| Chrome | Full |
| Firefox | Full |
| Safari | Full |
| Edge | Full |
| IE11 | Not supported |

---

## License

MIT License - See repository for details.

---

## Credits

- **D3.js** - Mike Bostock and contributors
- **Dataset** - Video Games Sales dataset
- **Design** - Electric Dark theme with Berkeley Mono typography
