# Why Do We Split Data into Separate Tables?

**Skill:** #sql
**Topic:** #joins
**Difficulty:** #beginner
**Source:** Tuan Vu — SQL for Data Analytics (Udemy) — Section 4

---

## The problem with one big table
Imagine storing everything in a single table: customer info, orders, products, shipping details — all in one place.

Problems:
- **Duplication**: Every order repeats the customer's name, address, email
- **Update anomalies**: Change a customer's address → need to update hundreds of rows
- **Inconsistency**: Easy to have slightly different versions of the same data

---

## The solution: Normalization
Split the data into focused tables, each about one thing, connected by keys.

| Table | Contains |
|-------|---------|
| `customers` | Customer info (name, address, email) |
| `orders` | Order info (date, amount, customer_id) |
| `products` | Product info (name, price, category) |

Now if a customer changes their address, you update it in **one place only**.

---

## How tables connect: Keys

**Primary Key** — uniquely identifies each row in a table
```
customers.customer_id  →  unique for every customer
```

**Foreign Key** — a column in one table that refers to the primary key of another
```
orders.customer_id  →  points back to customers.customer_id
```

---

## Why this matters for joins
Because data is split across tables, you need JOINs to bring it back together for analysis. Understanding WHY the data is split helps you understand how to JOIN it correctly.

---

## Connected to
- [[Introduction to Joins]]
- [[Inner Join]]

---

#flashcards

## Flashcards

Why do databases split data into multiple tables? :: To avoid duplication, prevent update anomalies, and keep data consistent

What is normalization? :: The process of organizing a database into focused tables connected by keys

What problem does normalization solve?
?
If customer info is repeated in every order row, updating one address requires changing hundreds of rows. With separate tables, you update it once.
