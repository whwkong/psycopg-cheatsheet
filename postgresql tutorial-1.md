# postgreesql tutorial-1

<https://www.postgresql.org/docs/8.0/static/tutorial.html>

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





