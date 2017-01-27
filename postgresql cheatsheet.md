# postgreSQL cheatsheet

see: <http://www.tutorialspoint.com/postgresql/>

## postgres.app
<http://postgresapp.com/>



## Commands

    $ postgres				# this is the server
	$ psql					# this is the client

	$ createdb <testdb>		# can also do this within client with SQL command


## Data Types

Basic data types

    char(80)        fixed length
    varchar(80)     variable length up to 80
    int
    smallint
    real            floats
    double          double floats
    date            
    time
    timestamp
    interval
    point           a geometric type


## Creating a db

command line

    createdb testdb

    pgres=# CREATE DATABASE testdb;


## listing a db

    \l or \list           : list a
    \dt                     : list all tables in current database

    \c                      : switch databases  
    \connect testdb          

## Generating SQL from internal commands

The `psql -E` switch outputs SQL commands for internal psql commands such as  
`\l` or `\dt`.  


    psql -E -l 
    psql -E -U postgres -c "\l"

The output is : 

    ********* QUERY **********
    SELECT d.datname as "Name",
           pg_catalog.pg_get_userbyid(d.datdba) as "Owner",
           pg_catalog.pg_encoding_to_char(d.encoding) as "Encoding",
           d.datcollate as "Collate",
           d.datctype as "Ctype",
           pg_catalog.array_to_string(d.datacl, E'\n') AS "Access privileges"
    FROM pg_catalog.pg_database d
    ORDER BY 1;
    **************************
    
                                                 List of databases
                 Name             |    Owner    | Encoding |   Collate   |    Ctype    |   Access privileges
    ------------------------------+-------------+----------+-------------+-------------+-----------------------
     postgres                     | postgres    | UTF8     | en_US.UTF-8 | en_US.UTF-8 |
     template0                    | postgres    | UTF8     | en_US.UTF-8 | en_US.UTF-8 | =c/postgres          +
                                  |             |          |             |             | postgres=CTc/postgres


    SELECT d.datname as "Name"
    FROM pg_catalog.pg_database d   -- d now refers to pg_catalog.pg_database 
    ORDER BY 1;     
        -- note that 'ORDER by 1' means order by the first column, which in this case is 'Name'.
        -- oft considered bad form as the column order can change.  

    SELECT datname from pg_database -- functionally same as above

## Drop Database

Drop or delete a database

    =# DROP DATABASE name
    $ dropdb <name> 

## CREATE Table

    CREATE TABLE table_name(
       column1 datatype,
       column2 datatype,
       column3 datatype,
       .....
       columnN datatype,
       PRIMARY KEY( one or more columns )
    );

Examples :

    CREATE TABLE weather (
        city            varchar(80),   
        temp_lo         int,           -- low temperature
        temp_hi         int,           -- high temperature
        prcp            real,          -- precipitation
        date            date
    );

    CREATE TABLE COMPANY(
       ID INT PRIMARY KEY     NOT NULL,
       NAME           TEXT    NOT NULL,
       AGE            INT     NOT NULL,
       ADDRESS        CHAR(50),
       SALARY         REAL
    );
    
    CREATE TABLE DEPARTMENT(
       ID INT PRIMARY KEY      NOT NULL,
       DEPT           CHAR(50) NOT NULL,
       EMP_ID         INT      NOT NULL
    );

## DROP Table

    DROP TABLE table_name;












