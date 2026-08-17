# EV-Market-Analysis

Integrating three separate electric vehicle data sources into a SQLite database to analyze sales trends, pricing, and charging infrastructure.

## Overview

Understanding the EV market means looking at more than one angle: how adoption is growing, what vehicles actually cost, and where the charging infrastructure exists to support them. This project pulls together three independently-sourced datasets, loads them into a SQLite database, and analyzes each to build a fuller picture of the U.S. EV market.

The resulting analysis can help:

- **Analysts** understand how EV sales growth, pricing, and infrastructure trends relate to one another
- **Consumers/researchers** see where charging infrastructure is concentrated relative to EV adoption
- **Data practitioners** see an example of working with multiple real-world sources at different levels of granularity, and handling that honestly rather than forcing a bad join

## Data

Three cleaned datasets, provided here as pickle files (`.pkl`) from earlier data collection and cleaning steps:

- `df_flat_cleaned.pkl` - national annual EV sales and stock figures by region and year (IEA Global EV Data)
- `df_web_cleaned.pkl` - individual EV listings with pricing details, scraped from manufacturer/dealer sites
- `df_api_cleaned.pkl` - individual charging station locations, pulled via API

All three are loaded into a local SQLite database (`ev_project.db`) for querying.

## Approach

1. **Load and store** - loaded the three cleaned datasets and wrote each into its own table in a SQLite database.
2. **Assess joinability** - an earlier version of this analysis attempted to join all three tables directly. Because they operate at different levels of granularity (country-year statistics, individual vehicle listings, and individual station locations) with no shared key, that join produced a misleading cross join rather than a meaningful result. This version instead queries and analyzes each source independently.
3. **Analysis and visualization** - built five visualizations covering price distribution, sales vs. stock growth over time, charging station concentration by state, average price by brand, and sales growth against average listed price on a dual-axis chart.

## Results

| Source | Finding |
|---|---|
| EV Sales & Stock | U.S. EV stock and sales trend upward, with stock growing faster than annual sales |
| EV Listing Prices | Prices are right-skewed, mostly clustering between $50k-$100k |
| Charging Infrastructure | Stations are concentrated in a few states, led by California |
| Data Limitation | The three sources don't share a common key, so each was analyzed independently rather than joined |

Beyond the individual findings, the more important takeaway is the data limitation itself: the three sources genuinely can't be meaningfully joined given their different levels of granularity, and forcing a join (as an earlier version of this analysis did) produces a chart that looks legitimate but reflects a coincidental relationship rather than a real one. Catching and correcting that is as much a part of this project as the findings themselves.

## Tech Stack

- Python, pandas
- SQLite (sqlite3)
- matplotlib, seaborn

## Future Improvements

- Source raw, re-collectible data for the web-scraped and API datasets to make the full pipeline reproducible from scratch
- Explore whether a looser, well-justified join (e.g. by state, if listing location data becomes available) could connect pricing to infrastructure meaningfully
- Extend the sales/stock analysis beyond the U.S. to compare adoption curves across countries
- Add year-over-year growth rate calculations rather than raw trend lines

## Author

Omar Rodriguez Arellano

Part of my [Data Science Portfolio](https://omaralexrod.github.io)
