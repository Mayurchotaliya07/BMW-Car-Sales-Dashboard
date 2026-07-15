# BMW Sales Dashboard (2016–2024)

An end-to-end Power BI analytics project that turns 50,000 raw BMW transaction records into a single-page executive dashboard covering revenue, volume, pricing, and product-mix performance across models, regions, fuel types, and transmissions.

---

## Project Overview

**Business Problem**
Automotive sales data is typically scattered across transactional exports with no consistent way to compare model performance, regional demand, or product mix at a glance. Decision-makers need a single source of truth to answer recurring questions — which models sell best, which regions generate the most revenue, and how price relates to volume and mileage — without opening spreadsheets or running manual queries.

**Business Objective**
Build a self-service Power BI dashboard that consolidates BMW sales transactions into a set of KPIs, trend charts, and comparison visuals, letting a sales or product team slice performance instantly by Year, Model, Region, Fuel Type, and Transmission.

**Dataset Overview**
The source file (`BMW_sales_data__2010-2024__1_.csv`) contains **50,000 rows and 11 columns**:

| Column | Description |
|---|---|
| Model | BMW nameplate (11 distinct models) |
| Year | Model year, 2010–2024 |
| Region | Sales region (6 categories) |
| Color | Exterior color (6 categories) |
| Fuel_Type | Petrol, Diesel, Hybrid, Electric |
| Transmission | Manual or Automatic |
| Engine_Size_L | Engine displacement in liters |
| Mileage_KM | Vehicle mileage in kilometers |
| Price_USD | Listed price in USD |
| Sales_Volume | Units sold for that record (100–9,999) |
| Sales_Classification | Rule-based flag — "High" if Sales_Volume ≥ 7,000, otherwise "Low" (verified programmatically against the data) |

**Why this dashboard was built**
To demonstrate a complete BI workflow — data modeling, DAX measure design, and dashboard storytelling — on a realistic multi-dimensional sales dataset, and to give stakeholders a single interactive view instead of static reports.

**Expected business value**
Faster identification of top- and bottom-performing models, clearer visibility into regional and fuel-type demand, and a reusable filtering layer (Year / Model / Region / Fuel Type / Transmission) that supports ad-hoc questions without rebuilding the report.

---

## Dashboard Preview

![Dashboard Preview](images/dashboard.png)

*Single-page report (3400 × 1500 canvas) with a custom dark background image, gold accent title text, eight KPI cards, five slicers, and seven charts.*

---

## Dashboard Features

- **Dynamic KPI Cards** — Eight card visuals surface Total Cars, Total Sales (units), Average Price, Average Sales Volume (ASV), Average Revenue per Car, and Average Mileage at a glance, all recalculating live as filters change.
- **Interactive Slicers** — Five slicers (Year, Model, Region, Fuel Type, Transmission) let users narrow the entire page to any combination of dimensions.
- **Dynamic Dashboard Subtitle** — A dedicated `Dashboard Subtitle` measure is bound to its own card visual, generating descriptive text that updates based on current filter selections rather than a static label.
- **Tooltips** — Standard Power BI hover tooltips are enabled on all charts, surfacing underlying values (e.g., exact Sales_Volume, Price_USD) without cluttering the visual itself.
- **Cross Filtering** — Because every visual is bound to the same table, selecting a data point (e.g., a model bar, a fuel-type slice) filters every other visual and KPI card on the page.
- **Interactive Charts** — Seven chart visuals (line, two column charts, one bar chart, two pie charts, one scatter chart) cover trend, ranking, composition, and correlation analysis in a single view.

---

## KPIs

| KPI | Measured Value (unfiltered) | Description |
|---|---|---|
| Total Cars | 50,000 | Count of transaction records in the dataset (`COUNTROWS`) |
| Total Sales / Total Quantity | 253,375,734 units | Sum of `Sales_Volume` across all records |
| Average Price | $75,034.60 | Mean of `Price_USD` across all records |
| Average Sales Volume (ASV) | 5,067.51 units | Mean of `Sales_Volume` per record |
| Average Revenue per Car | $75,035.77 | Total implied revenue (`Price_USD × Sales_Volume`) divided by total units sold |
| Average Mileage | 100,307.20 km | Mean of `Mileage_KM` across all records |
| Total Models | 11 | Distinct count of `Model` |

