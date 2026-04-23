# MIN, MAX, AVG

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What are they?
Three aggregation functions for understanding the range and center of your data.

- **MIN** — the smallest value in a column
- **MAX** — the largest value in a column
- **AVG** — the arithmetic mean (sum ÷ count of non-NULL values)

---

## Syntax
```sql
SELECT
    MIN(column_name),
    MAX(column_name),
    AVG(column_name)
FROM table_name;
```

---

## Examples
```sql
-- Range of years in the billboard dataset
SELECT
    MIN(year) AS earliest_year,
    MAX(year) AS latest_year
FROM billboard_top_100_year_end;

-- Average housing units per year
SELECT
    year,
    AVG(south) AS avg_south_units
FROM us_housing_units
GROUP BY year
ORDER BY year;
```

---

## Key concepts
- All three functions **ignore NULL values**
- MIN and MAX work on text too (returns alphabetical first/last)
- AVG only works on numeric columns
- Results can be combined in the same SELECT

---

## Common mistakes
- Expecting AVG to include NULLs in the denominator — it doesn't. AVG = SUM / COUNT(non-null values)
- Forgetting that MIN/MAX on text returns alphabetical boundaries, which may not be meaningful

---

## Connected to
- [[Introduction to Aggregation]]
- [[COUNT and SUM]]
- [[GROUP BY]]

---

#flashcards

## Flashcards

What does MIN return? :: The smallest value in a column

What does MAX return? :: The largest value in a column

What does AVG return? :: The arithmetic mean — SUM divided by the count of non-NULL values

Does AVG include NULL values in its calculation? :: No — AVG = SUM / COUNT(non-null values). NULLs are excluded from both numerator and denominator.

Does MIN/MAX work on text columns? :: Yes — returns the alphabetically first or last value
