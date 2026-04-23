# LIMIT

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
LIMIT restricts the number of rows returned by a query. It is one of the most important habits to develop — always use it when exploring unfamiliar tables.

---

## Syntax
```sql
SELECT *
FROM table_name
LIMIT 100;
```

---

## Example
```sql
-- Preview the billboard dataset without pulling all rows
SELECT *
FROM billboard_top_100_year_end
LIMIT 10;
```

---

## Key concepts
- LIMIT always goes at the **end** of the query
- It does NOT affect which rows are selected — just how many are shown
- Combined with ORDER BY, you can get the top N results

```sql
-- Top 5 most recent entries
SELECT *
FROM billboard_top_100_year_end
ORDER BY year DESC
LIMIT 5;
```

---

## Common mistakes
- Forgetting LIMIT when exploring large tables — can accidentally return millions of rows
- Thinking LIMIT filters data — it only cuts off the display

---

## Interview tip
LIMIT is simple, but using it reflexively on large tables signals good habits to interviewers and teammates.

---

## Connected to
- [[SELECT - FROM]]
- [[ORDER BY]]

---

#flashcards

## Flashcards

What does LIMIT do? :: Restricts the number of rows returned by a query

Where does LIMIT go in a query? :: At the very end, after ORDER BY

Does LIMIT affect which rows are selected? :: No — it only cuts off how many rows are displayed

How do you get the top 5 results by year (newest first)?
?
SELECT * FROM table ORDER BY year DESC LIMIT 5;