**Why each KPI matters**
- **Total Cars** frames the size of the transaction base being analyzed — the denominator for every rate-based metric on the page.
- **Total Sales / Total Quantity** is the headline volume metric, used across the trend line, ranking charts, and the High/Low classification.
- **Average Price** is the primary pricing benchmark used to compare models, fuel types, and regions on a like-for-like basis.
- **ASV** shows the typical scale of a single sales record, useful for spotting whether "High" classified transactions are outliers or the norm.
- **Average Revenue per Car** approximates realized revenue per unit; comparing it against Average Price reveals whether volume is being won through discounting (it is not — see Business Insights).
- **Average Mileage** provides a proxy for vehicle condition/usage, tested against price in the scatter plot.
- **Total Models** anchors the model-level breakdowns and confirms the full BMW lineup (sedans, SUVs, M-performance, and i-electrified models) is represented.

---

## Dashboard Visuals

**1. Sales Trend by Year** (Line Chart)

<img width="1032" height="270" alt="image" src="https://github.com/user-attachments/assets/277e7e1f-349e-48b4-8611-890eb5fccb9c" />

- *Purpose:* Plots Sum of Sales_Volume against Year (2010–2024).
- *Business question:* Is total sales volume growing, shrinking, or flat over time?
- *Business value:* Identifies the best and worst-performing years for volume planning.
- *Insight:* Volume is essentially flat across the 15-year window — 2022 is the peak year and 2023 the trough, with less than a 10% swing between them.

**2. Top 5 Models by Sales** (Clustered Column Chart)

<img width="636" height="407" alt="image" src="https://github.com/user-attachments/assets/15ae3526-65d4-41d9-bbe7-afc692dece0c" />

- *Purpose:* Ranks the top 5 models by Sum of Sales_Volume.
- *Business question:* Which models move the most units?
- *Business value:* Directs inventory and marketing spend toward proven performers.
- *Insight:* 7 Series, 3 Series, i8, X1, and 5 Series lead total unit volume, though the spread between them is narrow.

**3. Top 5 Models by Quantity Sold** (Clustered Bar Chart, using the `Total Quantity` measure)

<img width="527" height="342" alt="image" src="https://github.com/user-attachments/assets/4f1ceac1-ecca-4849-ac37-245465d3ee19" />

- *Purpose:* A horizontal ranking view of the same top-selling models by total quantity.
- *Business question:* Confirms the volume leaders in an easier-to-scan bar format for reporting.
- *Business value:* Useful for slide-ready reporting where a horizontal ranking reads better than a vertical one.
- *Insight:* Rankings mirror the column chart above, reinforcing 7 Series and 3 Series as consistent volume leaders.

**4. Color Popularity** (Pie Chart — Color × Sales_Volume)

<img width="445" height="266" alt="image" src="https://github.com/user-attachments/assets/52ec6b03-6bfd-4e5e-b9f3-77f5bcbf65b1" />

- *Purpose:* Shows the share of total sales volume by exterior color.
- *Business question:* Which colors are customers buying most?
- *Business value:* Informs paint-shop production ratios and dealer stock mix.
- *Insight:* All six colors sit within roughly one percentage point of each other (Red highest, Black lowest) — no color dominates demand.

**5. Fuel Type Distribution** (Pie Chart — Fuel_Type × Sales_Volume)

<img width="441" height="342" alt="image" src="https://github.com/user-attachments/assets/d0ae370c-ffd5-4c4b-8bc5-9bc7a15246a0" />

- *Purpose:* Shows the share of total sales volume by fuel type.
- *Business question:* How is demand split across Petrol, Diesel, Hybrid, and Electric?
- *Business value:* Supports powertrain investment and marketing allocation decisions.
- *Insight:* Hybrid narrowly leads, followed closely by Petrol, Electric, and Diesel — all four sit within about 3% of each other.

**6. Mileage vs Price Scatter Plot** (Scatter Chart — Mileage_KM × Price_USD, sized by Sales_Volume, colored by Fuel_Type, detailed by Model)

<img width="580" height="333" alt="image" src="https://github.com/user-attachments/assets/db063556-f3d4-414b-bfac-f527cafc97e5" />

