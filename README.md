# Solar Customer Wholesale Cost Analysis

This project ingests, cleans and analyses Ausgrid solar customer consumption data, combines it with NEM wholesale price data, and models comparative wholesale costs (with and without solar export and hedging) for residential customers.

## Repository Structure

```
.
├── 01-ausgrid-solar-customers/         # Raw Ausgrid CSVs, shapefiles, and export outputs
│   ├── 2010-2011 Solar home electricity data.csv
│   ├── 2011-2012 Solar home electricity data.csv
│   ├── 2012-2013 Solar home electricity data.csv
│   ├── Ausgrid solar home electricity data notes (Aug 2014).pdf
│   └── POA_2021_AUST_GDA2020_SHP/      # Boundary shapefiles
├── 02-nem-wholesale-price/             # NEM price CSV/RDS outputs
│   └── nem_30min_prices.{csv,rds}
├── 11-Solar_load_profile.qmd           # (Script 1) Prepare & reshape Ausgrid data
├── 12-Wholesale_prices.qmd             # (Script 2) Download, clean & summarise NEM wholesale prices
└── 13-model_wholesale_cost.qmd         # (Script 3) Merge profiles & prices, compute wholesale costs & hedging
```

## Scripts

### 11-Solar_load_profile.qmd  
1. **Load packages**: `data.table`, `lubridate`, `sf`, `dplyr`, `ggplot2`, etc.  
2. **Combine & clean** three years of Ausgrid “Solar home electricity” CSVs.  
3. **Reshape** from wide to long (half-hourly) then back to wide per customer.  
4. **Compute** total and net consumption and validate aggregates.  
5. **Export** cleaned data as RDS & CSV.  
6. **Plot** customer locations in NSW and annual consumption ranges.

### 12-Wholesale_prices.qmd  
1. **Load packages**: `data.table`, `lubridate`, `ggplot2`, etc.  
2. **Download** historical NEM price CSVs from AEMO (2005–present), re-download last two months.  
3. **Combine** into a single data.table, convert timestamps to Australia/Brisbane.  
4. **Aggregate** half-hourly weighted average spot prices by region.  
5. **Export** as CSV & RDS.  
6. **Plot** average and boxplot distributions (static & animated GIFs).

### 13-model_wholesale_cost.qmd  
1. **Load** cleaned consumption (`dt_ausgrid_long.rds`) and price (`nem_30min_prices.rds`) data.  
2. **Rescale** solar generation to a 6.6 kW system.  
3. **Standardise** timestamps (to 2020) for merging.  
4. **Compute** annual unit costs for “total” vs “net” consumption (with/without FiT).  
5. **Merge** half-hourly consumption with spot prices.  
6. **Define** hedging volumes (base swaps & cap contracts) and FiT scenarios.  
7. **Calculate** spot-only and hedged wholesale costs per interval, then summarise by state, quarter, and retailer type.  
8. **Visualise** cost comparisons, hedged volumes, demand profiles, and difference metrics.

## Requirements

- **R** ≥ 4.0 with the following packages:  
  `data.table`, `lubridate`, `tidyr`, `dplyr`, `ggplot2`, `sf`, `patchwork`, `readr`, `gganimate`, `magick`, `gifski`, `imputeTS`, `purrr`, `rnaturalearth`, `ggspatial`

- **Quarto** or **RStudio** to render the `.qmd` scripts to HTML.

## Usage

1. Clone the repo:  
   ```bash
   git clone https://github.com/EconomistMike/solar-customers-wholesale-cost.git
   cd solar-customers-wholesale-cost
   ```
2. Open and run each script in order:  
   ```bash
   quarto render 11-Solar_load_profile.qmd
   quarto render 12-Wholesale_prices.qmd
   quarto render 13-model_wholesale_cost.qmd
   ```
3. View the HTML outputs in your browser.

---
*Michael Wu*  
Centre for Independent Studies – Energy Team

