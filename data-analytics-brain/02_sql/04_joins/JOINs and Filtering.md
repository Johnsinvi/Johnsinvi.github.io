# JOINs and Filtering

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What is it?
When you combine JOINs with WHERE or conditions in the ON clause, the placement of filters changes what data is returned — especially with LEFT JOINs.

---

## Filtering with WHERE (after join)
```sql
-- Filter applied AFTER joining — some LEFT JOIN rows may be eliminated
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.amount > 100;
-- This effectively turns into an INNER JOIN because NULL amounts fail the WHERE check
```

---

## Filtering in the ON clause (during join)
```sql
-- Filter applied DURING join — all left table rows are preserved
SELECT c.name, o.order_id
FROM customers c
LEFT JOIN orders o
    ON c.customer_id = o.customer_id
    AND o.amount > 100;
-- Customers with no qualifying orders still appear, with NULL in order columns
```

---

## The key rule
| Filter location | Effect on LEFT JOIN |
|----------------|---------------------|
| `WHERE` clause | Can eliminate unmatched LEFT rows (turns into INNER JOIN) |
| `ON` clause | Preserves all left rows; filters what matches from the right |

---

## Common mistakes
- Adding a WHERE filter on the right table's column in a LEFT JOIN — accidentally converts it to an INNER JOIN
- Not realizing the difference until the row counts don't make sense

---

## Interview tip
This is a subtle but important distinction. Being able to explain it shows deep understanding of how joins work.

---

## Connected to
- [[Inner Join]]
- [[Left and Right Join]]
- [[WHERE]]
- [[JOINs and Aggregation]]

---

#flashcards

## Flashcards

What is the difference between filtering in ON vs WHERE with a LEFT JOIN?
?
ON: filter applied during the join — all left rows are preserved, unmatched right rows become NULL.
WHERE: filter applied after the join — can eliminate unmatched left rows, effectively turning it into an INNER JOIN.

If you want to keep all left rows but only match specific right rows, where should the filter go? :: In the ON clause, not the WHERE clause
