# 🧩 Phase 3 — Data Model

> **Goal:** Design a clean, efficient schema where tables are correctly related, and every relationship is intentional.

← Back to [[03_powerbi/02_data_sources]]
Next → [[03_powerbi/04_dax_calculations]]

---

## The Star Schema — always aim for this

The star schema is the standard for Power BI models. It separates:

- **Fact tables** — what happened (transactions, events, measures). One row per event. Usually large.
- **Dimension tables** — who, what, where, when. Descriptive context. Usually smaller.

```
         DimDate
            |
DimProduct — FactSales — DimCustomer
            |
         DimStore
```

> Why star schema? It makes DAX simpler, relationships clearer, and the model faster.

---

## Primary keys and foreign keys

- Every **dimension table** must have a unique, non-null **primary key** column
- The **fact table** has **foreign keys** that reference those primary keys
- Primary keys should be a single column (not composite)
- Never relate on columns with nulls or duplicates on the "one" side

> Tip: Always check uniqueness before setting a relationship. In Power Query: `Table → Keep Rows → Keep Duplicates` — if the result is empty, the column is unique.

---

## Relationships

In the Model view, relationships are:

| Property | Recommended setting |
|----------|-------------------|
| Cardinality | Many-to-one (fact → dimension) |
| Cross-filter direction | Single (fact filtered by dimension) |
| Active/Inactive | Active (use inactive only when you need multiple date relationships) |

### Common relationship patterns

- **One active date relationship** from FactTable to DimDate
- If you need multiple date roles (Order Date, Ship Date), create **inactive relationships** and activate them in DAX with `USERELATIONSHIP()`
- **Bridge tables** for many-to-many relationships — avoid direct many-to-many when possible

---

## Tables you'll typically build

| Table | Type | Description |
|-------|------|-------------|
| Fact_Sales (or equivalent) | Fact | Main transactional data |
| Dim_Date | Dimension | Calendar table — always needed |
| Dim_Customer | Dimension | Customer attributes |
| Dim_Product | Dimension | Product catalog |
| Dim_Geography | Dimension | Location data |
| _Measures | Utility | Empty table to store all your measures |
| _Params | Utility | Disconnected table for what-if parameters |

---

## Calendar table — always create one

Even if the source data has date columns, always create a dedicated calendar table. It gives you full control over fiscal years, week numbers, and time intelligence functions.

```dax
DimDate = 
ADDCOLUMNS(
    CALENDAR(DATE(2020,1,1), DATE(2030,12,31)),
    "Year",        YEAR([Date]),
    "Month",       MONTH([Date]),
    "MonthName",   FORMAT([Date], "MMMM"),
    "Quarter",     "Q" & QUARTER([Date]),
    "WeekNum",     WEEKNUM([Date]),
    "DayOfWeek",   FORMAT([Date], "dddd"),
    "IsWeekend",   IF(WEEKDAY([Date],2) >= 6, TRUE, FALSE),
    "YearMonth",   FORMAT([Date], "YYYY-MM")
)
```

Mark it as a Date Table: `Table Tools → Mark as Date Table → Date column`.

---

## Working in PBIR format (Power BI Projects)

When working with a `.pbip` project (the developer-friendly format), the semantic model lives as a folder of text files. Understanding this structure lets you build and modify the model directly in code instead of only through the UI.

### Folder structure

```
project.SemanticModel/
└── definition/
    ├── model.tmdl          ← model-level settings and table references
    ├── database.tmdl       ← compatibility level
    ├── relationships.tmdl  ← ALL relationships in one file
    ├── cultures/
    │   └── en-US.tmdl
    └── tables/
        ├── fact_table.tmdl     ← one file per table
        ├── dim_one.tmdl
        └── dim_calendar.tmdl
```

> Each table has its own `.tmdl` file. Relationships all go in a single `relationships.tmdl` file at the `definition/` level.

---

### model.tmdl — what belongs here

Only model-level settings and `ref` declarations. Do **not** put relationships here.

```tmdl
model Model
	culture: en-US
	defaultPowerBIDataSourceVersion: powerBI_V3
	sourceQueryCulture: en-US
	dataAccessOptions
		legacyRedirects
		returnErrorValuesAsNull

annotation __PBI_TimeIntelligenceEnabled = 1
annotation PBI_ProTooling = ["DevMode"]
annotation PBI_QueryOrder = ["fact_table","dim_one","dim_calendar"]

ref table fact_table
ref table dim_one
ref table dim_calendar

ref cultureInfo en-US
```

---

### relationships.tmdl — correct syntax

Relationships live in their own file using dot notation for columns. No quotes, no nested blocks.

```tmdl
relationship 'relationship_name'
	fromColumn: fact_table.foreign_key_column
	toColumn: dim_table.primary_key_column
```

> `fromColumn` is always the **fact table** side (many). `toColumn` is always the **dimension** side (one).

