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
| `DROP DATABASE <database_name>;` |Delete a DataBase|

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
| `SELECT IFNULL(<column_name>, 0) FROM <table_name>;` |Show 0 if column have value NULL|
| `SELECT <column_name> FROM <table_name>;` |Show the specified column|
| `SELECT <column_name> AS <abc> FROM <table_name>;` |Using Alias abc|
| `WHERE <any condition> ;` |Where is used to Filter data|
| `HAVING <any condition> ;` |Used Just like as WHERE , used after GROUP BY|
| `LIKE <any condition> ;` |LIKE is used after WHERE to ultra Filter data|
| `ORDER BY <column_name> ;` |ORDER BY is used to sort data in ascending order|
| `ORDER BY <column_name> DESC ;` |ORDER BY DESC is used to sort data in descending order|
| `LIMIT <strt_num>, <prsnt_num> ;` |Present only limited data|

___________________________
___________________________
## Insert UNIQUE DATA in tables (PRIMARY KEY & UNIQUE)
PRIMARY KEY : It is a column or combination of column that uniquely identifies a row in a table.
Given below example of combination (user_id, photo_id) which cannot be duplicated .
```
CREATE TABLE <table_name> (
    user_id INTEGER NOT NULL,
    photo_id INTEGER NOT NULL,
    created_at TIMESTAMP DEFAULT NOW(),
    FOREIGN KEY(user_id) REFERENCES users(id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY(photo_id) REFERENCES photos(id) ON DELETE CASCADE ON UPDATE CASCADE,
    PRIMARY KEY(user_id, photo_id)
);
```

UNIQUE : This keyword is used to uniquely set a data .
```
CREATE TABLE users (
    id INTEGER AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
```

___________________________
___________________________



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

| Command | Description |
| ------- | ----------- |
| `CONCAT` |Used to join two or more |
| `SUBSTRING` |Used as SPLICE in JS|
| `REPLACE` |Replaces certain characters in String|
| `REVERSE` |Reverse a String|
| `CHAR_LENGTH` |Show the specified column|
| `UPPER` |Show the specified column|
| `LOWER` |Show the specified column|
| `DISTINCT` |Show the specified column|

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

### DISTINCT
Remove Duplicates
```
SELECT
    DISTINCT(<column_name>)
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
____________________________
____________________________
## Aggregate Functions

| Command | Description |
| ------- | ----------- |
| `COUNT` |Gives no. of items present in table |
| `GROUP BY` |Summarizes identical data into single rows|
| `MIN & MAX` |Find minimum or maximum among a list|
| `SUM` |Used to sum a whole column|
| `AVG` |Used to AVG a whole column|

### Count
Gives the no. of \<required_item> present in table
```
SELECT
COUNT(
    <column_name>
)FROM <table_name>
```
________________________________

### Group By
Summarizes identical data into single rows
```
SELECT 
<column_name-1>, <column_name-2>, COUNT(*) 
FROM books 
GROUP BY <column_name-1>, <column_name-2>;
```
________________________________

### Min & Max
Find minimum or maximum among a list
```
SELECT MIN(<column_name>)
FROM <table_name>
```
_________________________________

### Using Min & Max along with Group By
Table containing first book release of every author
```
SELECT author_fname, 
       author_lname, 
       Min(released_year) 
FROM   books 
GROUP  BY author_lname, 
          author_fname;
