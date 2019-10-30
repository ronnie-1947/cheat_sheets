MySql
============

## Initial Commands

| Command | Description |
| ------- | ----------- |
| `SHOW DATABASES;` |Show all the databases present in Server|
| `CREATE DATABASE <database_name>;` |Create a new DataBase|
| `USE <database_name>;` |Use that DataBase|
| `SELECT DATABASE();` |Show DataBase currently in use|
| `DROP DATABASE <database_name>;` |Delte a DataBase|

## Table Initial Commands

| Command | Description |
| ------- | ----------- |
| `SHOW TABLES;` |Show all the tables present in Databases|
| `SHOW COLUMNS FROM <table_name>;` |Show all the columns & Properties|
| `DESC <table_name>;` |"|
| `DROP TABLE <table_name>;` |Delte a Table|

## Creating tables:

```
CREATE TABLE <table_name> (
    name VARCHAR(50),
    age INT,
    gender VARCHAR(6),
    etc...
);
```
## Inserting in Tables:

```
INSERT INTO <table_name> (c1, c2 , ....)
VALUES (V11, V12, ....),
       (V21, V22, ....),
       (V31, V32, ....);
);
```