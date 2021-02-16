# SQL Crash Course

#### Reference: FreeCodeCamp SQL Tutorial - Full Database Course for Beginners

#### [SQL Tutorial - Full Database Course for Beginners](https://www.youtube.com/watch?v=HXV3zeQKqGY)

## 1. Definition

General definition: any collection of related information.

Computer is the perfect medium for database (to store data).

Especially when:

- Keeps track of trillions of pieces of information
- Information is extremely valuable and critical
- Information security is essential



## 2. Database Management System (DBMS)

A special software that helps users create and maintain a database, it:

- Manages large amounts of information
- Handles security
- Backups
- Importing/exporting data
- Concurrency
- Interactions



### 2.1 C. R. U. D.

1. Create
2. Read

3. Update

4. Delete



### 2.2 Relational Database (SQL)

Organizes data into one or more tables.

- each table has columns and rows
- a unique key identifies each row



#### 2.2.1 SQL (Structured Query Language)

- standardized langauge for interacting with RDBMS



#### 2.2.2 Relational Database Management Systems (RDBMS)

Help users create and maintain a relational database

- MySQL, Oracle, postgreSQL, etc.



### 2.3 Non-relational Database (noSQL)

Organize data into anything but a traditional table.

- Key-value stores
- Documents
- Graphs
- Flexible tables

E.g.: JSON, XML, Key-value Hash (Dictionary)



#### 2.3.1 Non-relational Database Management Systems (NRDBMS)

- has no set language standard, has their own language to perform C.R.U.D.
- mongoDB, dynamoDB, Apache Cassandra, Firebase, etc.



### 2.4 Database Queries

Queries are requests made to the DBMS for specific information. Queries can help us handle data retrieval in very complex database structure.



## 3. Table Basics

Student Table:

| student_id * | name | major |
| ------------ | ---- | ----- |
|              |      |       |

User Account Table:

| email * | password | date_created | type |
| ------- | -------- | ------------ | ---- |
|         |          |              |      |

- Column: store a particular attribute of information

- Row: an individual entry of the data

* Primary key (*): an attribute that uniquely identify a specific entry
  * Note that student_id and email are the unique attribute in each of the tables.
  * Surrogate key: a key that is artificially created to uniquely identify each entry
  * Natural key: a unique key that has real world purpose.

Branch Supplier:

| branch_id * | supplier_name * | supplier_type |
| ----------- | --------------- | ------------- |
|             |                 |               |

* Primary key can compose of 2 attributes, it is called composite key.
  * Use composite keys when you need 2 or more attributes to uniquely define each entry
  * You can sometimes combine two foreign keys as a composite key to define the relationship between two tables.

Employee  Table:

| emp_id * | name | birth_date | sex  | salary | branch_id ** |
| -------- | ---- | ---------- | ---- | ------ | ------------ |
|          |      |            |      |        |              |

Branch Table:

| branch_id * | branch_name | mgr_id **                         |
| ----------- | ----------- | --------------------------------- |
|             |             | Note: Manager is also an employee |

* Foreign key (**): a primary key from another table, that refers you to that table.
  * Foreign key defines the relationship between different tables.
  * Foreign key maps you to another table so you can retrieve further information.
  * You can retrieve the branch_name of the employee through the foreign key.



## 4. SQL Basics

Variants of SQL languages may not be compatible across different RDBMS.

SQL compose of:

1. Data Query Language
   1. Get information from the database.
2. Data Definition Language
   1. Define database schemas
3. Data Control Language
   1. Define user permission and access.
4. Data Manipulating Language
   1. Insert, update and delete data.



### 4.1 An Example of a Query

Set of instructions given for the RDBMS.

```sql
SELECT employee.name, employee.age
FROM employee
WHERE employee.salary > 30000;
```



### 4.2 Creating Tables

Creating tables is the necessary first step to define a database.

#### Common Table Data Types

1. `INT`: whole numbers
2. `DECIMAL(M,N)`: decimal numbers
   1. `M` total number of digits
   2. `N` number of digits after the decimal point
3. `VARCHAR(N)`: string of text of length `N`
4. `BLOB`: binary large objects, e.g. photos, file, etc.
5. `DATE`: Date in `YYYY-MM-DD`
6. `TIMESTAMP`: Precise time `YYYY-MM-DD HH:MM:SS`



> Conventions: write SQL reserved words in capital letters.



#### Create a Table

```sql
CREATE TABLE student (
  student_id INT PRIMARY KEY,
  name VARCHAR(20),
  major VARCHAR(20)
  /* PRIMARY KEY(student_id) will work too
     if you want to define it later. */
);
```



#### Describe a Table

```
DESCRIBE student;
```



#### Alter a Table

```
ALTER TABLE student ADD gpa DECIMAL(3,2);
ALTER TABLE student DROP COLUMN gpa;
DROP TABLE student;
```



#### Insert Data into a Table

```sql
INSERT INTO student VALUES(
  1, 'Jack', 'Biology'
);
```

You can leave out some attributes by specifying the attributes you would want to insert.

```sql
INSERT INTO student(student_id, name) VALUES(3, 'Claire');
```

> If you did not specify the attributes in an `INSERT` query, the DBMS will insert the data according to how the table is defined.



#### Define Table Constraints

```sql
CREATE TABLE student (
  student_id INT AUTO_INCREMENT,
  name VARCHAR(20) NOT NULL,
  major VARCHAR(20) UNIQUE,
  class VARCHAR(20) DEFAULT 'Undecided',
  PRIMARY KEY(student_id)
);
```

* `NOT NULL` prevents empty entry

* `UNIQUE` prevents duplicate entry in the column
  * `PRIMARY KEY` is `NOT NULL` and `UNIQUE` automatically
* `DEFAULT` sets a default entry if none is provided
* you can `AUTO_INCREMENT` your primary key if you do not need to specify it



### 4.3 Altering Data in a Table

Update data given conditions.

```sql
UPDATE student
SET major = 'Bio'
WHERE major = 'Biology';
```



Reset all attributes in a table.

```sql
UPDATE student
SET major = 'Undecided';
```



Delete an entry.

```sql
DELETE FROM student
WHERE student_id = 5;
```



Delete all entries.

```sql
DELETE FROM student;
```



### 4.4 More Basic Queries

* `SELECT` command can query information from the database.
* `SELECT attribute` queries `attribute` from the table.
  * `SELECT *` queries all attributes from the table.

```sql
SELECT name, major
FROM student;
```



When you are in more complex queries, you have to specify the table name for the attributes you want to query.

```sql
SELECT student.name, student.major
FROM student;
```



#### Order Queries

In ascending order,

```sql
SELECT name, major
FROM student
ORDER BY name;
```

In descending order,

```sql
SELECT name, major
FROM student
ORDER BY student_id DESC;
```

> You can order your queries according to any attribute in the table, even if the attribute was not queried.

You can have multiple order rules, so that when the first rule results in a tie, the second rule will resolve it.

```sql
SELECT *
FROM student
ORDER BY major, student_id;
```



#### Limit Queries

You can limit your queries to the first few entries using `LIMIT`, which is useful when combined with  `ORDER BY`.

```sql
SELECT *
FROM student
ORDER BY student_id
LIMIT 2;
```



#### Conditional Queries

You can use these 

* use `where` to define conditions
* logical operands: `OR`, `AND`
* `<`, `>`, `<=`, `>=`, `=`, `<>`
* use `IN` for a set of matching criteria
  * `WHERE name IN ('Claire', 'Kate', 'Mike')`



