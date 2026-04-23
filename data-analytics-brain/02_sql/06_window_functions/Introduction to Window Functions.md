# Introduction to Window Functions

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What are they?
Window functions perform calculations across a set of rows **related to the current row**, without collapsing those rows into a single result like GROUP BY does.

---

## The key difference from GROUP BY

With GROUP BY:
```sql
SELECT year, SUM(south) FROM us_housing_units GROUP BY year;
-- Returns 1 row per year — individual rows are gone
```

With a window function:
```sql
SELECT year, south, SUM(south) OVER (PARTITION BY year) AS year_total
FROM us_housing_units;
-- Returns ALL rows, with the year total added as an extra column
```

GROUP BY collapses rows. Window functions **preserve rows** while adding a calculation alongside each one.

---

## Basic syntax
```sql
AGG_FUNCTION(column) OVER (
    PARTITION BY column   -- divide into groups (optional)
    ORDER BY column       -- define row order within the window
    ROWS/RANGE BETWEEN ... -- define the window frame (optional)
)
```

---

## The OVER clause
`OVER()` is what makes a function a window function. Without it, it's a regular aggregate.

```sql
-- Regular aggregate (collapses rows)
SELECT AVG(rank) FROM billboard_top_100_year_end;

-- Window function (keeps rows)
SELECT artist_name, rank, AVG(rank) OVER() AS overall_avg
FROM billboard_top_100_year_end;
```

---

## My analogy
A window function looks through a "window" at nearby rows and computes something — but it keeps every row in view. GROUP BY closes the window and only shows you the summary.

---

## Window function categories

| Category | Functions |
|----------|----------|
| Ranking | ROW_NUMBER, RANK, DENSE_RANK, NTILE |
| Aggregation | SUM, COUNT, AVG, MIN, MAX (with OVER) |
| Navigation | LAG, LEAD |

---

## Connected to
- [[ROW_NUMBER and PARTITION BY]]
- [[RANK and DENSE_RANK]]
- [[SUM Window Function]]
- [[LAG and LEAD]]
- [[WITH - Common Table Expression]]

---

#flashcards

## Flashcards

What is a window function? :: A function that performs calculations across related rows without collapsing them like GROUP BY does
<!--SR:!2026-04-17,1,230-->

What is the key difference between GROUP BY and window functions?
?
GROUP BY collapses rows into one result per group.
Window functions preserve all rows and add the calculation as an extra column.

What makes a function a window function? :: The OVER() clause — without it, it behaves as a regular aggregate

What are the 3 categories of window functions?
?
1. Ranking: ROW_NUMBER, RANK, DENSE_RANK
2. Aggregation: SUM, COUNT, AVG, MIN, MAX (with OVER)
3. Navigation: LAG, LEAD

What does PARTITION BY do in a window function? :: Divides rows into groups — the window function runs independently within each group

What does ORDER BY inside OVER() do? :: Defines the row order within the window for the calculation
