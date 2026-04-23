# Full Outer Join

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What is it?
A FULL OUTER JOIN returns **all rows from both tables**. Where there is a match, columns from both tables are populated. Where there is no match, the missing side returns NULL.

---

## Syntax
```sql
SELECT table1.column, table2.column
FROM table1
FULL OUTER JOIN table2
    ON table1.key = table2.key;
```

---

## Example
```sql
-- All customers AND all orders, matched where possible
SELECT
    c.name,
    o.order_id,
    o.amount
FROM customers AS c
FULL OUTER JOIN orders AS o
    ON c.customer_id = o.customer_id;
-- Customers with no orders: order columns are NULL
-- Orders with no customer: customer columns are NULL
```

---

## Finding unmatched rows from both sides
```sql
SELECT c.name, o.order_id
FROM customers c
FULL OUTER JOIN orders o
    ON c.customer_id = o.customer_id
WHERE c.customer_id IS NULL OR o.customer_id IS NULL;
```

---

## Venn diagram mental model
FULL OUTER JOIN = **both full circles** — every row from both tables, matched where possible, NULL where not.

---

## Key concepts
- Less common in everyday analytics but critical for finding data gaps
- SQLite does NOT support FULL OUTER JOIN natively — you can simulate it with UNION of LEFT JOIN and RIGHT JOIN
- Very useful for data reconciliation and auditing

---

## Common mistakes
- Confusing FULL OUTER JOIN with CROSS JOIN — CROSS JOIN produces every possible combination (cartesian product); FULL OUTER JOIN matches on a key

---

## Connected to
- [[Inner Join]]
- [[Left and Right Join]]
- [[UNION and UNION ALL]]
- [[IS NULL]]

---

#flashcards

## Flashcards

What does a FULL OUTER JOIN return? :: All rows from BOTH tables — matched where possible, NULL where not

Does SQLite support FULL OUTER JOIN natively? :: No — simulate it with a UNION of LEFT JOIN and RIGHT JOIN

How do you find unmatched rows from BOTH tables?
?
Use FULL OUTER JOIN, then filter WHERE left.key IS NULL OR right.key IS NULL

What is the Venn diagram mental model for FULL OUTER JOIN? :: Both full circles — every row from both tables
