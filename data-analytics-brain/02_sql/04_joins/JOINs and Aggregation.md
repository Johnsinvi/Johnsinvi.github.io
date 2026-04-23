# JOINs and Aggregation

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What is it?
Combining JOINs with GROUP BY and aggregate functions to summarize data from multiple tables. This is one of the most powerful patterns in SQL analytics.

---

## The pattern
```sql
SELECT t1.category_column, AGG_FUNCTION(t2.value_column)
FROM table1 AS t1
JOIN table2 AS t2 ON t1.key = t2.key
GROUP BY t1.category_column;
```

---

## Examples
```sql
-- Total sales per customer
SELECT
    c.name,
    COUNT(o.order_id) AS total_orders,
    SUM(o.amount) AS total_spent
FROM customers AS c
LEFT JOIN orders AS o
    ON c.customer_id = o.customer_id
GROUP BY c.name
ORDER BY total_spent DESC;

-- Average rank per artist on the billboard chart
SELECT
    a.genre,
    AVG(b.rank) AS avg_chart_rank
FROM artists AS a
JOIN billboard_top_100_year_end AS b
    ON a.artist_name = b.artist_name
GROUP BY a.genre
ORDER BY avg_chart_rank ASC;
```

---

## Important: COUNT(*) vs COUNT(column) after a LEFT JOIN
```sql
-- COUNT(*) includes NULL rows (customers with no orders)
-- COUNT(o.order_id) skips customers with no orders
SELECT c.name, COUNT(o.order_id) AS actual_orders
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
GROUP BY c.name;
```

---

## Common mistakes
- Using COUNT(*) after a LEFT JOIN when you only want to count matched rows — use COUNT(right_table.column) instead
- Forgetting that GROUP BY must include all non-aggregated SELECT columns

---

## Connected to
- [[Inner Join]]
- [[Left and Right Join]]
- [[GROUP BY]]
- [[COUNT and SUM]]

---

#flashcards

## Flashcards

What is the pattern for joining tables and then aggregating?
?
JOIN the tables → GROUP BY a category column → apply AGG_FUNCTION on value column

After a LEFT JOIN, what is the difference between COUNT(*) and COUNT(right_table.column)?
?
COUNT(*) includes NULL rows (unmatched left rows).
COUNT(right_table.column) skips NULL rows — only counts actual matches.
