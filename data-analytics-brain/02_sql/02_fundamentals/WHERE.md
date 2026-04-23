# WHERE

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
WHERE filters rows from a table, returning only the rows that match a specified condition. It is the primary tool for narrowing down data.

---

## Syntax
```sql
SELECT column1, column2
FROM table_name
WHERE condition;
```

---

## Example
```sql
-- Get all Billboard entries from the year 2000
SELECT *
FROM billboard_top_100_year_end
WHERE year = 2000;

-- Get housing units data after 1990
SELECT *
FROM us_housing_units
WHERE year > 1990;
```

---

## Key concepts
- WHERE comes **after** FROM and **before** GROUP BY
- The condition must evaluate to TRUE for a row to be included
- You can combine conditions with AND, OR, NOT
- Works with numbers, text, dates, and NULLs (with IS NULL)

---

## Order of clauses so far
```sql
SELECT
FROM
WHERE
LIMIT
```

---

## Common mistakes
- Filtering on a column not in the SELECT list is fine — WHERE works on the full table
- Comparing to NULL with `= NULL` does NOT work — use `IS NULL` instead
- Text comparisons are case-sensitive in some databases

---

## Connected to
- [[Comparison Operators]]
- [[Logical Operators AND OR NOT]]
- [[IS NULL]]
- [[SELECT - FROM]]

---

#flashcards

## Flashcards

What does WHERE do? :: Filters rows, returning only those that match the condition

Where does WHERE go in clause order? :: After FROM, before GROUP BY

Can WHERE filter on a column not in SELECT? :: Yes — WHERE operates on the full table, not just selected columns

Why can't you use WHERE to filter on a NULL value with `= NULL`? :: Because any comparison with NULL using = returns NULL, not TRUE or FALSE — use IS NULL instead

Clause order: SELECT, FROM, WHERE, ORDER BY, LIMIT
?
SELECT → FROM → WHERE → ORDER BY → LIMIT
