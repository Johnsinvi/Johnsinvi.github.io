# LAG, LEAD

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What are they?
Navigation functions that let you access the value of a column from a **previous row** (LAG) or a **following row** (LEAD) relative to the current row — without a self-join.

---

## LAG — look back
```sql
LAG(column, offset, default) OVER (PARTITION BY ... ORDER BY ...)
```
- `column` — the column to look back into
- `offset` — how many rows back (default: 1)
- `default` — value to return if no previous row exists (default: NULL)

### Example
```sql
-- Compare this year's housing units to the previous year
SELECT
    year,
    south,
    LAG(south, 1) OVER (ORDER BY year) AS prev_year_south,
    south - LAG(south, 1) OVER (ORDER BY year) AS year_over_year_change
FROM us_housing_units;
```

---

## LEAD — look ahead
```sql
LEAD(column, offset, default) OVER (PARTITION BY ... ORDER BY ...)
```

### Example
```sql
-- Show next year's value alongside the current year
SELECT
    year,
    south,
    LEAD(south, 1) OVER (ORDER BY year) AS next_year_south
FROM us_housing_units;
```

---

## With PARTITION BY — reset per group
```sql
-- Year-over-year change within each region
SELECT
    year,
    region,
    units,
    LAG(units) OVER (PARTITION BY region ORDER BY year) AS prev_year,
    units - LAG(units) OVER (PARTITION BY region ORDER BY year) AS change
FROM housing_by_region;
```

---

## Key concepts
- Without a default value, the first row (for LAG) or last row (for LEAD) returns NULL
- Extremely useful for **year-over-year**, **month-over-month**, or any sequential comparisons
- Much cleaner than a self-join for this type of analysis

---

## Common mistakes
- Forgetting ORDER BY inside OVER — LAG/LEAD are meaningless without a defined order
- Forgetting to handle NULL for the first/last row when using the result in calculations

---

## Interview tip
"How would you calculate month-over-month growth?" — LAG is the answer. It's a pattern that shows up constantly in business analytics.

---

## Connected to
- [[Introduction to Window Functions]]
- [[Window Frame]]
- [[SUM Window Function]]
- [[Self-Join]]

---

#flashcards

## Flashcards

What does LAG do? :: Accesses the value from a previous row relative to the current row

What does LEAD do? :: Accesses the value from a following row relative to the current row

What is the syntax for LAG?
?
LAG(column, offset, default) OVER (PARTITION BY ... ORDER BY ...)
offset = how many rows back (default 1)
default = value if no previous row exists (default NULL)

How do you calculate year-over-year change using LAG?
?
SELECT year, value,
  LAG(value, 1) OVER (ORDER BY year) AS prev_year,
  value - LAG(value, 1) OVER (ORDER BY year) AS change
FROM table;

Is LAG/LEAD cleaner than a self-join for sequential comparisons? :: Yes — much cleaner and more readable

What does LAG/LEAD return for the first/last row in the window? :: NULL (unless you specify a default value as the third argument)

Is ORDER BY required inside OVER() for LAG/LEAD? :: Yes — without it the result is meaningless
