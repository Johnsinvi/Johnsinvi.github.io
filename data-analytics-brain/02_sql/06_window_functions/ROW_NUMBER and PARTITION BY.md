# ROW_NUMBER, PARTITION BY

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## ROW_NUMBER
Assigns a unique sequential integer to each row within its window, starting at 1.

### Syntax
```sql
ROW_NUMBER() OVER (
    PARTITION BY group_column
    ORDER BY sort_column
)
```

### Example: Number rows overall
```sql
SELECT
    artist_name,
    year,
    rank,
    ROW_NUMBER() OVER (ORDER BY rank ASC) AS row_num
FROM billboard_top_100_year_end;
```

### Example: Number rows within each year
```sql
SELECT
    year,
    artist_name,
    rank,
    ROW_NUMBER() OVER (PARTITION BY year ORDER BY rank ASC) AS rank_within_year
FROM billboard_top_100_year_end;
-- Row 1 in each year = the #1 song that year
```

---

## PARTITION BY
Divides rows into groups (partitions) — the window function resets and runs independently within each partition.

- Without PARTITION BY → entire result set is one window
- With PARTITION BY → each unique value of the partition column creates a separate window

---

## Common use: Get the top N rows per group
```sql
-- Top 3 songs per year
WITH ranked AS (
    SELECT
        year,
        artist_name,
        song_name,
        ROW_NUMBER() OVER (PARTITION BY year ORDER BY rank ASC) AS rn
    FROM billboard_top_100_year_end
)
SELECT * FROM ranked WHERE rn <= 3;
```
This is one of the most useful patterns in SQL analytics.

---

## Key concepts
- ROW_NUMBER always produces unique numbers, even for ties
- For ties that should share the same rank, use RANK or DENSE_RANK
- PARTITION BY is not exclusive to ROW_NUMBER — it works with all window functions

---

## Common mistakes
- Confusing PARTITION BY (window function grouping) with GROUP BY (which collapses rows)
- Using ROW_NUMBER for ranking when ties should share the same rank

---

## Interview tip
"Get the top N records per group" is one of the most common SQL interview questions. The answer is ROW_NUMBER() with PARTITION BY + a CTE to filter.

---

## Connected to
- [[Introduction to Window Functions]]
- [[RANK and DENSE_RANK]]
- [[WITH - Common Table Expression]]

---

#flashcards

## Flashcards

What does ROW_NUMBER() do? :: Assigns a unique sequential integer to each row within its window, starting at 1

Does ROW_NUMBER handle ties? :: No — it always assigns unique numbers, even to tied rows

What is the most common use of ROW_NUMBER + PARTITION BY?
?
Getting the top N rows per group — e.g. the top 3 songs per year

What is the difference between PARTITION BY and GROUP BY?
?
PARTITION BY: used in window functions — divides rows into groups without collapsing them.
GROUP BY: collapses rows into one result per group.

How do you get the #1 song per year using ROW_NUMBER?
?
WITH ranked AS (
  SELECT year, song_name,
    ROW_NUMBER() OVER (PARTITION BY year ORDER BY rank ASC) AS rn
  FROM billboard_top_100_year_end
)
SELECT * FROM ranked WHERE rn = 1;
