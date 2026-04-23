# Aggregates in Window Functions

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What is it?
Standard aggregate functions (SUM, COUNT, AVG, MIN, MAX) can be used as window functions by adding OVER(). This lets you compute group-level aggregates while keeping all individual rows.

---

## Key insight: aggregate vs window aggregate

```sql
-- Regular aggregate — one row per group
SELECT year, AVG(rank) AS avg_rank
FROM billboard_top_100_year_end
GROUP BY year;

-- Window aggregate — all rows kept, group average added as column
SELECT
    year,
    artist_name,
    rank,
    AVG(rank) OVER (PARTITION BY year) AS avg_rank_that_year
FROM billboard_top_100_year_end;
```

---

## Useful patterns

### Compare each row to its group average
```sql
SELECT
    year,
    artist_name,
    rank,
    AVG(rank) OVER (PARTITION BY year) AS year_avg,
    rank - AVG(rank) OVER (PARTITION BY year) AS diff_from_avg
FROM billboard_top_100_year_end;
```

### What % of the year's total does each row represent?
```sql
SELECT
    year,
    south,
    SUM(south) OVER (PARTITION BY year) AS year_total,
    ROUND(south * 100.0 / SUM(south) OVER (PARTITION BY year), 2) AS pct_of_year
FROM us_housing_units;
```

### Count rows in each partition
```sql
SELECT
    year,
    artist_name,
    COUNT(*) OVER (PARTITION BY year) AS songs_that_year
FROM billboard_top_100_year_end;
```

---

## Key concepts
- Any aggregate function + OVER() = window function
- PARTITION BY groups the calculation (like GROUP BY, but without collapsing rows)
- ORDER BY inside OVER makes the aggregate cumulative

---

## Common mistakes
- Mixing window functions and GROUP BY in the same query is not allowed in most databases
- Forgetting that without ORDER BY in OVER, SUM returns the total for the whole partition (not cumulative)

---

## Connected to
- [[Introduction to Window Functions]]
- [[SUM Window Function]]
- [[Window Frame]]
- [[GROUP BY]]

---

#flashcards

## Flashcards

Can standard aggregate functions (SUM, AVG, COUNT) be used as window functions? :: Yes — add OVER() to any aggregate to make it a window function

How do you compare each row to its group average using a window function?
?
SELECT column, value,
  AVG(value) OVER (PARTITION BY column) AS group_avg,
  value - AVG(value) OVER (PARTITION BY column) AS diff_from_avg
FROM table;

Can you mix window functions and GROUP BY in the same query? :: No — most databases do not allow this. Use a CTE to separate the steps.