- *Purpose:* Tests whether higher-mileage vehicles are priced lower.
- *Business question:* Does mileage erode price, and does that relationship differ by fuel type or model?
- *Business value:* If a relationship existed, it would inform used-vehicle pricing rules; its absence (see below) tells the team pricing is not currently mileage-driven.
- *Insight:* Price and Mileage show a correlation of **-0.004** — statistically no relationship in this dataset.

**7. KPI Cards Row** (Card Visuals)

<img width="1176" height="115" alt="image" src="https://github.com/user-attachments/assets/c7a8f31c-3a66-45bc-9a90-28f7b15f91f4" />

- *Purpose:* Surfaces the seven KPIs listed above in a persistent, always-visible row.
- *Business question:* "What are the headline numbers right now, given my current filter selection?"
- *Business value:* Gives executives an immediate read without needing to interpret a chart.

---

## Dashboard Filters

<img width="856" height="85" alt="image" src="https://github.com/user-attachments/assets/b525a644-f72e-4e85-8732-50417de6a86a" />

| Slicer | Field | How it improves decision-making |
|---|---|---|
| Year | `Year` | Isolates a specific model year or range to study seasonality and year-over-year change. |
| Model | `Model` | Focuses the entire page on a single BMW nameplate for deep-dive comparisons. |
| Region | `Region` | Filters to a specific market (Asia, Europe, North America, Middle East, South America, Africa) for regional planning. |
| Fuel Type | `Fuel_Type` | Isolates Petrol, Diesel, Hybrid, or Electric performance for powertrain strategy discussions. |
| Transmission | `Transmission` | Compares Manual vs Automatic demand and pricing independently. |

Because every KPI card and chart shares the same data table, applying any one (or combination) of these slicers instantly recalculates the entire page — turning a static report into an ad-hoc analysis tool.

---

## DAX Measures

> The `.pbix` data model stores its DAX in a compressed, non-plain-text format, so the formulas below were reconstructed from the report's field wells, aggregation functions, and cross-checked against the raw CSV — they represent the measure logic actually driving the visuals, expressed in standard DAX syntax.

| Measure | Reconstructed DAX | What it calculates |
|---|---|---|
| Total Cars | `Total Cars = COUNTROWS('BMW sales data (2010-2024) (1)')` | Row count of the sales table |
| Total Sales / Total Quantity | `Total Quantity = SUM('BMW sales data (2010-2024) (1)'[Sales_Volume])` | Sum of all units sold |
| Avg Price | `Avg Price = AVERAGE('BMW sales data (2010-2024) (1)'[Price_USD])` | Average listed price |
| ASV | `ASV = AVERAGE('BMW sales data (2010-2024) (1)'[Sales_Volume])` | Average units sold per record |
| Average Revenue Per Car | `Average Revenue Per Car = DIVIDE(SUMX('BMW sales data (2010-2024) (1)', 'BMW sales data (2010-2024) (1)'[Price_USD] * 'BMW sales data (2010-2024) (1)'[Sales_Volume]), [Total Quantity])` | Total implied revenue divided by total units sold |
| Avg Mileage | `Avg Mileage = AVERAGE('BMW sales data (2010-2024) (1)'[Mileage_KM])` | Average vehicle mileage |
| Dashboard Subtitle | `Dashboard Subtitle = "Showing data for " & SELECTEDVALUE('BMW sales data (2010-2024) (1)'[Model], "All Models") & " | " & SELECTEDVALUE('BMW sales data (2010-2024) (1)'[Region], "All Regions")` (pattern) | Builds filter-aware descriptive text shown below the report title |

---

## Business Insights

