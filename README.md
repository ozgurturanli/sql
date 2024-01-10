# SQL (Structered Query Language)
The below notes are from my IT school in Hamburg.
You may always use https://dev.mysql.com/doc/refman/8.0/en/ as a refference.

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
```
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
```
ALTER USER 'root'@'<host>' IDENTIFIED BY '<password>';
```
Another possibility:  
```
UPDATE USER 'root'@'<host>' SET PASSWORD = '<password>';
```
Example:
```
ALTER USER 'root'@'127.0.0.1' IDENTIFIED BY 'a1b2c3c4';
```
Or:  
```
UPDATE USER 'root'@'localhost' SET PASSWORD = 'a1b2c3d4';  
```
You can create a new user with CREATE USER  
```
CREATE USER '<username>'@'<host>' IDENTIFIED BY '<password>';
```
#### Example  
```
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
```
REVOKE INSERT ON club_frelin.* FROM 'ozgur'@'localhost';  
FLUSH PRIVILEGES;
```

You can delete a user with DROP USER:  
```
DROP USER 'ozgur'@'localhost';
```
