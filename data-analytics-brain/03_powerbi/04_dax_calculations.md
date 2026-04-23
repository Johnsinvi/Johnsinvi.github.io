# 🔢 Phase 4 — DAX & Calculations

> **Goal:** Build all measures, calculated columns, and supporting tables needed before touching a single visual.

← Back to [[03_powerbi/03_data_model]]
Next → [[03_powerbi/05_kpis_and_metrics]]

---

## Measures vs Calculated Columns — choose carefully

| | Measures | Calculated Columns |
|---|----------|--------------------|
| Calculated | At query time (dynamic) | At refresh time (static) |
| Context | Responds to filters and slicers | Fixed to the row |
| Storage | No storage cost | Stored in the model |
| Use for | Aggregations, KPIs, ratios | Row-level categories, flags, lookups |
| **Default choice** | ✅ Almost always | Only when you need row context |

> Rule of thumb: if you're summing, counting, averaging, or comparing — use a measure. If you're adding a label or category to each row — use a calculated column.

---

## The `_Measures` table

Keep all your measures in one empty table created in DAX:

```dax
_Measures = DATATABLE("Placeholder", STRING, {{""}})
```

Then right-click → New Measure in that table. This keeps the field list clean and measures easy to find.

---

## Base measures — build these first

Always start with simple base measures before building complex ones on top:

```dax
Total Sales = SUM(FactSales[SalesAmount])

Total Quantity = SUM(FactSales[Quantity])

Total Orders = DISTINCTCOUNT(FactSales[OrderID])

Total Customers = DISTINCTCOUNT(FactSales[CustomerID])

Average Order Value = DIVIDE([Total Sales], [Total Orders], 0)
```

> Always use `DIVIDE()` instead of `/` — it handles division by zero gracefully.

---

## Time intelligence measures

Requires a properly marked calendar table (see [[03_powerbi/03_data_model]]).

```dax
Sales LY = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[Date]))

Sales YoY % = DIVIDE([Total Sales] - [Sales LY], [Sales LY], 0)

Sales MTD = TOTALMTD([Total Sales], DimDate[Date])

Sales YTD = TOTALYTD([Total Sales], DimDate[Date])

Sales QTD = TOTALQTD([Total Sales], DimDate[Date])
```

---

## Conditional and ranking measures

```dax
-- Running total
Sales Running Total = 
CALCULATE(
    [Total Sales],
    FILTER(
        ALL(DimDate[Date]),
        DimDate[Date] <= MAX(DimDate[Date])
    )
)

-- Rank by sales (for top N visuals)
Sales Rank = 
RANKX(ALL(DimProduct[ProductName]), [Total Sales], , DESC, DENSE)

-- Dynamic top N
Top N Products = 
IF([Sales Rank] <= SELECTEDVALUE(_Params[Top N Value], 10), [Total Sales], BLANK())
```

---

## What-if parameters (disconnected tables)

Useful for letting users control thresholds, top N, or scenarios:

1. `Modeling → New Parameter` → creates a disconnected slicer table automatically
2. Reference the parameter value in DAX with `SELECTEDVALUE()`
3. Use it to dynamically filter or change calculations based on the user's selection

---

## Auxiliary tables

Beyond the calendar, you may need:

- **Scenario table** — for comparing budget vs actual vs forecast (unpivoted)
- **Metric selector table** — to let users switch what measure a visual shows dynamically
- **Top N parameter table** — created with what-if parameters

---

## Checklist

- [ ] Created `_Measures` table and stored all measures inside it
- [ ] Built all base measures (sales, quantity, orders, customers, etc.)
- [ ] Built time intelligence measures (LY, YoY, MTD, YTD)
- [ ] All divisions use `DIVIDE()` not `/`
- [ ] Verified measures return correct values against known totals
- [ ] Calculated columns used only where row context is truly needed
- [ ] Calendar table confirmed as Date Table before using time intelligence
- [ ] Named all measures clearly and consistently (Title Case, no abbreviations)

---

## Related

- [[03_powerbi/03_data_model]]
- [[03_powerbi/05_kpis_and_metrics]]
