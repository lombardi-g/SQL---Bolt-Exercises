# SQL Lessons

Exercises from https://sqlbolt.com/

## SQL Lesson 1
SELECT queries 101
### Tasks

To retrieve data from a SQL database, we need to write SELECT statements, which are often colloquially refered to as queries. A query in itself is just a statement which declares what data we are looking for, where to find it in the database, and optionally, how to transform it before it is returned. It has a specific syntax though, which is what we are going to learn in the following exercises.

```sql
SELECT column, another_column, …
FROM mytable;
```

### Answers


    Find the title of each film
    Find the director of each film
    Find the title and director of each film
    Find the title and year of each film
    Find all the information about each film


```sql
SELECT title FROM movies;
SELECT director FROM movies;
SELECT title, director FROM movies;
SELECT title, year FROM movies;
SELECT * FROM movies;
```
## SQL Lesson 2
Queries with constraints (Pt. 1) 
### Tasks

Now we know how to select for specific columns of data from a table, but if you had a table with a hundred million rows of data, reading through all the rows would be inefficient and perhaps even impossible.

In order to filter certain results from being returned, we need to use a WHERE clause in the query. The clause is applied to each row of data by checking specific column values to determine whether it should be included in the results or not.
```sql
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

### Answers


    Find the movie with a row id of 6
    Find the movies released in the years between 2000 and 2010
    Find the movies not released in the years between 2000 and 2010
    Find the first 5 Pixar movies and their release year


```sql
SELECT * FROM movies WHERE id = 6;
SELECT * FROM movies WHERE year > 2000 and year <= 2010;
SELECT * FROM movies WHERE year < 2000 or year > 2010;
SELECT * FROM movies WHERE id between 1 and 5;
```

## SQL Lesson 3
Queries with constraints (Pt. 2

### Tasks 

When writing WHERE clauses with columns containing text data, SQL supports a number of useful operators to do things like case-insensitive string comparison and wildcard pattern matching. We show a few common text-data specific operators below:
```sql
SELECT column, another_column, …
FROM mytable
WHERE condition
    AND/OR another_condition
    AND/OR …;
```

| Operator | Condition | Example |
| --- | --- | --- |
| = | Case sensitive exact string comparison (notice the single equals) | col_name = "abc"|
| != or <> | Case sensitive exact string inequality comparison |	col_name != "abcd"|
| LIKE | Case insensitive exact string comparison | col_name LIKE "ABC" |
| NOT LIKE | Case insensitive exact string inequality comparison |	col_name NOT LIKE "ABCD" |
| % | Used anywhere in a string to match a sequence of zero or more characters (only with LIKE or NOT LIKE) | col_name LIKE "%AT%" (matches "AT", "ATTIC", "CAT" or even "BATS")|
| _ | Used anywhere in a string to match a single character (only with LIKE or NOT LIKE) |	col_name LIKE "AN_"
(matches "AND", but not "AN") |
| IN (...) | String exists in a list |	col_name IN ("A", "B", "C") |
| NOT IN (...) | String does not exist in a list |	col_name NOT IN ("D", "E", "F") |


### Answers


    Find all the Toy Story movies
    Find all the movies directed by John Lasseter
    Find all the movies (and director) not directed by John Lasseter
    Find all the WALL-* movies

```sql
SELECT * FROM movies WHERE Title like "Toy Story%";
SELECT * FROM movies WHERE Director like "John Lasseter";
SELECT * FROM movies WHERE Director not like "John Lasseter";
SELECT * FROM movies WHERE Title like "WALL-%";
```

## SQL Lesson 4
Filtering and sorting Query results

### Tasks

Even though the data in a database may be unique, the results of any particular query may not be – take our Movies table for example, many different movies can be released the same year. In such cases, SQL provides a convenient way to discard rows that have a duplicate column value by using the DISTINCT keyword.
Select query with unique results
```sql
SELECT DISTINCT column, another_column, …
FROM mytable
WHERE condition(s);
```

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```
### Answers

There are a few concepts in this lesson, but all are pretty straight-forward to apply. To spice things up, we've gone and scrambled the Movies table for you in the exercise to better mimic what kind of data you might see in real life. Try and use the necessary keywords and clauses introduced above in your queries.


    List all directors of Pixar movies (alphabetically), without duplicates
    List the last four Pixar movies released (ordered from most recent to least)
    List the first five Pixar movies sorted alphabetically
    List the next five Pixar movies sorted alphabetically


```sql
SELECT DISTINCT director FROM movies ORDER BY director ASC;
SELECT title, year FROM movies ORDER BY year DESC LIMIT 4;
SELECT title, year FROM movies ORDER BY title ASC LIMIT 5;
SELECT title, year FROM movies ORDER BY title ASC LIMIT 5 OFFSET 5;
```

## Review
Simple SELECT Queries

### Task
You've done a good job getting to this point! Now that you've gotten a taste of how to write a basic query, you need to practice writing queries that solve actual problems.

```sql
SELECT column, another_column, …
FROM mytable
WHERE condition(s)
ORDER BY column ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

### Answers
Try and write some queries to find the information requested in the tasks you know. You may have to use a different combination of clauses in your query for each task. Once you're done, continue onto the next lesson to learn about queries that span multiple tables.

    List all the Canadian cities and their populations
    Order all the cities in the United States by their latitude from north to south
    List all the cities west of Chicago, ordered from west to east
    List the two largest cities in Mexico (by population)
    List the third and fourth largest cities (by population) in the United States and their population

```sql
SELECT city, country, population FROM north_american_cities WHERE country = "Canada";
SELECT * FROM north_american_cities WHERE country = "United States" ORDER BY latitude DESC;
SELECT * FROM north_american_cities WHERE longitude < -87.629798 ORDER BY longitude ASC;
SELECT * FROM north_american_cities WHERE country = "Mexico" ORDER BY population DESC LIMIT 2;
SELECT * FROM north_american_cities WHERE country = "United States" ORDER BY population DESC LIMIT 2 OFFSET 2;
```
