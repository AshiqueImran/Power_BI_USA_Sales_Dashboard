# USA Sales Dashboard (Power BI)

An interactive Power BI dashboard analyzing sales and profit performance across US regions, product categories, and time periods, built on a Superstore-style orders dataset.

## Background

Sales volume alone hides a real problem in this data. Central region brings in solid revenue ($501K), but its profit margin sits at just 7.9%, well below South (11.9%), East (13.5%), and West (14.9%). The dashboard is built to surface that gap directly through a dedicated Profit Margin measure, rather than leaving it buried in raw sales numbers.

## Dashboard pages

**Overview**
KPI cards for Sales, Quantity, Profit, Customer, and Order counts, plus Sales by Sub-Category, Sales by Region, Sales by Ship Mode, and Profit by Category. Filterable by Year and Month.

**Data**
A detail table breaking out Sales, Profit, and Quantity by State, filterable by Year and Month.

**Region Drill Through**
A dedicated drillthrough page. Right-click any region on the Overview page to jump here and see that region's Sales, Profit, Quantity, and Profit Margin broken down by Sub-Category and State, with a funnel view by Quantity and a Back button to return.

## Data model

- **Orders** (fact table): order and shipping details, customer info, product info, and financial fields (Sales, Quantity, Discount, COGS, Profit)
- **Region** (dimension table): joined to Orders on Region, many-to-one
- Source: Excel workbook, loaded and typed through Power Query

## DAX

**Calculated columns**
- `Year` = `YEAR(Orders[Order Date])`
- `Month` = `FORMAT(Orders[Order Date], "MMM")`
- `Month Number` = extracted month number from Order Date, used to sort the Month column chronologically instead of alphabetically in the slicer

**Measures**
- `Profit Margin % = DIVIDE(SUM(Orders[Profit]), SUM(Orders[Sales]), 0)`
- `Region Profit Rank = RANKX(ALL(Orders[Region]), [Profit Margin %], , DESC)`

## Features

- Data cleaning and type transformations in Power Query
- Calculated columns for correct chronological month sorting
- Explicit DAX measures for profit margin and regional ranking
- Year and Month slicers across multiple pages
- Drillthrough navigation from Overview into a region-level detail page
- Detail table view for state-level breakdowns

## Tools

Power BI Desktop, Power Query (M), DAX

## Demo

<div align="center">
  <img src="File_Gif_USA_Sales_Power_BI.gif" width="720" alt="App Demo">
</div>

## Files

- `Power_BI_USA_Sales_Dashboard.pbix` — the full report file



