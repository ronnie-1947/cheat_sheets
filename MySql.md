MySql
============

## Initial Commands

| Command | Description |
| ------- | ----------- |
| `SHOW WARNINGS;` |Show all the warnings present|


## Database Commands

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
    name_id INT NOT NULL AUTO_INCREMENT,
    name VARCHAR(50) NOT NULL DEFAULT 'Dummy',
    age INT,
    gender VARCHAR(6),
    PRIMARY KEY (name_id)
    etc...
);
```
## Inserting in Tables:

```
INSERT INTO <table_name> (c1, c2 , ....)
VALUES (V11, V12, ....),
       (V21, V22, ....),
       (V31, V32, ....);
```


## Filtering Commands
| Command | Description |
| ------- | ----------- |
| `SELECT <column_name> FROM <table_name>;` |Show the specified column|
| `SELECT <column_name> AS <abc> FROM <table_name>;` |Using Alias abc|
| `WHERE <any condition> ;` |Where is used to Filter data|

## Updating & Deleting
```
///---- Updating Data ----////
UPDATE <table_name>
SET <column_name> ='Anything' 
WHERE <condition>;

////---- Delete Data ----////
DELETE FROM <table_name>
WHERE <condition>
```

## String Functions

### CONCAT
Used to join two or more 
```
SELECT
CONCAT (<column-1> , ' ' , <column-2 , .....>)
FROM <table_name>
WHERE <condition>
```
_________________________

### SUBSTRING
Used as SPLICE in JS
```
SELECT SUBSTRING(
    <column_name>, strt_num, end_num;
) FROM <table_name>
```
_________________________

### REPLACE
Replaces certain characters in String
```
SELECT
    REPLACE(<column_name> , 'e', 3)
    FROM <table_name>
```
_________________________

### REVERSE
Reverse a String
```
SELECT
    REVERSE(<column_name>)
    FROM <table_name>
```
_________________________

### CHAR_LENGTH
Gives num of characters in a String
```
SELECT
    CHAR_LENGTH(<column_name>)
    FROM <table_name>
```
_________________________

### UPPER & LOWER
Converts to UpperCase Or LowerCase
```
SELECT
    UPPER(<column_name>)
    FROM <table_name>

SELECT
    LOWER(<column_name>)
    FROM <table_name>
```
_________________________

Combine CONCAT & SUBSTRING & REPLACE
```
SELECT 
    CONCAT(
        SUBSTRING(column_name, strt_num, end_num),
        '...'
    ) AS '<Any alias_name>'
FROM books;
```