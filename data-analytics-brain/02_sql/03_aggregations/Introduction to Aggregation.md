# Introduction to Aggregation

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What is it?
Aggregation means summarizing many rows of data into a single value. Instead of looking at individual rows, you ask questions like "how many?", "what's the total?", or "what's the average?".

---

## Why it matters for data analytics
Raw data is just rows. Aggregation turns rows into **insights**.

Without aggregation:
> You see 10,000 individual Billboard chart entries.

With aggregation:
> You see that Eminem had the most #1 hits in the 2000s.

---

## The SQL aggregation functions

| Function | What it does |
|----------|-------------|
| `COUNT` | Counts rows or non-NULL values |
| `SUM` | Adds up numeric values |
| `MIN` | Returns the smallest value |
| `MAX` | Returns the largest value |
| `AVG` | Returns the average (mean) |

---

## Key concepts
- Aggregate functions collapse multiple rows into one result
- They are typically used with **GROUP BY** to aggregate by category
- Without GROUP BY, they aggregate the entire table into one row
- Most aggregation functions **ignore NULL values** automatically

---

## Connected to
- [[COUNT and SUM]]
- [[MIN MAX AVG]]
- [[GROUP BY]]
- [[HAVING]]

---

#flashcards

## Flashcards

What does aggregation mean in SQL? :: Summarizing many rows of data into a single value

What are the 5 SQL aggregation functions? :: COUNT, SUM, MIN, MAX, AVG

Do aggregate functions ignore NULL values? :: Yes — almost all aggregation functions ignore NULLs automatically (except COUNT(*))

What is the difference between using an aggregate with and without GROUP BY?
?
Without GROUP BY: one result for the entire table.
With GROUP BY: one result per group.
