# Logical Operators: AND, OR, NOT

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What are they?
Logical operators combine multiple conditions in a WHERE clause, letting you build more precise filters.

---

## The three operators

### AND — both conditions must be true
```sql
SELECT *
FROM billboard_top_100_year_end
WHERE year = 2000 AND artist_name = 'Destiny''s Child';
```

### OR — at least one condition must be true
```sql
SELECT *
FROM billboard_top_100_year_end
WHERE year = 2000 OR year = 2001;
```

### NOT — reverses the condition
```sql
SELECT *
FROM billboard_top_100_year_end
WHERE NOT year = 2000;
-- Same as: WHERE year \!= 2000
```

---

## Combining them
```sql
SELECT *
FROM billboard_top_100_year_end
WHERE (year = 2000 OR year = 2001)
  AND artist_name = 'Eminem';
```

---

## Order of operations (important\!)
SQL evaluates in this order:
1. NOT
2. AND
3. OR

Use **parentheses** to make your intent explicit and avoid bugs.

---

## Common mistakes
- Forgetting parentheses: `WHERE year = 2000 OR year = 2001 AND rank = 1` is NOT the same as `WHERE (year = 2000 OR year = 2001) AND rank = 1`
- Writing `WHERE year = 2000 OR 2001` — this does NOT work, must repeat the column name

---

## Connected to
- [[WHERE]]
- [[Comparison Operators]]
- [[BETWEEN]]
- [[IN and NOT IN]]

---

#flashcards

## Flashcards

What are the 3 logical operators in SQL? :: AND, OR, NOT

What is the order of operations for logical operators?
?
1. NOT
2. AND
3. OR

What does AND require? :: Both conditions must be TRUE

What does OR require? :: At least one condition must be TRUE

What is wrong with `WHERE year = 2000 OR 2001`? :: You must repeat the column name: WHERE year = 2000 OR year = 2001

Why use parentheses in logical expressions? :: To make the order of evaluation explicit and avoid bugs from default precedence
