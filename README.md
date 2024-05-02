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

## SQL Lesson 10
Queries with aggregates (Pt. 1)

### Tasks

In addition to the simple expressions that we introduced last lesson, SQL also supports the use of aggregate expressions (or functions) that allow you to summarize information about a group of rows of data.

```sql
SELECT AGG_FUNC(column_or_expression) AS aggregate_description, …
FROM mytable
WHERE constraint_expression;
```
| Function | Description |
| --- | --- |
| COUNT(*), COUNT(column) | A common function used to counts the number of rows in the group if no column name is specified. Otherwise, count the number of rows in the group with non-NULL values in the specified column. |
| MIN(column) | Finds the smallest numerical value in the specified column for all rows in the group. |
| MAX(column) | Finds the largest numerical value in the specified column for all rows in the group. |
| AVG(column) | Finds the average numerical value in the specified column for all rows in the group. |
| Sum(column) | Finds the sum of all numerical values in the specified column for the rows in the group.|

### Answers

For this exercise, we are going to work with our Employees table. Notice how the rows in this table have shared data, which will give us an opportunity to use aggregate functions to summarize some high-level metrics about the teams.

    Find the longest time that an employee has been at the studio
    For each role, find the average number of years employed by employees in that role
    Find the total number of employee years worked in each building

```sql
SELECT role, MAX(years_employed) FROM employees;
SELECT role, AVG(years_employed) FROM employees GROUP BY role;
SELECT building, SUM(years_employed) FROM employees GROUP BY building;
```

## SQL Lesson 11
Queries with aggregates (Pt. 2)

### Tasks

Our queries are getting fairly complex, but we have nearly introduced all the important parts of a SELECT query. One thing that you might have noticed is that if the GROUP BY clause is executed after the WHERE clause (which filters the rows which are to be grouped), then how exactly do we filter the grouped rows?
Luckily, SQL allows us to do this by adding an additional HAVING clause which is used specifically with the GROUP BY clause to allow us to filter grouped rows from the result set.

```sql
SELECT group_by_column, AGG_FUNC(column_expression) AS aggregate_result_alias, …
FROM mytable
WHERE condition
GROUP BY column
HAVING group_condition;
```

### Answers

For this exercise, you are going to dive deeper into Employee data at the film studio. Think about the different clauses you want to apply for each task.

    Find the number of Artists in the studio (without a HAVING clause)
    Find the number of Employees of each role in the studio
    Find the total number of years employed by all Engineers

```sql
SELECT COUNT(name) FROM employees WHERE role LIKE "artist";
SELECT role, COUNT(name) AS employees FROM employees GROUP BY role;
SELECT role, SUM(years_employed) FROM employees GROUP BY role HAVING role LIKE "engineer";
```

## SQL Lesson 12
Order of execution of a Query

### Tasks

Now that we have an idea of all the parts of a query, we can now talk about how they all fit together in the context of a complete query.

```sql
SELECT DISTINCT column, AGG_FUNC(column_or_expression), …
FROM mytable
    JOIN another_table
      ON mytable.column = another_table.column
    WHERE constraint_expression
    GROUP BY column
    HAVING constraint_expression
    ORDER BY column ASC/DESC
    LIMIT count OFFSET COUNT;
```
Query order of execution: FROM and JOIN, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, ORDER BY, LIMIT/OFFSET.

### Answers

Here ends our lessons on SELECT queries, congrats of making it this far! This exercise will try and test your understanding of queries, so don't be discouraged if you find them challenging. Just try your best.

    Find the number of movies each director has directed
    Find the total domestic and international sales that can be attributed to each director

```sql
SELECT director, COUNT(Id) FROM movies GROUP BY director;
SELECT director, SUM(domestic_sales)+ SUM(international_sales) as total_sales FROM movies LEFT JOIN boxoffice ON Id = movie_id GROUP BY director;
```

## SQL Lesson 13
Inserting rows

### Tasks

We've spent quite a few lessons on how to query for data in a database, so it's time to start learning a bit about SQL schemas and how to add new data.

```sql
INSERT INTO mytable
(column, another_column, …)
VALUES (value_or_expr, another_value_or_expr, …),
      (value_or_expr_2, another_value_or_expr_2, …),
      …;
```

### Answers

In this exercise, we are going to play studio executive and add a few movies to the Movies to our portfolio. In this table, the Id is an auto-incrementing integer, so you can try inserting a row with only the other columns defined. 


    Add the studio's new production, Toy Story 4 to the list of movies (you can use any director)
    Toy Story 4 has been released to critical acclaim! It had a rating of 8.7, and made 340 million domestically and 270 million internationally. Add the record to the BoxOffice table.

```sql
INSERT INTO movies VALUES (4, "Toy Story 4", null, null, null);
INSERT INTO boxoffice VALUES (4, 8.7, 340000000, 270000000);
```

## SQL Lesson 14
Updating rows

### Tasks

In addition to adding new data, a common task is to update existing data, which can be done using an UPDATE statement. Similar to the INSERT statement, you have to specify exactly which table, columns, and rows to update. In addition, the data you are updating has to match the data type of the columns in the table schema.

```sql
UPDATE mytable
SET column = value_or_expr, 
    other_column = another_value_or_expr, 
    …
WHERE condition;
```

### Answers
It looks like some of the information in our Movies database might be incorrect, so go ahead and fix them through the exercises below.

    The director for A Bug's Life is incorrect, it was actually directed by John Lasseter
    The year that Toy Story 2 was released is incorrect, it was actually released in 1999
    Both the title and director for Toy Story 8 is incorrect! The title should be "Toy Story 3" and it was directed by Lee Unkrich

```sql
UPDATE movies SET director = 'John Lasseter' WHERE id = 2;
UPDATE movies SET year = 1999 WHERE title = 'Toy Story 2';
UPDATE movies SET title = 'Toy Story 3', director = 'Lee Unkrich' WHERE title = 'Toy Story 8';
```
