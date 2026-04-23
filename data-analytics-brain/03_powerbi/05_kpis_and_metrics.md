# 🎯 Phase 5 — KPIs & Metrics

> **Goal:** Define exactly what information the dashboard will communicate, and what questions each visual must answer.

← Back to [[03_powerbi/04_dax_calculations]]
Next → [[03_powerbi/06_design_and_layout]]

---

## Why define this before designing

It's tempting to open Power BI and start dragging visuals. Don't. Every visual should exist to answer a specific question. If you can't state the question, you don't need the visual.

---

## The framework: question → metric → visual

For every piece of information on the dashboard, write:

1. **Business question** — "What is the client actually asking?"
2. **Metric** — "Which measure answers it?"
3. **Dimension** — "How should it be broken down?" (by time, by product, by region…)
4. **Visual type** — "What's the best chart for this?" (decided in Phase 6)

---

## Common KPI categories

### Sales & Revenue
- Total Revenue / Total Sales
- Revenue vs Target / Budget
- Revenue growth YoY, MoM
- Average Order Value (AOV)
- Revenue by product, category, region, channel

### Volume & Activity
- Number of orders / transactions
- Units sold / quantity
- Number of active customers
- New vs returning customers

### Efficiency & Operations
- Conversion rate
- Return rate / refund rate
- Fulfillment time / lead time
- Cost per order

### Time-based
- Sales MTD, QTD, YTD
- Comparison to same period last year
- Trend over rolling N days/months

---

## KPI definition template

Use this format to document each KPI before building:

```
KPI Name:         Total Revenue
Business Question: How much revenue did we generate in the selected period?
Measure:          [Total Sales]
Breakdown:        By Month, by Category, by Region
Target/Goal:      Monthly target from budget table
Comparison:       vs Last Year (SAMEPERIODLASTYEAR)
Owner:            Sales Manager
```

---

## Prioritizing what to show

Not everything can (or should) be on one page. Use this filter:

- **Must have** — the client asked for it explicitly, or it drives the main business decision
- **Should have** — useful context that supports the main KPIs
- **Nice to have** — interesting but not critical; goes on a secondary page or a drill-through

> If a visual doesn't answer a question that someone in the room actually asks, cut it.

---

## Checklist

- [ ] Listed every KPI/metric the client mentioned in discovery
- [ ] Written the business question behind each one
- [ ] Confirmed which DAX measures cover each KPI (from Phase 4)
- [ ] Defined how each metric will be broken down (dimensions)
- [ ] Identified any comparison needed (YoY, vs target, vs average)
- [ ] Prioritized into must-have / should-have / nice-to-have
- [ ] Decided which metrics go on which page

---

## Related

- [[03_powerbi/01_client_discovery]]
- [[03_powerbi/04_dax_calculations]]
- [[03_powerbi/06_design_and_layout]]
