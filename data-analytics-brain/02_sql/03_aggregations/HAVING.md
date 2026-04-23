# HAVING

**Skill:** #sql
**Topic:** #aggregations
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 3

---

## What is it?
HAVING filters groups after GROUP BY has been applied. It is the equivalent of WHERE, but for aggregated results.

---

## Syntax
```sql
SELECT column, AGG_FUNCTION(other_column)
FROM table_name
GROUP BY column
HAVING AGG_FUNCTION(other_column) condition;
```

---

## Examples
```sql
-- Artists who appeared more than 5 times on the billboard chart
SELECT artist_name, COUNT(*) AS appearances
FROM billboard_top_100_year_end
GROUP BY artist_name
HAVING COUNT(*) > 5
ORDER BY appearances DESC;

-- Years where average housing units in the South exceeded 100
SELECT year, AVG(south) AS avg_south
FROM us_housing_units
GROUP BY year
HAVING AVG(south) > 100;
```

---

## WHERE vs HAVING — the key difference

| | WHERE | HAVING |
|--|-------|--------|
| Filters | Individual rows | Groups |
| Runs | Before GROUP BY | After GROUP BY |
| Can use aggregates? | ❌ No | ✅ Yes |

```sql
-- WHERE filters rows BEFORE grouping
-- HAVING filters groups AFTER grouping
SELECT year, COUNT(*) AS entries
FROM billboard_top_100_year_end
WHERE year > 1990            -- filters rows first
GROUP BY year
HAVING COUNT(*) > 50         -- then filters groups
ORDER BY year;
```

---

## Common mistakes
- Using WHERE to filter on an aggregate — must use HAVING
- Forgetting that HAVING always comes after GROUP BY

---

## Interview tip
"What's the difference between WHERE and HAVING?" is extremely common. WHERE filters rows before grouping; HAVING filters groups after aggregation.

---

## Connected to
- [[GROUP BY]]
- [[WHERE]]
- [[Introduction to Aggregation]]

---

#flashcards

## Flashcards

What does HAVING do? :: Filters groups after GROUP BY has been applied

What is the difference between WHERE and HAVING?
?
WHERE filters individual rows BEFORE grouping.
HAVING filters groups AFTER aggregation.
WHERE cannot use aggregate functions. HAVING can.

Can you use WHERE to filter on COUNT(*) or SUM()? :: No — you must use HAVING for aggregate conditions

What clause order includes HAVING?
?
SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY → LIMIT
<!--SR:!2026-04-18,2,248-->

Can you use both WHERE and HAVING in the same query? :: Yes — WHERE filters rows first, then GROUP BY groups them, then HAVING filters the groups
<!--SR:!2026-04-17,2,230-->
