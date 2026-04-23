# UNION, UNION ALL

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What are they?
UNION and UNION ALL stack the results of two queries **vertically** (adding rows), unlike JOIN which combines tables horizontally (adding columns).

---

## UNION — no duplicates
```sql
SELECT column FROM table1
UNION
SELECT column FROM table2;
```
Removes duplicate rows from the combined result.

---

## UNION ALL — keeps duplicates
```sql
SELECT column FROM table1
UNION ALL
SELECT column FROM table2;
```
Keeps all rows, including duplicates. Faster than UNION.

---

## Rules
1. Both queries must have the **same number of columns**
2. Columns must have **compatible data types**
3. Column names come from the **first query**

---

## Example
```sql
-- Combine two years of billboard data into one result
SELECT year, song_name, artist_name
FROM billboard_top_100_year_end
WHERE year = 2000

UNION ALL

SELECT year, song_name, artist_name
FROM billboard_top_100_year_end
WHERE year = 2001;
```

---

## Practical use: simulating FULL OUTER JOIN in SQLite
```sql
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id

UNION

SELECT c.name, o.order_id
FROM orders o
LEFT JOIN customers c ON o.customer_id = c.customer_id;
```

---

## UNION vs JOIN

| | UNION | JOIN |
|--|-------|------|
| Direction | Vertical (adds rows) | Horizontal (adds columns) |
| Requirement | Same columns | Matching key column |

---

## Common mistakes
- Using UNION when columns don't match in number or type — will error
- Forgetting that UNION is slower than UNION ALL because it deduplicates

---

## Connected to
- [[Full Outer Join]]
- [[Introduction to Joins]]

---

#flashcards

## Flashcards

What is the difference between UNION and UNION ALL?
?
UNION removes duplicate rows from the combined result.
UNION ALL keeps all rows including duplicates — faster than UNION.

What are the rules for using UNION?
?
1. Both queries must have the same number of columns.
2. Columns must have compatible data types.
3. Column names come from the first query.

What is the difference between UNION and JOIN?
?
UNION stacks results vertically (adds rows).
JOIN combines results horizontally (adds columns).

Which is faster, UNION or UNION ALL? :: UNION ALL — because UNION has to deduplicate results
