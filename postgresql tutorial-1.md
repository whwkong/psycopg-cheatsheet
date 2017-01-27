# postgreSQL tutorial-1

<https://www.postgresql.org/docs/8.0/static/tutorial.html>

The db associated with this tutorial is: 
testdb.postgres.org.tutorial.  

## Creating a New Table
<https://www.postgresql.org/docs/8.0/static/tutorial-table.html>

    CREATE TABLE weather (
        city            varchar(80),
        temp_lo         int,           -- low temperature
        temp_hi         int,           -- high temperature
        prcp            real,          -- precipitation
        date            date
    );

    CREATE TABLE cities (
        name            varchar(80),
        location        point
    );

## Populating a Table With Rows
<https://www.postgresql.org/docs/8.0/static/tutorial-populate.html>

    \dt     - list tables within db

Insert using fixed positional arguments.

    INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');
    INSERT INTO cities VALUES ('San Francisco', '(-194.0, 53.0)');

Insert using keyword format.  

    INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
        VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');

You can list the columns in a different order if you wish or even omit some  
columns, e.g., if the precipitation is unknown:

    INSERT INTO weather (date, city, temp_hi, temp_lo)
        VALUES ('1994-11-29', 'Hayward', 54, 37);

You can use COPY to load large amounts of data from flat-text files.  
This is usually faster because the COPY command is optimized for this  
application while allowing less flexibility than INSERT. An example would be:

    COPY weather FROM '/home/user/weather.txt';


## Querying a table
<https://www.postgresql.org/docs/8.0/static/tutorial-select.html>

    select * from tables;       -- select all columns from tables (this form usually discouraged)
    select city, temp_lo, temp_hi, prcp, date from weather;     -- the very same 

You can write expressions (rather than just simple column references) with. 
SELECT.

    select city, (temp_lo+temp_hi)/2 as temp_avg, date from weather

WHERE is analagous to IF

    SELECT * FROM weather
        WHERE city = 'San Francisco' AND prcp > 0.0;

You can request that the results of a query be returned in sorted order:

    SELECT * FROM weather
        ORDER BY city;

    SELECT * FROM weather
        ORDER BY city, temp_lo;

You can request that duplicate rows be removed from the result of a query:

    SELECT DISTINCT city
        FROM weather;

Sorts the distinct results.  

    SELECT DISTINCT city
        FROM weather
        ORDER BY city;


## Joins between tables
<https://www.postgresql.org/docs/8.0/static/tutorial-join.html>

Well visualized SQL representation of various types of joins :  <https://www.codeproject.com/kb/database/visual_sql_joins.aspx>

A join query is one that accesses multiple rows of the same or different tables at one time.  

Whenever a query is done on multiple tables, you can conceptualize a join as creating a new table that is a composite of the joined tables, and the query is executed on the joined table.   

### Inner Joins

    SELECT *                    -- inner join
        FROM weather, cities
        WHERE city = name;

equivalent to 

    SELECT *
        FROM weather INNER JOIN cities ON (weather.city = cities.name);

### Left Outer Joins

    SELECT *
        FROM weather LEFT OUTER JOIN cities ON (weather.city = cities.name);

         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     Hayward       |      37 |      54 |      | 1994-11-29 |               |
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
    (3 rows)


### Self Joins 

A self-join is when we join a table with itself.  
Suppose we wish to find all the weather records that are in the temperature  
range of other weather records.  
Thus we ened to compare the `temp_lo` and `temp_hi` columns of each weather  
row to the temp_lo and temp_hi columns of all other weather rows.

        SELECT W1.city, W1.temp_lo AS low, W1.temp_hi AS high,
            W2.city, W2.temp_lo AS low, W2.temp_hi AS high
            FROM weather W1, weather W2
              WHERE W1.temp_lo < W2.temp_lo
              AND W1.temp_hi > W2.temp_hi;

             city      | low | high |     city      | low | high
        ---------------+-----+------+---------------+-----+------
         San Francisco |  43 |   57 | San Francisco |  46 |   50
         Hayward       |  37 |   54 | San Francisco |  46 |   50

Here we have relabled the weather table as W1 and W2 to distinguish between 
the left and right side of the join.  

### Aliasing of variables

Example of aliasing of variables.  

    SELECT *
        FROM weather w, cities c
        WHERE w.city = c.name;

### Examples of Various Types of Joins

see : <https://www.codeproject.com/kb/database/visual_sql_joins.aspx>


     =# SELECT * FROM weather; 
     
         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27
     San Francisco |      43 |      57 |    0 | 1994-11-29
     Hayward       |      37 |      54 |      | 1994-11-29
     Toronto       |      33 |      45 |  0.1 | 1993-04-26
    (4 rows)
    
    =# SELECT * FROM weather; 
    
     name      | location
    ---------------+-----------
     San Francisco | (-194,53)
     Hayward       | (100,34)
    (2 rows)


Inner Join

    =# SELECT * FROM weather a INNER JOIN cities b ON (a.city=b.name); 
    
         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
     Hayward       |      37 |      54 |      | 1994-11-29 | Hayward       | (100,34)

Left Outer Join

    =# SELECT * FROM weather a LEFT JOIN cities b ON (a.city=b.name);
    
         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
     Hayward       |      37 |      54 |      | 1994-11-29 | Hayward       | (100,34)
     Toronto       |      33 |      45 |  0.1 | 1993-04-26 |               |
    (4 rows)

Right Outer Join

    =# SELECT * FROM weather a RIGHT JOIN cities b ON (a.city=b.name);
         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     Hayward       |      37 |      54 |      | 1994-11-29 | Hayward       | (100,34)

LEFT OUTER JOIN, *

    =# SELECT * FROM weather a LEFT JOIN cities b ON (a.city=b.name) WHERE (b.name=NULL)
    (null)