1. The dataset contains **50,000 transaction-level records** spanning **11 BMW models** and **2010–2024**, though the dashboard is titled for the 2016–2024 window.
2. Total sales volume across the full dataset is **253,375,734 units**, averaging **5,067.5 units per record**.
3. Total implied revenue (Price × Volume) is roughly **$19.0 trillion** — a figure far beyond real-world BMW sales, confirming this is a synthetically generated dataset built for BI-tool practice rather than actual company financials.
4. Average price per vehicle is **$75,034.60**, and this average is remarkably stable — every model's average price falls within a **~1.5% band** ($74,435–$75,570).
5. **7 Series** generates the highest total revenue ($1.79T), narrowly ahead of **3 Series** ($1.77T) and **i8** ($1.76T) — the top three models are separated by less than 2%.
6. **M3** and **X6** are the lowest revenue-generating models, but even they trail the top model by only about 5–6%, not a dramatic gap.
7. `Sales_Classification` is a **hard-coded threshold rule**, not a percentile split: every record with Sales_Volume ≥ 7,000 is labeled "High," everything below is "Low" — confirmed by inspecting the min/max volume in each bucket.
8. "High" classified records make up **30.5%** of all transactions (15,246 of 50,000) but represent a disproportionate **51.1%** of total units sold (129.6M of 253.4M) — average volume per High record (8,497) is more than double the average for Low records (3,563).
9. **Price and Sales Volume are statistically uncorrelated** (correlation coefficient ≈ 0.00008) — high-volume sales are not being achieved through lower pricing.
10. **Price and Mileage are also uncorrelated** (correlation ≈ -0.004) — higher-mileage vehicles are not discounted relative to lower-mileage ones in this dataset.
11. Regional distribution of transactions is close to perfectly even (16.5%–16.9% share each), with **Asia** narrowly leading total revenue ($3.25T) and **South America** and **Africa** trailing slightly (~$3.11T each) — under 5% separates the top and bottom regions.
12. **Hybrid** leads all fuel types in both total revenue ($4.82T) and total units (64.5M), narrowly ahead of Petrol, Electric, and Diesel — all four sit within roughly 3% of one another.
13. **Electric** vehicles carry the highest average price ($75,276) among fuel types, though the premium over the lowest (Diesel, $75,080) is under 0.3%.
14. **Red** is both the most common color (16.93% of records) and the top revenue-contributing color ($3.21T); **Black** is the least common (16.55%) — the full color spread is under half a percentage point, indicating negligible color-driven demand skew.
15. **Manual transmission** narrowly outsells **Automatic** in both revenue ($9.55T vs $9.46T) and volume (127.4M vs 126.0M units) — a gap of roughly 1%.
16. Year-over-year revenue is essentially flat: 2010 ($1.26T) and 2024 ($1.31T) differ by only ~4%, with **2022 the single highest revenue year** ($1.34T) and **2023 the lowest** ($1.22T) in the 15-year series.
17. Average unit price is stable across the entire time series, ranging narrowly from **$74,050 (2015)** to **$75,544 (2016)** — under a 2% band across 15 years.
18. **Average Revenue per Car ($75,035.77) is nearly identical to Average Price ($75,034.60)** — because price and volume are uncorrelated, revenue per unit tracks list price almost exactly, with no evidence of a volume-driven pricing premium or discount.
19. The 11 tracked models span three product families — sedans (3/5/7 Series), SUVs (X1/X3/X5/X6), M-performance (M3/M5), and electrified (i3/i8) — giving balanced representation across BMW's actual lineup structure.
20. `Engine_Size_L` and `Sales_Classification` exist in the source data but are **not currently used** in any slicer or chart — a clear enrichment opportunity (see Future Enhancements).
21. Every categorical dimension in this dataset (Model, Region, Color, Fuel_Type) is **near-uniformly distributed**, a strong signal that the data was randomly generated to exercise BI tooling rather than to model a real, skewed market — this should be communicated transparently when presenting this project.

---

## Business Recommendations

