# LIKE

**Skill:** #sql
**Topic:** #fundamentals
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 2

---

## What is it?
LIKE performs pattern matching on text columns. It lets you search for partial matches using wildcard characters instead of exact values.

---

## Wildcards

| Wildcard | Meaning |
|----------|---------|
| `%` | Any sequence of characters (including none) |
| `_` | Exactly one character |

---

## Syntax
```sql
SELECT *
FROM table_name
WHERE column LIKE 'pattern';
```

---

## Examples
```sql
-- Artist names starting with 'J'
SELECT * FROM billboard_top_100_year_end
WHERE artist_name LIKE 'J%';

-- Artist names ending with 'son'
SELECT * FROM billboard_top_100_year_end
WHERE artist_name LIKE '%son';

-- Artist names containing 'an' anywhere
SELECT * FROM billboard_top_100_year_end
WHERE artist_name LIKE '%an%';

-- Five-letter artist names
SELECT * FROM billboard_top_100_year_end
WHERE artist_name LIKE '_____';
```

---

## NOT LIKE
```sql
-- Exclude anything with 'feat' in the title
SELECT * FROM billboard_top_100_year_end
WHERE song_name NOT LIKE '%feat%';
```

---

## Key concepts
- LIKE is case-insensitive in most databases (SQLite, PostgreSQL with ILIKE)
- % is the most commonly used wildcard
- Combine with NOT for exclusion patterns

---

## Common mistakes
- Forgetting the quotes around the pattern
- Using `=` instead of `LIKE` when trying to do pattern matching

---

## Connected to
- [[WHERE]]
- [[IN and NOT IN]]

---

#flashcards

## Flashcards

What does the % wildcard mean in LIKE? :: Any sequence of characters (including none)

What does the _ wildcard mean in LIKE? :: Exactly one character

How do you find all artists whose name starts with 'J'? :: WHERE artist_name LIKE 'J%'

How do you find all artists whose name contains 'an'? :: WHERE artist_name LIKE '%an%'

How do you exclude patterns with LIKE? :: Use NOT LIKE — e.g. WHERE song_name NOT LIKE '%feat%'
