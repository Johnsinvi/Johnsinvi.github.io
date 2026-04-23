# DISTINCT

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What is it?
DISTINCT removes duplicate values from the result, returning only unique values for the specified column(s).

---

## Syntax
```sql
SELECT DISTINCT column_name
FROM table_name;
```

---

## Examples
```sql
-- How many unique years are in the billboard dataset?
SELECT DISTINCT year
FROM billboard_top_100_year_end
ORDER BY year;

-- All unique artists (no repeats)
SELECT DISTINCT artist_name
FROM billboard_top_100_year_end
ORDER BY artist_name;
```

---

## DISTINCT with COUNT
```sql
-- How many unique artists have ever charted?
SELECT COUNT(DISTINCT artist_name) AS unique_artists
FROM billboard_top_100_year_end;
```

---

## Key concepts
- DISTINCT applies to the **entire row** when multiple columns are selected — it deduplicates the combination
- Very useful for exploring what unique values exist in a column
- Used inside COUNT(DISTINCT column) to count unique values

---

## Common mistakes
- Thinking DISTINCT applies only to the first column in a multi-column SELECT — it applies to the whole row
- Overusing DISTINCT to hide data quality issues — better to investigate why duplicates exist

---

## Connected to
- [[COUNT and SUM]]
- [[GROUP BY]]

---

#flashcards

## Flashcards

What does DISTINCT do? :: Removes duplicate values, returning only unique rows

How do you count unique values in a column? :: SELECT COUNT(DISTINCT column_name) FROM table;

Does DISTINCT apply to one column or all selected columns? :: All selected columns — it deduplicates the entire row combination

What is an overuse warning for DISTINCT? :: Using it to hide data quality issues — better to investigate why duplicates exist
