# Forest Impact Simulator - R Version

R Markdown companion to the [Forest Impact Simulator](https://forest-impact-simulator.vercel.app/) web app.

## About

Simulates forest carbon impacts, biodiversity, resilience, water retention, and air quality for planting and clear-cutting — using the **same growth curves and modifiers** as the TypeScript web app.

## Features

- **Planting mode**: Cumulative carbon with realistic year-by-year growth factors (5% → 100% of mature rate)
- **Clear-cutting mode**: Immediate release (lifetime stored carbon) + lost future sequestration
- **Web app CSV import**: Reads the single-row export from the live site
- **Plot CSV / manual input**: Per-plot analysis when not using a web export
- **Visualizations**: CO₂ bars, environmental metrics, growth-curve projection
- **Exports**: Processed CSV + web-app-compatible JSON

## Quick Start

1. Install packages:
```r
install.packages(c("tidyverse", "ggplot2", "jsonlite", "readr", "dplyr", "plotly"))
```

2. Optional: run a simulation on the [web app](https://forest-impact-simulator.vercel.app/) and download CSV, or use the included `sample-forest-planting-data.csv`.

3. Open `forest-impact-simulator.Rmd` in RStudio and run all chunks.

4. Check `output/` for CSV and JSON results.

## Data Formats

### Web app CSV (recommended)

Single header + one data row — produced by **Export → CSV** in the web app. Columns include:

```
timestamp, simulator_version, simulation_years, latitude, longitude,
region_north, region_south, region_east, region_west,
soil_carbon_g_kg, soil_ph, soil_texture, temperature_c, precipitation_mm,
annual_carbon_sequestration_kg_co2_year, total_carbon_kg_co2,
biodiversity_impact, forest_resilience, water_retention_percent,
air_quality_improvement_percent, average_biodiversity, average_resilience,
area_hectares, total_trees, spacing_meters, density_trees_hectare,
years_to_complete, trees_per_season,
tree_names, tree_scientific_names, tree_carbon_rates_kg_co2_year, tree_percentages
```

Multiple species are semicolon-separated in the tree columns (e.g. `Oak;Pine`).

See also [`doc/examples/csv_usage_examples.md`](https://github.com/karimogit/Forest-Impact-Simulator/blob/main/doc/examples/csv_usage_examples.md) in the TypeScript repo.

### Plot CSV (optional)

One row per plot:

| Column | Required | Description |
|--------|----------|-------------|
| `land_id` / `plot_id` | Yes | Plot identifier |
| `area_ha` | Yes | Area in hectares |
| `tree_type` | Yes | Species name |
| `carbon_per_tree` | Yes | Mature kg CO₂/year **per tree** |
| `trees_per_ha` | No | Default 625 |
| `biodiversity_score` | No | 0–5 scale |
| `resilience_score` | No | 0–5 scale |
| `latitude`, `longitude`, `simulation_years` | No | Shared across rows |
| `soil_carbon`, `temperature`, `precipitation` | No | Environmental inputs |

Save as `sample-forest-plot-data.csv` or replace the sample file.

### Manual input

Edit the `manual_land_data` / `manual_location` frames in the notebook when no CSV is present.

## Calculations (aligned with web app)

**Planting growth factors:** Year 1: 5%, 2: 15%, 3: 30%, 4: 50%, 5: 70%, 6–10: 80%, 11–20: 90%, 20+: 100%.

**Clear-cutting:** Sum of age-based sequestration from year 1…`tree_age` (immediate release) plus lost future sequestration over the simulation period.

**Soil modifier:** `+ soil_carbon_g_kg × 0.1` kg CO₂/year per tree-equivalent rate.

**Comparisons:** Car (~4,600 kg CO₂/year), NY–London flight (~986 kg), US household electricity (~7,500 kg/year).

## Usage

```r
mode <- "planting"   # or "clear-cutting"
tree_age <- 20       # clear-cutting only
```

## Related Projects

- **TypeScript / Web**: [Forest-Impact-Simulator](https://github.com/karimogit/Forest-Impact-Simulator)
- **Python**: [Forest-Impact-Simulator-Python](https://github.com/karimogit/Forest-Impact-Simulator-Python)
- **Live app**: [forest-impact-simulator.vercel.app](https://forest-impact-simulator.vercel.app/)

## License

MIT — see [LICENSE](LICENSE).
