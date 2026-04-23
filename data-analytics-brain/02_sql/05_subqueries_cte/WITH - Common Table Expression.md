# WITH — Common Table Expression (CTE)

**Skill:** #sql
**Topic:** #subqueries
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 5

---

## What is it?
A CTE (Common Table Expression) defines a temporary named result set using the WITH keyword. It is like giving a subquery a name, making the query much easier to read and maintain.

---

## Syntax
```sql
WITH cte_name AS (
    SELECT ...
    FROM ...
    WHERE ...
)
SELECT *
FROM cte_name;
```

---

## Example
```sql
-- Find artists with an average rank better than 20
WITH artist_avg_rank AS (
    SELECT
        artist_name,
        AVG(rank) AS avg_rank
    FROM billboard_top_100_year_end
    GROUP BY artist_name
)
SELECT artist_name, avg_rank
FROM artist_avg_rank
WHERE avg_rank <= 20
ORDER BY avg_rank;
```

---

## Multiple CTEs
```sql
WITH
top_artists AS (
    SELECT artist_name, COUNT(*) AS appearances
    FROM billboard_top_100_year_end
    GROUP BY artist_name
    HAVING COUNT(*) > 10
),
top_artists_ranked AS (
    SELECT artist_name, appearances,
           RANK() OVER (ORDER BY appearances DESC) AS rnk
    FROM top_artists
)
SELECT * FROM top_artists_ranked WHERE rnk <= 5;
```

---

## CTE vs Subquery — when to use each

| | CTE (WITH) | Subquery |
|--|-----------|---------|
| Readability | ✅ Very readable | ❌ Can get deeply nested |
| Reusability | ✅ Can reference it multiple times | ❌ Must repeat the query |
| Performance | Similar in most databases | Similar |
| Best for | Complex multi-step logic | Simple one-off filters |

---

## Key concepts
- CTEs exist only for the duration of the query — they are not stored
- You can chain multiple CTEs using commas before SELECT
- CTEs make it easy to break complex logic into named steps
- Some databases allow recursive CTEs (for hierarchical data)

---

## Common mistakes
- Forgetting the comma between multiple CTEs
- Trying to use a CTE outside the query it was defined in

---

## Interview tip
Using a CTE instead of deeply nested subqueries shows you write production-quality, readable SQL. Always prefer CTEs when the logic has more than two steps.

---

## Connected to
- [[Subqueries]]
- [[GROUP BY]]
- [[Introduction to Window Functions]]

---

#flashcards

## Flashcards

What is a CTE? :: A temporary named result set defined with the WITH keyword, used to simplify complex queries

What is the syntax for a CTE?
?
WITH cte_name AS (
  SELECT ...
  FROM ...
)
SELECT * FROM cte_name;

How long does a CTE exist? :: Only for the duration of the query — it is not stored

How do you write multiple CTEs in one query? :: Separate them with commas, all before the final SELECT

What is the main advantage of a CTE over a subquery? :: Readability and reusability — you name the logic and can reference it multiple times

When should you prefer a CTE over a subquery? :: When the logic has more than two steps, or when you need to reference the same subquery result more than once
