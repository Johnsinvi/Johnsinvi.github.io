# ORDER BY

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
ORDER BY sorts the result set by one or more columns, either ascending or descending.

---

## Syntax
```sql
SELECT column1, column2
FROM table_name
ORDER BY column1 ASC;   -- or DESC
```

---

## Examples
```sql
-- Sort billboard data by year, newest first
SELECT *
FROM billboard_top_100_year_end
ORDER BY year DESC;

-- Sort by rank within each year
SELECT *
FROM billboard_top_100_year_end
ORDER BY year DESC, rank ASC;

-- Sort housing units data oldest to newest
SELECT *
FROM us_housing_units
ORDER BY year ASC;
```

---

## Key concepts
- Default sort order is **ASC** (ascending) — you don't need to write it
- **DESC** sorts from highest to lowest (Z→A, 9→0, newest→oldest)
- You can sort by **multiple columns** — SQL applies them left to right
- ORDER BY can reference columns not in the SELECT list
- ORDER BY goes **after WHERE** and **before LIMIT**

---

## Full clause order so far
```sql
SELECT
FROM
WHERE
ORDER BY
LIMIT
```

---

## Common mistakes
- Forgetting that ORDER BY comes before LIMIT — if you swap them, the query errors
- Assuming SQL returns rows in any predictable order without ORDER BY — it does not

---

## Interview tip
Without an ORDER BY clause, row order is not guaranteed. Never assume data will come back sorted just because it looks that way in a small test.

---

## Connected to
- [[SELECT - FROM]]
- [[LIMIT]]
- [[WHERE]]

---

#flashcards

## Flashcards

What is the default sort order of ORDER BY? :: ASC (ascending)

How do you sort from highest to lowest? :: Use DESC — ORDER BY column DESC

Can ORDER BY sort by multiple columns? :: Yes — applied left to right

Can ORDER BY reference a column not in SELECT? :: Yes

What is the correct clause order including ORDER BY?
?
SELECT → FROM → WHERE → ORDER BY → LIMIT

Is row order guaranteed in SQL without ORDER BY? :: No — never assume data will come back in any predictable order without ORDER BY
