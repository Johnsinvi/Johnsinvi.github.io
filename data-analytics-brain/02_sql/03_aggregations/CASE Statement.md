# CASE Statement

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What is it?
CASE is SQL's version of if/else logic. It evaluates conditions row by row and returns a value depending on which condition is met.

---

## Syntax
```sql
CASE
    WHEN condition1 THEN value1
    WHEN condition2 THEN value2
    ELSE default_value
END AS alias_name
```

---

## Examples
```sql
-- Label billboard entries by era
SELECT
    year,
    song_name,
    CASE
        WHEN year < 1980 THEN '70s or earlier'
        WHEN year BETWEEN 1980 AND 1989 THEN '80s'
        WHEN year BETWEEN 1990 AND 1999 THEN '90s'
        WHEN year >= 2000 THEN '2000s or later'
        ELSE 'Unknown'
    END AS era
FROM billboard_top_100_year_end;

-- Classify housing growth
SELECT
    year,
    south,
    CASE
        WHEN south > 300 THEN 'High'
        WHEN south BETWEEN 150 AND 300 THEN 'Medium'
        ELSE 'Low'
    END AS growth_level
FROM us_housing_units;
```

---

## CASE with aggregation
```sql
-- Count songs by era
SELECT
    CASE
        WHEN year < 1990 THEN 'Pre-90s'
        ELSE '90s and later'
    END AS era,
    COUNT(*) AS song_count
FROM billboard_top_100_year_end
GROUP BY era;
```

---

## Key concepts
- Conditions are evaluated in order — SQL stops at the first TRUE condition
- ELSE is optional but good practice to catch unexpected values
- Always close with END
- Give it an alias with AS for readable output

---

## Common mistakes
- Forgetting END at the close of the CASE expression
- Not including ELSE — rows that match no condition return NULL

---

## Interview tip
CASE is powerful in interviews because it shows you can add business logic inside SQL without needing Python or a separate step.

---

## Connected to
- [[GROUP BY]]
- [[WHERE]]
- [[Introduction to Aggregation]]

---

#flashcards

## Flashcards

What is a CASE statement in SQL? :: SQL's version of if/else logic — evaluates conditions row by row and returns a value

What keyword closes a CASE expression? :: END

What happens if no CASE condition matches and there is no ELSE? :: The row returns NULL

In what order does SQL evaluate CASE conditions? :: Top to bottom — it stops at the first TRUE condition

How do you combine CASE with GROUP BY?
?
Use CASE in SELECT to create a category column, then GROUP BY that column to aggregate by category.

What is the syntax structure of CASE?
?
CASE
  WHEN condition1 THEN value1
  WHEN condition2 THEN value2
  ELSE default_value
END AS alias
