# Inner Join

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What is it?
An INNER JOIN returns only the rows where there is a match in **both** tables. If a row in either table has no matching row in the other, it is excluded from the result.

---

## Syntax
```sql
SELECT table1.column, table2.column
FROM table1
INNER JOIN table2
    ON table1.key_column = table2.key_column;
```

---

## Example
```sql
-- Join orders with customers (only matched rows)
SELECT
    orders.order_id,
    orders.amount,
    customers.name,
    customers.email
FROM orders
INNER JOIN customers
    ON orders.customer_id = customers.customer_id;
```

---

## Key concepts
- INNER JOIN is the most common type of join
- Only matched rows appear — unmatched rows from either table are dropped
- You can write just `JOIN` — it defaults to INNER JOIN
- Use table aliases to keep queries readable

```sql
-- Using aliases
SELECT o.order_id, c.name
FROM orders AS o
INNER JOIN customers AS c
    ON o.customer_id = c.customer_id;
```

---

## Venn diagram mental model
Think of two overlapping circles. INNER JOIN returns only the **overlapping center** — rows that exist in both.

---

## Common mistakes
- Forgetting the ON clause — causes a cartesian product (every row × every row)
- Not qualifying column names when both tables share a column name — causes ambiguity error

---

## Interview tip
"What's the difference between INNER JOIN and LEFT JOIN?" is very common. INNER keeps only matches; LEFT keeps all left-table rows even without a match.

---

## Connected to
- [[Introduction to Joins]]
- [[Left and Right Join]]
- [[Full Outer Join]]
- [[Why Split Data into Tables]]

---

#flashcards

## Flashcards

What does an INNER JOIN return? :: Only rows that have a match in BOTH tables
<!--SR:!2026-04-19,3,250-->

What happens to unmatched rows in an INNER JOIN? :: They are excluded from the result entirely

What is the shorthand for INNER JOIN? :: Just JOIN — it defaults to INNER JOIN

What does the ON clause do in a JOIN? :: Specifies which columns to match between the two tables

What happens if you forget the ON clause in a JOIN? :: A cartesian product — every row from table A is combined with every row from table B
