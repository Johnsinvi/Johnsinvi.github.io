# 🗄️ Phase 2 — Data Sources

> **Goal:** Connect to the data, understand its format, assess its quality, and get it into a clean, usable state.

← Back to [[03_powerbi/01_client_discovery]]
Next → [[03_powerbi/03_data_model]]

---

## Common data source types

| Source | Connector in Power BI | Notes |
|--------|----------------------|-------|
| Excel / CSV | Excel, Text/CSV | Most common. Watch for merged cells, multi-row headers |
| SQL Server | SQL Server | Direct query or import mode |
| PostgreSQL / MySQL | PostgreSQL, MySQL | Requires gateway for scheduled refresh |
| SharePoint / OneDrive | SharePoint Folder | Good for files that update regularly |
| Google Sheets | Web connector or native | May need authentication |
| REST API | Web / custom connector | JSON or XML response |
| Cloud DBs (BigQuery, Snowflake) | Native connectors | Best for large data |

---

## Import Mode vs DirectQuery

| | Import Mode | DirectQuery |
|---|-------------|-------------|
| Speed | Fast (data in memory) | Slower (queries live DB) |
| Refresh | Scheduled or manual | Always current |
| Data size | Limited by RAM | No limit |
| DAX support | Full | Some limitations |
| **When to use** | Most cases | When data must be real-time or very large |

> Default recommendation: **Import Mode** unless the client explicitly needs real-time data.

---

## Assessing data quality

Before building anything, look at the raw data and ask:

- Are there **missing values** in key columns? How should they be handled?
- Are there **duplicates**? At what grain — row level, ID level?
- Are **dates formatted correctly**? (Text vs Date type is a common issue)
- Are **numeric fields stored as text**? (Can't aggregate until fixed)
- Are there **inconsistent categories**? (e.g., "Male", "male", "M" all meaning the same thing)
- Are there **outliers or impossible values**? (Negative quantities, future dates in past data)

---

## Data cleaning in Power Query

Power Query is where you clean before loading into the model. Common transformations:

```
- Remove empty rows and columns
- Promote first row as headers
- Change column data types explicitly
- Replace values (standardize categories)
- Remove duplicates
- Filter out test or irrelevant records
- Split columns (e.g., "Full Name" → "First Name" + "Last Name")
- Merge or append queries
- Add custom columns for derived fields
- Rename columns to clean, consistent names (no spaces if possible)
```

> **Best practice:** Never modify the source file. Do all transformations in Power Query so the process is repeatable and auditable.

---

## Unpivoting — when source data is "wide" and needs to be "long"

Some source tables store what should be multiple rows as multiple columns. A common example is an events table where each event has two participants (e.g., a game with a home team and an away team), both in the same row. For analysis you need one row per participant — otherwise every measure requires duplicate logic for each side.

**When to unpivot:** any time a single row represents two or more instances of the same entity playing the same role (home/away, team A/team B, before/after).

**Pattern in Power Query (M):**

```m
// Build the "home" perspective
HomeRows = Table.SelectColumns(Source, {"id","Date","TeamA","TeamA Score","TeamB Score"}),
HomeRenamed = Table.RenameColumns(HomeRows, {
    {"TeamA", "team_id"},
    {"TeamA Score", "Points Scored"},
    {"TeamB Score", "Points Allowed"}
}),
HomeWithLocation = Table.AddColumn(HomeRenamed, "Location", each "Home"),

// Build the "away" perspective
AwayRows = Table.SelectColumns(Source, {"id","Date","TeamB","TeamB Score","TeamA Score"}),
AwayRenamed = Table.RenameColumns(AwayRows, {
    {"TeamB", "team_id"},
    {"TeamB Score", "Points Scored"},
    {"TeamA Score", "Points Allowed"}
}),
AwayWithLocation = Table.AddColumn(AwayRenamed, "Location", each "Away"),

// Combine both into one long table
Combined = Table.Combine({HomeWithLocation, AwayWithLocation})
```

This results in a clean fact table where every measure (win rate, points scored, etc.) is a single calculation that works for both home and away — just filtered by the `Location` column.

> Add a derived `Win` column after combining: `each if [Points Scored] > [Points Allowed] then 1 else 0`

---

## How data will be received

Document this per project:

- **Delivery method:** Direct connection / file export / email / shared folder
- **Format:** .xlsx / .csv / database table / API
- **Frequency:** One-time / daily / weekly / on request
- **Owner:** Who provides or maintains this data?
- **Refresh schedule:** How often should the dashboard update?

---

## Checklist

- [ ] Connected to all data sources successfully
- [ ] Identified the grain (one row = one what?) of each table
- [ ] Assessed null rates in key columns
- [ ] Fixed data type issues (dates, numbers)
- [ ] Standardized categorical values
- [ ] Removed duplicates where needed
- [ ] Renamed all columns to clean, readable names
- [ ] Documented source, format, frequency, and owner for each table
- [ ] Confirmed refresh schedule with client

---

## Related

- [[03_powerbi/01_client_discovery]]
- [[03_powerbi/03_data_model]]
