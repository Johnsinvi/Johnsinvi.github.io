# SUM: Compute a Cumulative Sum

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What is it?
Using SUM as a window function lets you calculate a **running total** — adding up values row by row as you go through the data, rather than summing everything into one number.

---

## Syntax
```sql
SUM(column) OVER (
    PARTITION BY group_column
    ORDER BY order_column
)
```

---

## Example: Running total of housing units by year
```sql
SELECT
    year,
    south,
    SUM(south) OVER (ORDER BY year) AS cumulative_south
FROM us_housing_units;
```
Each row now shows the south value for that year PLUS all previous years combined.

---

## With PARTITION BY — reset the running total per group
```sql
SELECT
    year,
    month,
    south,
    SUM(south) OVER (PARTITION BY year ORDER BY month) AS running_total_by_year
FROM us_housing_units;
-- Running total resets at the start of each year
```

---

## Without ORDER BY — total for the entire partition
```sql
SELECT
    year,
    south,
    SUM(south) OVER (PARTITION BY year) AS year_total
FROM us_housing_units;
-- Every row in the same year shows the full year's total
```

---

## Key concepts
- Without ORDER BY inside OVER: returns the total for the entire window/partition
- With ORDER BY inside OVER: returns a cumulative (running) total
- PARTITION BY resets the running total for each group

---

## Common mistakes
- Confusing ORDER BY inside OVER with ORDER BY at the end of the query — they serve different purposes
- Expecting a cumulative sum without including ORDER BY in the OVER clause

---

## Connected to
- [[Introduction to Window Functions]]
- [[Window Frame]]
- [[ROW_NUMBER and PARTITION BY]]

---

#flashcards

## Flashcards

What does SUM with OVER(ORDER BY) produce? :: A cumulative (running) total — each row shows the sum of all rows up to that point

What does SUM with OVER(PARTITION BY) but no ORDER BY produce? :: The total for the entire partition on every row

What does PARTITION BY do to a running total? :: Resets it at the start of each new group

What is the difference between SUM as an aggregate vs SUM as a window function?
?
Aggregate SUM: collapses rows — returns one number for the group.
Window SUM: preserves rows — adds the sum as an extra column alongside each row.
