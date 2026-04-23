# IN, NOT IN

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
IN checks whether a column's value matches any value in a specified list. It is a cleaner alternative to writing multiple OR conditions.

---

## Syntax
```sql
SELECT *
FROM table_name
WHERE column IN (value1, value2, value3);
```

---

## Examples
```sql
-- Instead of: WHERE year = 2000 OR year = 2001 OR year = 2002
SELECT *
FROM billboard_top_100_year_end
WHERE year IN (2000, 2001, 2002);

-- With text values
SELECT *
FROM billboard_top_100_year_end
WHERE artist_name IN ('Eminem', 'Jay-Z', 'Beyoncé');
```

---

## NOT IN
```sql
-- Exclude specific years
SELECT *
FROM billboard_top_100_year_end
WHERE year NOT IN (2000, 2001, 2002);
```

---

## Key concepts
- IN is equivalent to multiple OR conditions — just much more readable
- Works with numbers, text, and dates
- Can also be used with subqueries: `WHERE id IN (SELECT id FROM other_table)`

---

## Common mistakes
- NOT IN behaves unexpectedly if the list contains NULL — it returns no rows. Use IS NOT NULL to guard against this.
- Forgetting parentheses around the list

---

## Interview tip
If asked to filter by multiple specific values, always use IN instead of chaining ORs — cleaner and easier to maintain.

---

## Connected to
- [[WHERE]]
- [[Logical Operators AND OR NOT]]
- [[Subqueries]]

---

#flashcards

## Flashcards

What does IN do? :: Checks if a column's value matches any value in a specified list

What is IN equivalent to? :: Multiple OR conditions — just more readable

What is the risk of NOT IN when the list contains NULL? :: It returns no rows — always guard against NULLs in the list

How do you filter for years 2000, 2001, and 2002 using IN?
?
WHERE year IN (2000, 2001, 2002)

Can IN be used with a subquery? :: Yes — WHERE id IN (SELECT id FROM other_table)
