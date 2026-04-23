# Introduction to Joins

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #intermediate
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## What are joins?
A JOIN combines rows from two or more tables based on a related column between them. It is how you pull together information that lives in separate tables.

---

## Why do we need joins?
Real databases split data into multiple tables to avoid repetition and keep things organized. A JOIN is how you put them back together when you need information from more than one place.

Example: You have:
- A `songs` table with song info
- An `artists` table with artist info

A JOIN connects them through a shared key (like `artist_id`).

---

## The types of joins

| Join Type | Returns |
|-----------|---------|
| INNER JOIN | Only rows that match in BOTH tables |
| LEFT JOIN | All rows from the left table + matches from right |
| RIGHT JOIN | All rows from the right table + matches from left |
| FULL OUTER JOIN | All rows from both tables |
| SELF JOIN | A table joined to itself |

---

## Key concepts
- Joins always need an ON clause specifying which columns to match
- The column used to connect tables is called a **key**
- A PRIMARY KEY uniquely identifies each row in a table
- A FOREIGN KEY in one table points to the primary key of another

---

## My analogy
Joining two tables is like matching two spreadsheets by a shared ID column. The JOIN type determines what happens when a row in one spreadsheet has no match in the other.

---

## Connected to
- [[Why Split Data into Tables]]
- [[Inner Join]]
- [[Left and Right Join]]
- [[Full Outer Join]]

---

#flashcards

## Flashcards

What does a JOIN do? :: Combines rows from two or more tables based on a related column between them

What are the 5 main types of joins?
?
INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN, SELF JOIN

What is a PRIMARY KEY? :: A column that uniquely identifies each row in a table

What is a FOREIGN KEY? :: A column in one table that points to the primary key of another table
