# RANK, DENSE_RANK

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What are they?
Ranking functions that assign a rank to each row based on a sort order — unlike ROW_NUMBER, they handle **ties**.

---

## RANK
Assigns the same rank to tied rows, then **skips** the next rank(s).
```
Scores: 100, 95, 95, 90
RANK:     1,  2,  2,  4   ← rank 3 is skipped
```

## DENSE_RANK
Assigns the same rank to tied rows, but does **not skip** the next rank.
```
Scores: 100, 95, 95, 90
DENSE_RANK: 1,  2,  2,  3   ← no gap
```

---

## Syntax
```sql
RANK() OVER (PARTITION BY group_column ORDER BY sort_column DESC)
DENSE_RANK() OVER (PARTITION BY group_column ORDER BY sort_column DESC)
```

---

## Example
```sql
SELECT
    artist_name,
    year,
    rank AS chart_rank,
    RANK() OVER (PARTITION BY year ORDER BY rank ASC) AS rank_position,
    DENSE_RANK() OVER (PARTITION BY year ORDER BY rank ASC) AS dense_position
FROM billboard_top_100_year_end;
```

---

## Comparison: ROW_NUMBER vs RANK vs DENSE_RANK

| Ties get... | ROW_NUMBER | RANK | DENSE_RANK |
|------------|-----------|------|-----------|
| Same rank? | ❌ No | ✅ Yes | ✅ Yes |
| Next rank skipped? | N/A | ✅ Yes | ❌ No |
| Always unique? | ✅ Yes | ❌ No | ❌ No |

---

## When to use which?
- **ROW_NUMBER** — when you need unique row IDs, even for ties
- **RANK** — when you want to match real-world ranking (Olympic medals — two silvers, no bronze)
- **DENSE_RANK** — when you want no gaps in the ranking sequence

---

## Common mistakes
- Using RANK when you expected no gaps — use DENSE_RANK for gapless ranking
- Forgetting ORDER BY inside OVER — the ranking has no meaning without it

---

## Interview tip
"What's the difference between RANK and DENSE_RANK?" is very common. Use the tied-scores example to explain it clearly.

---

## Connected to
- [[ROW_NUMBER and PARTITION BY]]
- [[Introduction to Window Functions]]

---

#flashcards

## Flashcards

What is the difference between RANK and DENSE_RANK?
?
RANK: gives tied rows the same rank, then SKIPS the next number (1,2,2,4).
DENSE_RANK: gives tied rows the same rank, does NOT skip (1,2,2,3).

What is the difference between ROW_NUMBER and RANK?
?
ROW_NUMBER: always unique, never repeats even for ties.
RANK: repeats for ties, skips next numbers.
DENSE_RANK: repeats for ties, no gaps.

When would you use RANK over DENSE_RANK? :: When you want ranking that matches real-world convention — e.g. two people tied for 2nd place means no 3rd place

When must you always include ORDER BY in RANK/DENSE_RANK? :: Always — ranking is meaningless without a defined order
