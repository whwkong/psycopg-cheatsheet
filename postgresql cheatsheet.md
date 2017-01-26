# postgreSQL cheatsheet

see: <http://www.tutorialspoint.com/postgresql/>

## postgres.app
<http://postgresapp.com/>



## Commands

    $ postgres				# this is the server
	$ psql					# this is the client

	$ createdb <testdb>		# can also do this within client with SQL command


## Data Types

## Creating a db

command line

    createdb testdb

    pgres=# CREATE DATABASE testdb;


## listing a db

    \l or \list           : list a
    \dt                     : list all tables in current database

    \c                      : switch databases  
    \connect testdb          

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

Example :

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












