# IS NULL

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
NULL represents a missing or unknown value in a database. IS NULL is the only correct way to check if a value is NULL.

---

## Syntax
```sql
-- Find rows with missing values
SELECT *
FROM table_name
WHERE column IS NULL;

-- Find rows where value exists
SELECT *
FROM table_name
WHERE column IS NOT NULL;
```

---

## Example
```sql
-- Find records with no artist name
SELECT *
FROM billboard_top_100_year_end
WHERE artist_name IS NULL;

-- Only return complete records
SELECT *
FROM billboard_top_100_year_end
WHERE artist_name IS NOT NULL;
```

---

## Key concepts
- NULL is NOT the same as 0, empty string '', or 'NULL' as text
- NULL means the value is **unknown** — it's not a value itself
- Any comparison with NULL using `=` returns NULL (not TRUE or FALSE)
- This is why `WHERE column = NULL` **never works**

---

## Why NULL matters for data analysts
- Real-world data has missing values everywhere
- Aggregations like AVG and SUM ignore NULLs by default
- Joins can produce NULLs in unmatched rows (Left Join)
- Always check for NULLs when exploring a new dataset

---

## Common mistakes
- Writing `WHERE column = NULL` — this always returns 0 rows
- Forgetting that COUNT(*) counts NULLs but COUNT(column) does not

---

## Connected to
- [[WHERE]]
- [[COUNT and SUM]]
- [[Left and Right Join]]

---

#flashcards

## Flashcards

What is NULL in SQL? :: A missing or unknown value — it is not zero, not empty string, not 'NULL' text

Why does `WHERE column = NULL` never work? :: Because any comparison with NULL using = returns NULL, not TRUE — use IS NULL instead

What is the correct syntax to find missing values? :: WHERE column IS NULL

What is the correct syntax to exclude missing values? :: WHERE column IS NOT NULL

Does COUNT(*) count NULL rows? :: Yes — COUNT(*) counts all rows including NULLs

Does COUNT(column) count NULL rows? :: No — COUNT(column) skips NULLs
