# Subqueries

**Skill:** #sql
**Topic:** #subqueries
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 5

---

## What is it?
A subquery is a SQL query nested inside another query. It lets you use the result of one query as input for another — building multi-step logic inside a single statement.

---

## Types of subqueries

### 1. In the WHERE clause
Filter based on a calculated value
```sql
-- Artists who charted above average rank
SELECT artist_name, rank
FROM billboard_top_100_year_end
WHERE rank < (SELECT AVG(rank) FROM billboard_top_100_year_end);
```

### 2. In the FROM clause (inline view)
Use a query result as a temporary table
```sql
SELECT year, avg_rank
FROM (
    SELECT year, AVG(rank) AS avg_rank
    FROM billboard_top_100_year_end
    GROUP BY year
) AS yearly_averages
WHERE avg_rank < 50;
```

### 3. In the SELECT clause (scalar subquery)
Calculate a value for every row
```sql
SELECT
    artist_name,
    rank,
    (SELECT AVG(rank) FROM billboard_top_100_year_end) AS overall_avg
FROM billboard_top_100_year_end;
```

---

## Correlated vs Non-correlated subqueries

**Non-correlated** — the subquery runs once independently
```sql
WHERE rank < (SELECT AVG(rank) FROM billboard_top_100_year_end)
```

**Correlated** — the subquery references the outer query and runs once per row (slower)
```sql
WHERE rank < (SELECT AVG(rank) FROM billboard_top_100_year_end b2 WHERE b2.year = b1.year)
```

---

## Key concepts
- Subqueries must be wrapped in parentheses
- Subqueries in FROM must always have an alias
- Non-correlated subqueries are generally more efficient
- CTEs (WITH clause) are often a cleaner alternative to complex subqueries

---

## Common mistakes
- Forgetting the alias when using a subquery in FROM
- Using a correlated subquery where a JOIN would be faster
- Subquery in WHERE returning more than one row when a scalar is expected — causes an error

---

## Interview tip
Interviewers often give you a problem solvable with a subquery or a JOIN — knowing both approaches and their tradeoffs demonstrates strong SQL thinking.

---

## Connected to
- [[WITH - Common Table Expression]]
- [[WHERE]]
- [[Introduction to Joins]]
- [[GROUP BY]]

---

#flashcards

## Flashcards

What is a subquery? :: A SQL query nested inside another query

What are the 3 places you can put a subquery?
?
1. In the WHERE clause — filter based on a calculated value
2. In the FROM clause — use the result as a temporary table (inline view)
3. In the SELECT clause — calculate a scalar value for every row

What is required when using a subquery in the FROM clause? :: It must always have an alias

What is a correlated subquery? :: A subquery that references the outer query and runs once per row — generally slower
<!--SR:!2026-04-19,3,250-->

What is a non-correlated subquery? :: A subquery that runs once independently of the outer query
<!--SR:!2026-04-19,3,250-->

What is a cleaner alternative to deeply nested subqueries? :: CTEs (WITH clause)

What error occurs if a subquery in WHERE returns more than one row when a scalar is expected? :: The query errors — use IN instead of = if multiple rows are possible
