# GROUP BY

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What is it?
GROUP BY splits the data into groups based on the values of one or more columns, and applies an aggregate function to each group separately.

Without GROUP BY: one aggregate result for the whole table.
With GROUP BY: one aggregate result **per group**.

---

## Syntax
```sql
SELECT column, AGG_FUNCTION(other_column)
FROM table_name
GROUP BY column;
```

---

## Examples
```sql
-- How many Billboard entries per year?
SELECT year, COUNT(*) AS entries
FROM billboard_top_100_year_end
GROUP BY year
ORDER BY year;

-- Total housing units per year in the South
SELECT year, SUM(south) AS total_south
FROM us_housing_units
GROUP BY year
ORDER BY year DESC;

-- Average rank per artist
SELECT artist_name, AVG(rank) AS avg_rank
FROM billboard_top_100_year_end
GROUP BY artist_name
ORDER BY avg_rank ASC;
```

---

## Full clause order
```sql
SELECT
FROM
WHERE
GROUP BY
ORDER BY
LIMIT
```

---

## Key rule
Any column in your SELECT that is NOT inside an aggregate function **must** appear in GROUP BY.

```sql
-- This works ✓
SELECT year, COUNT(*) FROM billboard_top_100_year_end GROUP BY year;

-- This errors ✗ (year not in GROUP BY but not aggregated)
SELECT year, artist_name, COUNT(*) FROM billboard_top_100_year_end GROUP BY year;
```

---

## Common mistakes
- Selecting a non-aggregated column without including it in GROUP BY
- Trying to filter grouped results with WHERE — use HAVING instead

---

## Interview tip
One of the most tested SQL concepts. Be ready to explain: "What happens if a column is in SELECT but not in GROUP BY and not aggregated?" Answer: it errors (in strict SQL) or returns arbitrary values.

---

## Connected to
- [[Introduction to Aggregation]]
- [[HAVING]]
- [[COUNT and SUM]]
- [[WHERE]]

---

#flashcards

## Flashcards

What does GROUP BY do? :: Splits data into groups and applies an aggregate function to each group separately
<!--SR:!2026-04-19,3,250-->

What is the key rule for columns in SELECT with GROUP BY?
?
Any column in SELECT that is NOT inside an aggregate function MUST appear in GROUP BY.

What is the correct clause order including GROUP BY?
?
SELECT → FROM → WHERE → GROUP BY → ORDER BY → LIMIT

Does GROUP BY collapse rows? :: Yes — it reduces multiple rows into one result per group

How do you filter results after GROUP BY? :: Use HAVING — not WHERE
