# Window Frame

**Skill:** #sql
**Topic:** #window-functions
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 6

---

## What is it?
A window frame defines exactly which rows are included in the calculation for each row in the window. It gives you fine-grained control over the range of rows a window function considers.

---

## Syntax
```sql
AGG_FUNCTION(column) OVER (
    PARTITION BY ...
    ORDER BY ...
    ROWS BETWEEN start AND end
)
```

---

## Frame boundary keywords

| Keyword | Meaning |
|---------|---------|
| `UNBOUNDED PRECEDING` | From the first row of the partition |
| `N PRECEDING` | N rows before the current row |
| `CURRENT ROW` | The current row |
| `N FOLLOWING` | N rows after the current row |
| `UNBOUNDED FOLLOWING` | To the last row of the partition |

---

## Examples

### Running total (default cumulative behavior)
```sql
SUM(south) OVER (
    ORDER BY year
    ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
)
```

### 3-row moving average (current + 2 previous)
```sql
SELECT
    year,
    south,
    AVG(south) OVER (
        ORDER BY year
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3yr
FROM us_housing_units;
```

### Centered moving average (1 before, current, 1 after)
```sql
AVG(south) OVER (
    ORDER BY year
    ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
)
```

---

## Default frame behavior
When you write ORDER BY inside OVER without specifying a frame, the default is:
`ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
That's why SUM with ORDER BY gives a cumulative total.

Without ORDER BY, the default is:
`ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING`
That's why SUM without ORDER BY gives the full partition total for every row.

---

## Common mistakes
- Not realizing the default frame changes depending on whether ORDER BY is present
- Using RANGE instead of ROWS — RANGE can include non-contiguous rows with tied values

---

## Interview tip
Moving averages (3-month, 7-day, etc.) are a very common analytical task. Window frames are the clean SQL way to compute them.

---

## Connected to
- [[Introduction to Window Functions]]
- [[SUM Window Function]]
- [[Aggregates in Window Functions]]

---

#flashcards

## Flashcards

What is a window frame? :: The specific set of rows included in a window function calculation for each row

What does UNBOUNDED PRECEDING mean? :: From the very first row of the partition

What does CURRENT ROW mean in a window frame? :: The current row being processed

How do you write a 3-row moving average (current + 2 previous)?
?
AVG(column) OVER (
  ORDER BY date_col
  ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
)

What is the default window frame when ORDER BY is present in OVER()?
?
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
(that's why SUM with ORDER BY gives a cumulative total)

What is the default window frame when ORDER BY is NOT present in OVER()?
?
ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
(that's why SUM without ORDER BY gives the full partition total)
<!--SR:!2026-04-17,1,210-->
