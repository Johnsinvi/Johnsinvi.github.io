# Self-Join

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #advanced
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What is it?
A Self-Join is when a table is joined to **itself**. It's used when a table contains rows that relate to other rows in the same table.

---

## When do you need it?
- Employee–manager relationships (both in the same `employees` table)
- Comparing rows within the same dataset (e.g., year-over-year)
- Finding pairs within the same table

---

## Syntax
You must use **aliases** to distinguish the two "copies" of the same table.

```sql
SELECT a.column, b.column
FROM table_name AS a
JOIN table_name AS b
    ON a.related_column = b.primary_column;
```

---

## Example: Employee → Manager
```sql
-- employees table: employee_id, name, manager_id
SELECT
    e.name AS employee,
    m.name AS manager
FROM employees AS e
LEFT JOIN employees AS m
    ON e.manager_id = m.employee_id;
```

---

## Example: Comparing rows within the same dataset
```sql
-- Find all pairs of songs from the same year
SELECT
    a.song_name AS song_1,
    b.song_name AS song_2,
    a.year
FROM billboard_top_100_year_end AS a
JOIN billboard_top_100_year_end AS b
    ON a.year = b.year
    AND a.song_name < b.song_name;  -- prevents duplicate pairs
```

---

## Key concepts
- Always use aliases — without them SQL can't distinguish the two copies
- Self-joins are conceptually the same as any other join, just on the same table
- Use LEFT JOIN version if you want to keep rows with no match (e.g., employees with no manager)

---

## Common mistakes
- Forgetting aliases — the query won't run or will be ambiguous
- Creating duplicate pairs — add a condition like `a.id < b.id` to avoid (A,B) and (B,A) both appearing

---

## Connected to
- [[Introduction to Joins]]
- [[Inner Join]]
- [[Left and Right Join]]

---

#flashcards

## Flashcards

What is a self-join? :: A join where a table is joined to itself

When do you need a self-join?
?
When rows in a table relate to other rows in the same table — e.g. employee-manager relationships, or comparing rows within the same dataset.

What is required when writing a self-join? :: Table aliases — you must give each "copy" of the table a different alias

How do you avoid duplicate pairs in a self-join?
?
Add a condition like a.id < b.id so that (A,B) and (B,A) don't both appear.