1. **Pricing Strategy:** Since price shows no relationship to volume or mileage, treat pricing and volume as independent levers — do not assume discounting will move units without a controlled pricing test.
2. **Inventory Planning:** Prioritize stock allocation toward 7 Series, 3 Series, and i8, the current top three revenue and volume performers.
3. **Regional Expansion:** With regional shares this tight, use the Region slicer on a recurring (monthly/quarterly) basis rather than assuming any single region is a runaway leader; incremental investment in Asia — the current narrow leader — is defensible but should be re-validated regularly.
4. **Marketing Focus:** Concentrate campaign spend on Hybrid and Petrol lines, the two leading fuel types by both volume and revenue.
5. **Fuel Type Promotion:** Position Electric models as the premium tier given their highest average price, even though the premium margin is currently thin.
6. **Model Production:** Maintain or increase production commitments for 7 Series and 3 Series; review the business case for continued investment in M3 and X6 given their trailing position.
7. **Seasonal Sales Planning:** Investigate what drove 2022's peak revenue and 2023's trough, and test whether any repeatable seasonal or promotional factor explains the swing.
8. **Customer Segmentation:** Use the existing `Sales_Classification` (High/Low) flag as a ready-made segmentation axis for differentiated retention or upsell campaigns.
9. **Premium Vehicle Strategy:** Build a dedicated premium-tier narrative around Electric and 7 Series, the standout performers on price and revenue respectively.
10. **Revenue Optimization:** Because Average Revenue per Car tracks Average Price almost exactly, revenue growth must come from **volume growth**, not price-mix shifts — plan targets accordingly.
11. **Upselling Opportunities:** Use the Mileage vs Price scatter plot to flag any vehicles priced inconsistently with their mileage/model peers for manual review.
12. **Inventory Optimization:** Cross-reference the Top 5 by Sales and Top 5 by Quantity charts to avoid overstocking high-volume, lower-margin models at the expense of higher-margin ones.
13. **Future Product Planning:** Given M3 and X6's trailing performance, evaluate refreshed variants or repositioning ahead of the next model cycle.
14. **Operational Improvements:** Automate the CSV-to-model refresh (e.g., via Power BI dataflows or a scheduled gateway refresh) rather than manual file replacement, to keep the dashboard current.
15. **Profitability Improvements:** Extend the data model with true cost and margin fields; current KPIs are revenue/volume-based only and cannot yet answer profitability questions.
16. **Data Enrichment:** Incorporate the currently unused `Engine_Size_L` field into a slicer or chart to unlock engine-size-based segmentation.

---

## Key Takeaways

- The dashboard consolidates 50,000 BMW sales records into 8 KPI cards, 5 slicers, and 7 charts on a single, fully cross-filterable page.
- Total sales volume: **253.4M units**; average price: **$75,034.60**; average mileage: **100,307 km**; 11 models tracked.
- 7 Series, 3 Series, and i8 are the top revenue and volume performers, though margins between all models, regions, fuel types, and colors are narrow.
- Price shows no measurable relationship to either sales volume or mileage in this dataset.
- The "High" vs "Low" sales classification is a simple 7,000-unit threshold rule, not a statistical outlier detection.
- The dataset's near-uniform distribution across every categorical field indicates synthetic/randomized data, appropriate for demonstrating BI skills but not for drawing real BMW market conclusions.

---

## Skills Demonstrated

- Power BI
- Power Query
- DAX
- Data Modeling
- Data Visualization
- Business Intelligence
- Data Storytelling
- Interactive Dashboard Design
- KPI Development
- Dashboard Optimization

---

## Technologies Used

- Power BI Desktop
- Power Query
- DAX
- Microsoft Excel
- CSV Dataset

---

## Repository Structure

```
📂 BMW-Sales-Dashboard
│── BWM_Dashboard.pbix
│── BMW_sales_data__2010-2024__1_.csv
│── images
│   └── dashboard.png
│── README.md
```

---

## How to Use

1. Clone or download this repository.
2. Ensure you have **Power BI Desktop** installed (free from Microsoft).
3. Open `BWM_Dashboard.pbix` in Power BI Desktop.
4. If prompted, point the data source to the local path of `BMW_sales_data__2010-2024__1_.csv`.
5. Use the Year, Model, Region, Fuel Type, and Transmission slicers to explore the data interactively.

---

## Future Enhancements

1. Add a forecasting visual (e.g., trend line with confidence interval) for Sales Volume by Year.
2. Introduce drill-through pages for a single-model deep dive (Region, Color, Fuel Type breakdown).
3. Add bookmarks for pre-set views (e.g., "Top Performers," "Regional Overview").
4. Build dynamic, filter-aware tooltip pages for each chart.
5. Design a dedicated mobile layout for on-the-go viewing.
6. Add advanced KPIs such as year-over-year % change and rolling averages.
7. Incorporate Power BI's AI visuals (Key Influencers, Decomposition Tree) to explore what drives High vs Low classification.
8. Add What-if parameters to model hypothetical price or volume changes.
9. Apply a custom Power BI theme file for consistent branding across future reports.
10. Optimize the data model (e.g., star schema with a separate date table) for faster refresh and query performance.

---

## License

This project is licensed under the MIT License.
