# SELECT - FROM

## What it does
The most fundamental SQL query. Retrieves data from a table.

## Syntax
SELECT column1, column2
FROM table_name;

-- To get ALL columns:
SELECT *
FROM table_name;

-- To limit rows returned:
SELECT *
FROM table_name
LIMIT 100;

## Key concepts
- SELECT tells SQL *what* you want to see
- FROM tells SQL *where* to look
- * means "all columns" — useful for exploring, avoid in production
- LIMIT prevents your query from returning millions of rows accidentally

## My first query (billboard dataset)
SELECT *
FROM billboard_top_100_years_end
LIMIT 100;

## Common mistakes
- Forgetting the semicolon at the end
- Writing the table name wrong (case sensitive in some databases)
- Using SELECT * in big tables without LIMIT

## Interview tip
Interviewers often ask: "What's the difference between SELECT * and selecting specific columns?"
Answer: SELECT * is slower and returns unnecessary data. Always select only what you need in real work.

## Related notes
- [[What is SQL]]
- [[07_WHERE]]
---

#flashcards

## Flashcards

What does SELECT do? :: Specifies which columns to return from a query

What does FROM do? :: Specifies which table to query

What does SELECT * mean? :: Returns all columns from the table

What does LIMIT do in a query? :: Restricts the number of rows returned

What is the risk of using SELECT * in production? :: It is slower and returns unnecessary data — always select only the columns you need

Basic clause order for a simple query
?
SELECT → FROM → LIMIT