---

### Table TMDL files — syntax rules

**Column names with spaces must be wrapped in single quotes:**

```tmdl
column 'Team Name'
    dataType: string
    sourceColumn: Team Name
```

**Measure names with spaces must also be wrapped in single quotes:**

```tmdl
measure 'Win%' =
        DIVIDE([Wins], [Games], 0) * 100
```

**Do not use `///` comments** — they are not valid TMDL and will cause a parse error. Use standard single-line comments with `--` only inside DAX expressions if needed.

**Do not use `isKey`** on columns — the key relationship is established via `relationships.tmdl`, not on the column definition.

**The `sourceColumn` value should match exactly** what Power Query outputs as the column name (including spaces), even if the TMDL column declaration uses single quotes.

---

### Minimal working table template

```tmdl
table dim_example
	lineageTag: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

	column id_column
		dataType: string
		lineageTag: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
		summarizeBy: none
		sourceColumn: id_column

		annotation SummarizationSetBy = Automatic

	column 'Display Name'
		dataType: string
		lineageTag: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
		summarizeBy: none
		sourceColumn: Display Name

		annotation SummarizationSetBy = Automatic

	partition dim_example = m
		mode: import
		source =
				let
				    Source = ...
				in
				    Source

	annotation PBI_ResultType = Table
```

> Generate unique lineageTags with any UUID generator. Each object (table, column, measure) needs its own unique tag.

---

### Where to put measures

Measures belong on the **fact table** file, defined between the columns and the partition block. They operate on the rows of that table and are logically grouped there.

```tmdl
	measure 'My Measure' =
			DIVIDE([Base Measure], [Other Measure], 0)
```

---

### Sort by column — text fields with a natural order

Text columns like "Month Year" (e.g., "Oct 2025") sort alphabetically by default, which is wrong. To sort them correctly, create a numeric sort key column and reference it with `sortByColumn`.

If the sort spans multiple years (e.g., a season Oct 2025 → Apr 2026), `Month Number` alone is not enough — use `Year * 100 + Month` as the sort key so ordering is always chronologically correct.

```tmdl
column 'Month Year'
    dataType: string
    lineageTag: ...
    summarizeBy: none
    sourceColumn: Month Year
    sortByColumn: 'Month Sort'

    annotation SummarizationSetBy = Automatic

column 'Month Sort'
    dataType: int64
    isHidden
    formatString: 0
    lineageTag: ...
    summarizeBy: none
    sourceColumn: Month Sort

    annotation SummarizationSetBy = Automatic
```

In Power Query (M), add the sort key column as:
```m
AddMonthSort = Table.AddColumn(PrevStep, "Month Sort",
    each Date.Year([Date]) * 100 + Date.Month([Date]), Int64.Type)
```

> Mark the sort key column `isHidden` so it doesn't appear in the field list.

---

### Relationship join keys — must match exactly

The join key used in `relationships.tmdl` must contain the **same values** on both sides. A common mistake is joining on an ID that is numeric on one side and a text abbreviation on the other — these will never match and all lookups return blank.

**Always verify before finalizing a relationship:**
- Pull both columns into a table visual and check they share values
- If the fact table uses abbreviations (e.g., "BOS") and the dimension uses numeric IDs (e.g., "2"), find the column that actually contains the abbreviation on the dimension side and join on that

---

### Data cleaning — filter non-matching rows in Power Query

If the fact table contains rows that have no match in a dimension (e.g., codes that don't exist in the lookup table), those rows will appear as **(Blank)** in all visuals. Filter them out in Power Query before the data reaches the model.

```m
// Define the valid set of keys from the dimension
ValidKeys = {"KEY1", "KEY2", "KEY3", ...},
// Filter fact rows to only those whose key exists in the valid set
CleanRows = Table.SelectRows(Source, each List.Contains(ValidKeys, [key_column]))
```

> This is cleaner than an inactive relationship or DAX workaround. The filtering happens at load time and keeps the model lean.

---

## Checklist

- [ ] Identified all fact tables and dimension tables
- [ ] Verified primary key uniqueness in all dimension tables
- [ ] All relationships set to many-to-one (fact side is "many")
- [ ] Cross-filter direction is Single on all relationships (unless explicitly needed)
- [ ] Calendar table created and marked as Date Table
- [ ] Calendar table connected to all date columns in fact tables
- [ ] Created a dedicated `_Measures` table (empty, just for organization)
- [ ] Model view is clean — no hidden tables creating confusion
- [ ] No circular relationships
- [ ] (PBIR) Relationships defined in `relationships.tmdl`, not in `model.tmdl`
- [ ] (PBIR) All multi-word column and measure names wrapped in single quotes
- [ ] (PBIR) No `///` comments or `isKey` properties in table files

---

## Related

- [[03_powerbi/02_data_sources]]
- [[03_powerbi/04_dax_calculations]]
