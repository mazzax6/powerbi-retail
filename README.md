# Retail KPI Dashboard (Power BI)

One-page executive dashboard showing **Total Sales**, **Gross Profit**, **Margin %**, monthly trend, **Category gross profit**, and a **Region × Channel** performance view with slicers.


## Highlights
- Clear monthly trend with time-intelligence (YoY).
- Category profitability at a glance; matrix for Region × Channel margin.
- Clean star-schema model: fact **Sales** + dim **Dates**; **Dates[Date] 1→* Sales[OrderDate]**.

## Data & Model
- Source: sample retail transactions (Jan 2024–Aug 2025) with Region, Channel, Category, Subcategory, Quantity, UnitPrice, UnitCost, Discount.
- Import mode; `Dates = CALENDARAUTO()` marked as the date table; single-direction filtering.

## Key DAX (selection)
```DAX
Total Sales = SUMX(Sales, Sales[Quantity] * (Sales[UnitPrice] * (1 - Sales[Discount])))
Total Cost  = SUMX(Sales, Sales[Quantity] * Sales[UnitCost])
Gross Profit = [Total Sales] - [Total Cost]
Margin % = DIVIDE([Gross Profit], [Total Sales])
Orders = DISTINCTCOUNT(Sales[OrderID])
AOV = DIVIDE([Total Sales], [Orders])
Sales LY = CALCULATE([Total Sales], DATEADD(Dates[Date], -1, YEAR))
Sales YoY % = VAR t=[Total Sales] VAR l=[Sales LY] RETURN DIVIDE(t-l, l)
