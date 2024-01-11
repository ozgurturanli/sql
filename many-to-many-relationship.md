# Many to Many (m:n) Relationship in SQL
Let's consider a scenario where we have a database that stores information about students and the courses they are enrolled in. In a many-to-many relationship, each student can enroll in multiple courses, and each course can have multiple students.  
Here are two sample tables to represent this scenario:
## Students table:
```sql
CREATE TABLE Students
(
    student_id INT PRIMARY KEY AUTO_INCREMENT,
    firstname VARCHAR(50),
    surname VARCHAR(50),
    birthday DATE
)
```
## Courses table:
```sql
CREATE TABLE Courses
(
    course_id INT PRIMARY KEY AUTO_INCREMENT,
    course_name VARCHAR(50)
);
```
Now, to establish the many-to-many relationship, we need a junction table. Let's call it "Enrollments":

## Enrollments table:
```sql
CREATE TABLE Enrollments
(
    enrollment_id INT PRIMARY KEY AUTO_INCREMENT,
    student_id INT,
    course_id INT,
    FOREIGN KEY (student_id) REFERENCES Students(student_id),
    FOREIGN KEY (course_id) REFERENCES Courses(course_id)
);
```
Now, let's add some sample data:  
Insert data into Students table:  
```sql
INSERT INTO Students (firstname, surname, birthday)
VALUES
    ('John', 'Doe', '2000-04-04'),
    ('Jane', 'Smith', '2000-01-01'),
    ('Alice', 'Johnson', '2000-01-01'),
    ('Ahmet', 'Ozturk', '2000-01-01'),
    ('Ozgur', 'Turanli', '2000-01-01'),
    ('Buket', 'Turanli', '2000-01-01');
```
Insert data into Courses table:
```sql
INSERT INTO Courses (course_name)
VALUES
    ('Math'),
    ('English'),
    ('Science');
```
Insert data into Enrollments table:
```sql
INSERT INTO Enrollments (student_id, course_id)
VALUES
    (1, 1), -- John Doe is enrolled in Math
    (1, 2), -- John Doe is enrolled in English
    (1, 3), -- John Doe is enrolled in Science
    (2, 1), -- Jane Smith is enrolled in Math
    (2, 2), -- Jane Smith is enrolled in English
    (3, 1), -- Alice Johnson is enrolled in Math
    (3, 2), -- Alice Johnson is enrolled in English
    (4, 1), -- Ahmet Ozturk is enrolled in Math
    (4, 2), -- Ahmet Ozturk is enrolled in English
    (5, 1), -- Ozgur Turanli is enrolled in Math
    (5, 2), -- Ozgur Turanli is enrolled in English
    (6, 1), -- Buket Turanli is enrolled in Math
    (6, 2); -- Buket Turanli is enrolled in English
```
Now, you can retrieve information about students and their enrolled courses using SQL queries. For example:

SQL Query to retrieve students and their enrolled courses:
```sql
SELECT
    Students.firstname AS firstname,
    Students.surname AS surname,
    Courses.course_name AS course_name
FROM
    Students
JOIN
    Enrollments ON Students.student_id = Enrollments.student_id
JOIN
    Courses ON Enrollments.course_id = Courses.course_id;
```
The output would be like this:
```
+-----------+---------+-------------+
| firstname | surname | course_name |
+-----------+---------+-------------+
| John      | Doe     | Math        |
| Jane      | Smith   | Math        |
| Alice     | Johnson | Math        |
| Ahmet     | Ozturk  | Math        |
| Ozgur     | Turanli | Math        |
| Buket     | Turanli | Math        |
| John      | Doe     | English     |
| Jane      | Smith   | English     |
| Alice     | Johnson | English     |
| Ahmet     | Ozturk  | English     |
| Ozgur     | Turanli | English     |
| Buket     | Turanli | English     |
| John      | Doe     | Science     |
+-----------+---------+-------------+
```
This is a simple example of a many-to-many relationship in SQL. **The key is to use a junction table (Enrollments) to connect the two entities (Students and Courses)**.
