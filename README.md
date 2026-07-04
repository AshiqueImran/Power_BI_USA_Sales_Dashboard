# USA Sales Dashboard (Power BI)

An interactive Power BI dashboard analyzing sales and profit performance across US regions, product categories, and time periods, built on a Superstore-style orders dataset.

## Background

Sales volume alone hides a real problem in this data. Central region brings in solid revenue ($501K), but its profit margin sits at just 7.9%, well below South (11.9%), East (13.5%), and West (14.9%). Drilling further shows the gap is concentrated in Furniture, where several sub-categories (Furnishings, Appliances, Tables, Bookcases) carry 30-45% average discounts and post negative margin as a result. Pulling back to the whole dataset shows why: profit margin holds positive up to roughly 20% discount, then turns negative and worsens sharply beyond that, reaching -78.8% margin at discounts of 50% or higher, a pattern that holds across all four regions, not just Central.

## How to read this dashboard

1. Start on **Overview** for the regional and category-level picture
2. Right-click a Central data point and drill through to **Region Drill Through** to see which sub-categories are actually driving the gap
3. Visit **Discount Impact** to see why those sub-categories are losing money, and confirm the same discount threshold applies everywhere

## Dashboard pages

**Overview**
KPI cards for Sales, Quantity, Profit, Customer, and Order counts, plus Sales by Sub-Category, Sales by Region, Sales by Ship Mode, and Profit by Category. Filterable by Year and Month.

**Data**
A detail table breaking out Sales, Profit, and Quantity by State, filterable by Year and Month.

**Region Drill Through**
A dedicated drillthrough page. Right-click any region on the Overview page to jump here and see that region's Sales, Profit, Quantity, and Profit Margin broken down by Sub-Category and State, with a funnel view by Quantity and a Back button to return. Includes a scatter chart of Avg Discount Rate vs Profit Margin % by Sub-Category, showing which product lines are driving the region's performance.

**Discount Impact**
A bar chart of Profit Margin % across discount buckets (0%, 0-10%, 10-20%, up to 50%+), showing margin turning negative past the 20% discount threshold. A matrix breaks the same view out by Region to confirm the pattern is universal, not specific to any one region.

## Data model

- **Orders** (fact table): order and shipping details, customer info, product info, and financial fields (Sales, Quantity, Discount, COGS, Profit)
- **Region** (dimension table): joined to Orders on Region, many-to-one
- Source: Excel workbook, loaded and typed through Power Query

## DAX

**Calculated columns**
- `Year` = `YEAR(Orders[Order Date])`
- `Month` = `FORMAT(Orders[Order Date], "MMM")`
- `Month Number` = extracted month number from Order Date, used to sort the Month column chronologically instead of alphabetically in the slicer
- `Discount Bucket` = groups each order into a discount range (0%, 0-10%, 10-20%, ... 50%+) using `SWITCH(TRUE(), ...)`
- `Discount Bucket Sort` = numeric sort key so the buckets display in order rather than alphabetically

**Measures**
- `Profit Margin % = DIVIDE(SUM(Orders[Profit]), SUM(Orders[Sales]), 0)`
- `Region Profit Rank = RANKX(ALL(Orders[Region]), [Profit Margin %], , DESC)`
- `Avg Discount Rate = AVERAGE(Orders[Discount])`

## Recommendation

Profit margin holds positive up to roughly 20% discount, then turns negative and worsens sharply beyond that threshold, reaching -78.8% margin at discounts of 50% or higher. This pattern holds across all four regions. Central is hit hardest because its discounting is concentrated in Furniture, where several sub-categories (Furnishings, Appliances, Tables, Bookcases) average 30-45% discounts and post negative margin as a result. Capping discount approval at 20%, starting with these Furniture sub-categories in Central, would likely have the largest single impact on profitability.

## Features

- Data cleaning and type transformations in Power Query
- Calculated columns for chronological month sorting and discount bucketing
- Explicit DAX measures for profit margin, discount rate, and regional ranking
- Year and Month slicers across multiple pages
- Drillthrough navigation from Overview into a region-level detail page
- Scatter chart and matrix visuals connecting discount behavior to profitability
- Detail table view for state-level breakdowns

## Tools

Power BI Desktop, Power Query (M), DAX

## Demo

<div align="center">
  <img src="File_Gif_USA_Sales_Power_BI.gif" width="720" alt="App Demo">
</div>

## Files

- `Power_BI_USA_Sales_Dashboard.pbix` — the full report file

---

Repo: https://github.com/AshiqueImran/Power_BI_USA_Sales_Dashboard