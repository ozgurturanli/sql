# SQL (Structered Query Language)
The below notes are from my IT school in Hamburg.
You may always use https://dev.mysql.com/doc/refman/8.0/en/ as a refference.

The SQL statements are generally divided into two groups:
- Data Definition Language (DDL)
- Data Manipulation Language (DML)
------------------------------------------------------
There are also systems that further divide into:  
- Data Query Language
- Data Control Language
- Transaction Control Language

DDL includes all statements with CREATE, DROP, ALTER, EXPLAIN 
DML includes statements with INSERT, DELETE, SELECT, UPDATE  

SQL consists of verbs, nouns and prepositions from the English language  

MySQL (and other relational database management systems)

Some groups of data types:
- Integral types
- Floating point numbers
- Decimal numbers with a fixed number of decimal places
- Spatial data types
- Character strings
- Date and Time
- JSON

With the;
- integers, you should definitely remember INT from the start. INT is an integer that requires 4 bytes of memory.
- floating point numbers, you should remember either DOUBLE or FLOAT
- decimal numbers, either DECIMAL or NUMERIC
- character strings, you need VARCHAR and CHAR right from the start
- date and time types DATE, TIME, DATETIME, TIMESTAMP

## Floating point numbers

MySQL distinguishes between a single-precision type (FLOAT, 4 bytes) and a double-precision type (DOUBLE, 8 bytes) for floating point numbers.

## Date and Time

When entering the date and time, make sure to use ISO format. A date must be entered as a string with quotation marks. Certain separators between the components of the date or time are not required:  
```
"2023|12|24"  
'2024|01|06'  
"20240105"  
But it is always presented like this:  
2024-01-06  
17:15:24  
```
## Strings
### VARCHAR  
Is a variable length string.

A column of type VARCHAR must be defined with a Maximum Length parameter:
```sql
CREATE TABLE user (
first_name VARCHAR(50),
last_name VARCHAR(50));
```
Here you can enter a value of a maximum of 50 characters in each of the two columns. If you enter fewer than 50 characters, fewer bytes are used for storage.

A column of type CHAR must also be defined with a maximum length. If you enter fewer than the allowed number of characters, the maximum number of bytes will still be used to store the value.

## User management and rights management in the MySQL server
User management cannot be transferred from one database system to another.  
In a MySQL server there is the mysql database for user and rights management. The users are entered in a table user, the connection parameters that are needed to log in to a server are in the user, host and password columns.  

First, you should always assign a password for the root user.  
One possibility is:  
```sql
ALTER USER 'root'@'<host>' IDENTIFIED BY '<password>';
```
Another possibility:  
```sql
UPDATE USER 'root'@'<host>' SET PASSWORD = '<password>';
```
Example:
```sql
ALTER USER 'root'@'127.0.0.1' IDENTIFIED BY 'a1b2c3c4';
```
Or:  
```sql
UPDATE USER 'root'@'localhost' SET PASSWORD = 'a1b2c3d4';  
```
You can create a new user with CREATE USER
```sql
CREATE USER '<username>'@'<host>' IDENTIFIED BY '<password>';
```
#### Example
```sql
CREATE USER 'ozgur'@'localhost' IDENTIFIED BY 'ozgur';
```
This creates a new user in the user table, but it does not yet have any rights.
However, the new user must be given explicit rights:  
There are statements with GRANT for this  
This allows a user to have rights such as SELECT, DELETE, UPDATE, INSERT, CREATE, ALTER
GRANT SELECT, INSERT ON club_frelin.* TO 'ozgur'@'localhost';  
In order for the changes to take effect, the rights table must be reloaded, which you do
FLUSH PRIVILEGES;  

You can revoke rights with REVOKE:  
```sql
REVOKE INSERT ON club_frelin.* FROM 'ozgur'@'localhost';  
FLUSH PRIVILEGES;
```

You can delete a user with DROP USER:  
```sql
DROP USER 'ozgur'@'localhost';
```

## INSERT statements

An insert statement begins with the verb INSERT  
We continue with INTO <table name>  
Next comes a list of columns in parentheses  
Then comes the keyword VALUES  
Lastly, in brackets, a list of values. The value list must match the column list (number, order, data types)  
If you have a value for each column in the value list, then you can do without the column list  
```sql
INSERT INTO customers (firstname, lastname, email) VALUES
('Hans', 'Mueller', 'hans@me.com'),
('Franz', 'Meier', 'franz@me.com');
SELECT * FROM customers;
```

## SELECT statements

The simplest SELECT statement looks like this:
```sql
SELECT * FROM <table name>
```
Example
```sql
SELECT * FROM member;
```

It is also possible to only display certain columns (projection)
```sql
SELECT first_name, last_name FROM member;
```
You can use a WHERE clause to limit the number of rows

## WHERE clause

A WHERE clause contains a condition (an expression that is either true or false)
Such expressions are formed using comparison operators and logical operators:

### The comparators
=  
<  
<=  
>  
>=  
%  
### The logical operators
&& (AND)  
|| (OR)  
^ (XOR)  
! (NOT)  
IN  
BETWEEN  

For character strings, in addition to the comparison with =, there is also an operator LIKE for simple search patterns with _ and %
_ represents any character.  
% stands for any number of characters  
```
MariaDB [club_frelin]> SELECT * FROM member WHERE last_name LIKE 'P_ters';

+------------+-----------+------------+-------------+
| first_name | last_name | background | aspirations |
+------------+-----------+------------+-------------+
| Jen        | Peters    | Retraining | let's see   |
+------------+-----------+------------+-------------+


MariaDB [club_frelin]> SELECT * FROM member WHERE last_name LIKE 'P%';
+------------+-----------+------------+-------------+
| first_name | last_name | background | aspirations |
+------------+-----------+------------+-------------+
| Jen        | Peters    | Retraining | let's see   |
+------------+-----------+------------+-------------+
```
A WHERE clause is used in a SELECT statement. You can't damage data with a SELECT statement. You can destroy data with an UPDATE or DELETE statement, so you have to be careful that the WHERE clause works correctly.  

"Normalization" in the context of relational databases means:
Planning the structure of a database, specifically the structure of the individual tables and the relationships between the tables.
An attempt is made to exclude certain sources of error

The sources of error are called “anomalies”
Insert Anomaly
Update Anomaly
Delete Anomaly

Normalization is a procedure in several precisely defined steps.
The individual steps of normalization are called “normal forms”
A distinction is made between 1.NF, 2.NF, 3.NF, Boyce-Codd normal form, 4.NF, 5.NF
