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
"2023|12|24"
'2024|01|06'
"20240105"
But it is always presented like this:
2024-01-06
17:15:24

## VARCHAR
Is a variable length string.

A column of type VARCHAR must be defined with a Maximum Length parameter:
CREATE TABLE user (
first_name VARCHAR(50),
last_name VARCHAR(50));
Here you can enter a value of a maximum of 50 characters in each of the two columns. If you enter fewer than 50 characters, fewer bytes are used for storage.

A column of type CHAR must also be defined with a maximum length. If you enter fewer than the allowed number of characters, the maximum number of bytes will still be used to store the value.
