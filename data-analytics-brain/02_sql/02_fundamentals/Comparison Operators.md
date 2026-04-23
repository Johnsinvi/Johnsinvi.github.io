# Comparison Operators

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What are they?
Comparison operators are used inside WHERE clauses to compare a column's value against another value. They return TRUE or FALSE for each row.

---

## The operators

| Operator | Meaning |
|----------|---------|
| `=` | Equal to |
| `\!=` or `<>` | Not equal to |
| `>` | Greater than |
| `<` | Less than |
| `>=` | Greater than or equal to |
| `<=` | Less than or equal to |

---

## Examples
```sql
-- Equal
SELECT * FROM billboard_top_100_year_end WHERE year = 2005;

-- Not equal
SELECT * FROM billboard_top_100_year_end WHERE year \!= 2005;

-- Greater than
SELECT * FROM us_housing_units WHERE year > 2000;

-- Less than or equal
SELECT * FROM us_housing_units WHERE year <= 1990;
```

---

## Key concepts
- `\!=` and `<>` do the same thing — both mean "not equal"
- Operators work on numbers, dates, and text
- Text comparisons follow alphabetical order (`'B' > 'A'` is TRUE)

---

## Common mistakes
- Using `=` to check for NULL — must use `IS NULL` instead
- Forgetting quotes around text values: `WHERE name = Taylor` fails, use `WHERE name = 'Taylor'`

---

## Connected to
- [[WHERE]]
- [[Logical Operators AND OR NOT]]
- [[IS NULL]]

---

#flashcards

## Flashcards

What are the 6 SQL comparison operators? :: =, \!=, <>, <, >, <=, >=

What is the difference between \!= and <>? :: Nothing — both mean "not equal to"

How do you check for NULL values in SQL? :: Use IS NULL — never use = NULL

If you write `WHERE name = Taylor` (no quotes), what happens? :: The query errors — text values must be wrapped in single quotes: WHERE name = 'Taylor'
<!--SR:!2026-04-20,4,270-->

Does `'B' > 'A'` evaluate to TRUE in SQL? :: Yes — text comparisons follow alphabetical order
