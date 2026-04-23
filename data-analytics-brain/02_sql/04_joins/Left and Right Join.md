# Left and Right Join

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## LEFT JOIN
Returns **all rows from the left table**, and matching rows from the right table. If there is no match, the right table columns return NULL.

### Syntax
```sql
SELECT table1.column, table2.column
FROM table1
LEFT JOIN table2
    ON table1.key = table2.key;
```

### Example
```sql
-- All customers, even those with no orders
SELECT
    c.name,
    o.order_id,
    o.amount
FROM customers AS c
LEFT JOIN orders AS o
    ON c.customer_id = o.customer_id;
-- Customers with no orders will appear with NULL in order columns
```

---

## RIGHT JOIN
Returns **all rows from the right table**, and matching rows from the left table. Unmatched left rows become NULL.

```sql
SELECT table1.column, table2.column
FROM table1
RIGHT JOIN table2
    ON table1.key = table2.key;
```

---

## Key concepts
- LEFT JOIN is far more common than RIGHT JOIN in practice
- Any RIGHT JOIN can be rewritten as a LEFT JOIN by swapping the table order
- Unmatched rows produce NULLs — use IS NULL to find them

```sql
-- Find customers who have NEVER placed an order
SELECT c.name
FROM customers AS c
LEFT JOIN orders AS o
    ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;
```

---

## Venn diagram mental model
LEFT JOIN = the full left circle + the overlapping center (but not the right-only part)

---

## Common mistakes
- Expecting NULLs to behave like values — remember to use IS NULL, not = NULL
- Accidentally filtering out NULLs with a WHERE clause, turning a LEFT JOIN into an INNER JOIN

---

## Interview tip
If an interviewer asks you to "find records in table A that have no match in table B", use a LEFT JOIN + WHERE right_table.key IS NULL.

---

## Connected to
- [[Inner Join]]
- [[Full Outer Join]]
- [[IS NULL]]
- [[JOINs and Filtering]]

---

#flashcards

## Flashcards

What does a LEFT JOIN return?
?
All rows from the left table, plus matching rows from the right table.
Unmatched right-table columns appear as NULL.

What does a RIGHT JOIN return?
?
All rows from the right table, plus matching rows from the left table.
Unmatched left-table columns appear as NULL.

Which is more commonly used — LEFT JOIN or RIGHT JOIN? :: LEFT JOIN — any RIGHT JOIN can be rewritten as a LEFT JOIN by swapping table order

How do you find rows in table A with NO match in table B?
?
Use LEFT JOIN, then filter WHERE b.key IS NULL

What is the risk of adding a WHERE filter on a right-table column in a LEFT JOIN?
?
It can accidentally turn the LEFT JOIN into an INNER JOIN, because NULL values fail the WHERE condition.
