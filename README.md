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

## SQL Lesson 6
Multi-table queries with JOINs

### Tasks
Up to now, we've been working with a single table, but entity data in the real world is often broken down into pieces and stored across multiple orthogonal tables using a process known as normalization
```sql
SELECT column, another_table_column, …
FROM mytable
INNER JOIN another_table 
    ON mytable.id = another_table.id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

### Answers

We've added a new table to the Pixar database so that you can try practicing some joins. The BoxOffice table stores information about the ratings and sales of each particular Pixar movie, and the Movie_id column in that table corresponds with the Id column in the Movies table 1-to-1. Try and solve the tasks below using the INNER JOIN introduced above.

    Find the domestic and international sales for each movie
    Show the sales numbers for each movie that did better internationally rather than domestically
    List all the movies by their ratings in descending order

```sql
SELECT * FROM movies INNER JOIN boxoffice ON id = movie_id;
SELECT * FROM movies INNER JOIN boxoffice ON id = movie_id WHERE international_sales > domestic_sales;
SELECT * FROM movies INNER JOIN boxoffice ON id = movie_id ORDER BY rating desc;
```

## SQL Lesson 7
OUTER JOINs

### Tasks

Depending on how you want to analyze the data, the INNER JOIN we used last lesson might not be sufficient because the resulting table only contains data that belongs in both of the tables.
If the two tables have asymmetric data, which can easily happen when data is entered in different stages, then we would have to use a LEFT JOIN, RIGHT JOIN or FULL JOIN instead to ensure that the data you need is not left out of the results.
```sql
SELECT column, another_column, …
FROM mytable
INNER/LEFT/RIGHT/FULL JOIN another_table 
    ON mytable.id = another_table.matching_id
WHERE condition(s)
ORDER BY column, … ASC/DESC
LIMIT num_limit OFFSET num_offset;
```

### Answers
In this exercise, you are going to be working with a new table which stores fictional data about Employees in the film studio and their assigned office Buildings. Some of the buildings are new, so they don't have any employees in them yet, but we need to find some information about them regardless.
Since our browser SQL database is somewhat limited, only the LEFT JOIN is supported in the exercise below.

    Find the list of all buildings that have employees
    Find the list of all buildings and their capacity
    List all buildings and the distinct employee roles in each building (including empty buildings)

```sql
SELECT DISTINCT building FROM employees;
SELECT * FROM buildings;
SELECT DISTINCT building_name, role FROM buildings LEFT JOIN employees ON building_name = building;
```

## SQL Lesson 8
A short note on NULLs

### Tasks

It's always good to reduce the possibility of NULL values in databases because they require special attention when constructing queries, constraints (certain functions behave differently with null values) and when processing the results.

An alternative to NULL values in your database is to have data-type appropriate default values, like 0 for numerical data, empty strings for text data, etc. But if your database needs to store incomplete data, then NULL values can be appropriate if the default values will skew later analysis (for example, when taking averages of numerical data).

```sql
SELECT column, another_column, …
FROM mytable
WHERE column IS/IS NOT NULL
AND/OR another_condition
AND/OR …;
```

### Answers

This exercise will be a sort of review of the last few lessons. We're using the same Employees and Buildings table from the last lesson, but we've hired a few more people, who haven't yet been assigned a building.

    Find the name and role of all employees who have not been assigned to a building
    Find the names of the buildings that hold no employees

```sql
SELECT name, role FROM employees WHERE building IS NULL;
SELECT building_name FROM buildings LEFT JOIN employees ON building_name = building WHERE building IS NULL;
```

## SQL Lesson 9
Queries with expressions

### Tasks

In addition to querying and referencing raw column data with SQL, you can also use expressions to write more complex logic on column values in a query. These expressions can use mathematical and string functions along with basic arithmetic to transform values when the query is executed, as shown in this physics example.

```sql
SELECT particle_speed / 2.0 AS half_particle_speed
FROM physics_data
WHERE ABS(particle_position) * 10.0 > 500;
```

###  Answers

You are going to have to use expressions to transform the BoxOffice data into something easier to understand for the tasks below.

    List all movies and their combined sales in millions of dollars
    List all movies and their ratings in percent
    List all movies that were released on even number years

```sql
SELECT title, (domestic_sales+international_sales)/1000000 AS total_sales FROM movies LEFT JOIN boxoffice ON id = boxoffice.movie_id;
SELECT title, rating*10 AS PercentRating FROM movies LEFT JOIN boxoffice ON id = boxoffice.movie_id;
SELECT title, year as evenyear FROM movies LEFT JOIN boxoffice ON id = boxoffice.movie_id WHERE year % 2 = 0;
```
