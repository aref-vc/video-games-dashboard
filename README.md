# Video Games Sales Dashboard

Interactive D3.js dashboard analyzing 40 years of video game sales data (1980-2020).

## Dataset

- **Source**: Video Games Sales dataset
- **Records**: 16,598 games
- **Time Range**: 1980-2020
- **Metrics**: Global, NA, EU, JP, and Other regional sales

## Features

### 4 Narrative Sections

1. **Industry Evolution** - Platform timeline, genre trends, market cycles
2. **Regional Markets** - Geographic preferences, market share analysis
3. **Business Intelligence** - Publisher performance, franchise success
4. **Key Insights** - Top performers, platform lifecycles, data explorer

### 16 Interactive Visualizations

- Streamgraph, Bump Chart, Calendar Heatmap, Sunburst
- Chord Diagram, Marimekko, Radar Charts, Cartogram
- Force Graph, Beeswarm, Treemap, Parallel Coordinates
- Circle Packing, Spiral Timeline, Waterfall, Scatterplot Explorer

## Tech Stack

- **D3.js** - Data visualization
- **Single HTML file** - No build step required
- **Electric Dark theme** - Neon accents on dark background

## Usage

Open `index.html` in a modern browser. Requires a local server for CSV loading:

```bash
python -m http.server 8000
# or
npx serve .
```

Then visit `http://localhost:8000`
