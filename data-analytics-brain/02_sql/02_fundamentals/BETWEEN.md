# BETWEEN

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
BETWEEN filters rows where a column value falls within a range — inclusive of both the lower and upper boundary.

---

## Syntax
```sql
SELECT *
FROM table_name
WHERE column BETWEEN low_value AND high_value;
```

---

## Examples
```sql
-- Years between 1990 and 2000 (inclusive)
SELECT *
FROM billboard_top_100_year_end
WHERE year BETWEEN 1990 AND 2000;

-- Housing units data in a range
SELECT *
FROM us_housing_units
WHERE year BETWEEN 1980 AND 2000;
```

---

## Key concepts
- BETWEEN is **inclusive** — both boundary values are included
- Equivalent to: `WHERE column >= low AND column <= high`
- Works on numbers, dates, and text (alphabetical range)

---

## NOT BETWEEN
```sql
-- Exclude the 1990s
SELECT *
FROM billboard_top_100_year_end
WHERE year NOT BETWEEN 1990 AND 1999;
```

---

## Common mistakes
- Thinking BETWEEN is exclusive — it always includes both endpoints
- Writing the high value first: `BETWEEN 2000 AND 1990` returns nothing

---

## Connected to
- [[WHERE]]
- [[Comparison Operators]]

---

#flashcards

## Flashcards

Is BETWEEN inclusive or exclusive of boundary values? :: Inclusive — both the low and high values are included
<!--SR:!2026-04-19,3,250-->

What is BETWEEN equivalent to? :: WHERE column >= low AND column <= high

What happens if you write BETWEEN 2000 AND 1990 (high first)? :: Returns no rows — the low value must come first

Does BETWEEN work on text values? :: Yes — it applies alphabetical ordering