```
__________________________________

### SUM
Used to sum a whole column
```
SELECT SUM(<column_name>) 
FROM <table_name>;
```
__________________________________

### AVG
Used to AVG a whole column
```
SELECT AVG(<column_name>) 
FROM <table_name>;
```
__________________________________
__________________________________

## WORKING WITH DATES

### Create Table of DATES
```
CREATE TABLE people (name VARCHAR(100), 
birthdate DATE, 
birthtime TIME, 
birthdt DATETIME);
```

### Format_Date
```
SELECT DATE_FORMAT(<column_date-time>, '%m/%d/%Y at %h:%i')
FROM <table_name>;
```
__________________________________

### DATE_DIFF & DATE_ADD
```
SELECT 
DATEDIFF(NOW(), birthdate) 
FROM people;
```
```
SELECT 
DATE_ADD(birthdt, INTERVAL 10 year) 
FROM people;
```
__________________________________

### TIME-INTERVAL , DATE_DIFF & DATE_ADD
```
SELECT NOW()+INTERVAL 1 MONTH  FROM people;
SELECT NOW()-INTERVAL 1 MONTH  FROM people;
```
__________________________________

### Create TIME-STAMP Table
```
CREATE TABLE comments (
    content VARCHAR(100),
    created_at TIMESTAMP DEFAULT NOW() ON UPDATE NOW()
);
```
____________________________
____________________________

## CASE STATEMENTS & IF STATEMENTS
CASE works as IF ELSE
```
SELECT title, stock_quantity,
    CASE 
        WHEN stock_quantity <= 50 THEN '*'
        WHEN stock_quantity <= 100 THEN '**'
        ELSE '***'
    END AS 'Stock'
FROM books; 
```
Using IF
```
SELECT title, stock_quantity,
    IF stock < 10 , 'Out of stock', 'Available' AS status
```

_____________________________
_____________________________
## DATA RELATIONSHIPS

### Creating relational table

```
CREATE TABLE <table_name>(
    id INT AUTO_INCREMENT PRIMARY KEY,
    customer_id INT,
    FOREIGN KEY(customer_id) REFERENCES customers(id)
);
```
_____________________________

### Inner Joint
```
SELECT * FROM <table_1>
    JOIN <table_2>
    ON <condition>
```
_____________________________
### Left Join

```
SELECT * FROM <table_1>
    LEFT JOIN <table_2>
    ON <condition>
```
_____________________________
### RIGHT Join

```
SELECT * FROM <table_1>
    RIGHT JOIN <table_2>
    ON <condition>
```
_____________________________

### Join MANY TO MANY (3 tables)
```
SELECT 
    title,
    rating,
    CONCAT(first_name,' ', last_name) AS reviewer
FROM reviewers
INNER JOIN reviews 
    ON reviewers.id = reviews.reviewer_id
INNER JOIN series
    ON series.id = reviews.series_id
ORDER BY title;
```
_____________________________
_____________________________

## TRIGGERS
These are rules which can be set for a table, that take place after or just before an event . Which can also give an error message set by us.

Given below , an example of a trigger that gives error message when age entered is less than 18.
```
DELIMITER $$
CREATE TRIGGER must_be_adult
	BEFORE INSERT ON users FOR EACH ROW
    BEGIN 
		IF NEW.age < 18
        THEN 
			SIGNAL SQLSTATE '45000'
				SET MESSAGE_TEXT = 'Must be an adult';
		END IF;
	END;
$$
DELIMITER ;
```
Given below , Trigger code that stops self-following on social media.
```
DELIMITER $$
CREATE TRIGGER prevent_self_follows
	INSERT BEFORE ON follows FOR EACH ROW
		BEGIN
			IF NEW.follower.id = NEW.followee_id
            THEN 
				SIGNAL SQLSTATE '45000'
                SET MESSAGE_TEXT = 'You cannot follow yourself!';
			END IF;
        END;
$$
DELIMITER
```
Given below , Trigger code that entrys data when someone unfollows someone. This also uses another syntax to insert data in a table.
```
DELIMITER $$
CREATE TRIGGER capture_unfollow
	AFTER DELETE ON follows FOR EACH ROW
		BEGIN
			INSERT INTO unfollows
            SET follower_id = OLD.follower_id,
				followee_id = OLD.followee_id;
        END;
$$;
DELIMITER
```

### Managing TRIGGERS


| Command | Description |
| ------- | ----------- |
| `SHOW TRIGGERS` |Show and list all triggers |
| `DROP TRIGGER <trigger_name>` |Delete a trigger|
