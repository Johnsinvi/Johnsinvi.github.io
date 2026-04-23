# COUNT, SUM

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## COUNT — how many rows?

### Syntax
```sql
SELECT COUNT(*) FROM table_name;
SELECT COUNT(column_name) FROM table_name;
```

### The key difference
- `COUNT(*)` — counts ALL rows, including NULLs
- `COUNT(column)` — counts only rows where that column is NOT NULL

### Example
```sql
-- How many total entries in the billboard dataset?
SELECT COUNT(*) FROM billboard_top_100_year_end;

-- How many entries have an artist name?
SELECT COUNT(artist_name) FROM billboard_top_100_year_end;
```

---

## SUM — what is the total?

### Syntax
```sql
SELECT SUM(column_name) FROM table_name;
```

### Example
```sql
-- Total housing units built
SELECT SUM(south) FROM us_housing_units;

-- Sum by year (with GROUP BY)
SELECT year, SUM(south) AS total_south
FROM us_housing_units
GROUP BY year;
```

---

## Key concepts
- SUM only works on **numeric** columns
- COUNT works on any column type
- Both ignore NULL values (except COUNT(*))
- Use aliases (`AS`) to give results meaningful names

---

## Common mistakes
- Using SUM on a text column — will error
- Forgetting that COUNT(column) skips NULLs — use COUNT(*) if you want all rows

---

## Interview tip
"What's the difference between COUNT(*) and COUNT(column)?" is a classic interview question. COUNT(*) counts all rows; COUNT(column) skips NULLs.

---

## Connected to
- [[Introduction to Aggregation]]
- [[MIN MAX AVG]]
- [[GROUP BY]]
- [[IS NULL]]

---

#flashcards

## Flashcards

What is the difference between COUNT(*) and COUNT(column)?
?
COUNT(*) counts ALL rows including NULLs.
COUNT(column) counts only rows where that column is NOT NULL.

What does SUM do? :: Adds up all numeric values in a column, ignoring NULLs

Can SUM be used on text columns? :: No — SUM only works on numeric columns

How do you count only rows where artist_name is not NULL?
?
SELECT COUNT(artist_name) FROM billboard_top_100_year_end;

What does AS do in a query? :: Creates an alias — gives the result column a readable name
