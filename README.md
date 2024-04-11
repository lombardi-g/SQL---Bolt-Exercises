# SQL Lessons

Exercises from https://sqlbolt.com/

## SQL Lesson 1

### Tasks

Find the title of each film
Find the director of each film
Find the title and director of each film
Find the title and year of each film
Find all the information about each film

### Answers

```sql
SELECT title FROM movies;
SELECT director FROM movies;
SELECT title, director FROM movies;
SELECT title, year FROM movies;
SELECT * FROM movies;
```
## SQL Lesson 2

### Tasks

Using the right constraints, find the information we need from the Movies table for each task below.

### Answers

```sql
SELECT * FROM movies WHERE id = 6;
SELECT * FROM movies WHERE year > 2000 and year <= 2010;
SELECT * FROM movies WHERE year < 2000 or year > 2010;
SELECT * FROM movies WHERE id between 1 and 5;
```

## SQL Lesson 3

### Tasks 

Here's the definition of a query with a WHERE clause again, go ahead and try and write some queries with the operators above to limit the results to the information we need in the tasks below.

### Answers

```sql
SELECT * FROM movies WHERE Title like "Toy Story%";
SELECT * FROM movies WHERE Director like "John Lasseter";
SELECT * FROM movies WHERE Director not like "John Lasseter";
SELECT * FROM movies WHERE Title like "WALL-%";
```
