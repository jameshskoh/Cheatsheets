# SQL Crash Course - FreeCodeCamp

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
  * The usual syntaxes to set up `FOREIGN KEY`s are as follows, more in Section 5.4.

```sql
CREATE TABLE branch (
  -- ...... skipped
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id)
  ON DELETE SET NULL
);
```



```sql
CREATE TABLE branch_supplier (
  -- ...... skipped
  FOREIGN KEY(branch_id) REFERENCES
  branch(branch_id) ON DELETE CASCADE
);

```



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



## 5. SQL Advanced

### 5.1 More Query Commands

#### Renaming Table Views

You can query the data and rename it's column name in the data view.

```sql
SELECT first_name AS forename, last_name AS surname
FROM employee;
```



#### Query Types of Entries in an Attribute

```sql
SELECT DISTINCT sex
FROM employee;
```



### 5.2 Common SQL Functions

#### `COUNT()`

Count number of entries.

```sql
SELECT COUNT(emp_id)
FROM employee;
```



```sql
SELECT COUNT(emp_id)
FROM employee
WHERE sex = 'F' AND birth_date > '1970-01-01';
```



#### `AVERAGE()`

Calculate the average value of the entries.

```sql
SELECT AVG(salary)
FROM employee
WHERE sex = 'M';
```



#### `SUM()`

Calculate the total value of the entries.

```sql
SELECT SUM(salary)
FROM employee;
```



#### `GROUP BY`

Count entries and group them.

```sql
SELECT sex, COUNT(sex)
FROM employee
GROUP BY sex;
```



e.g. Calculate the total sales of an employee, where there are multiple entries of his/her sales in the table.

```sql
SELECT emp_id, SUM(total_sales)
-- this line queries the SUM() and then display emp_id along side so you know who the sum belongs to
FROM works_with
GROUP BY emp_id;
```



#### Wildcards and `LIKE`

#### `*`

- Wildcard for querying attributes



#### `%`

* Wildcard for any number of characters



#### `_`

* Wildcard for one character only



```sql
SELECT *
FROM client
WHERE client_name LIKE '%LLC';
-- matches any LLC companies
```



```sql
SELECT *
FROM client
WHERE client_name LIKE '%Labels%';
-- matches any companies with 'Labels' in it
```



```sql
SELECT *
FROM employee
WHERE birth_date LIKE '____-10%';
-- matches any employees who's birthday are in October
```



#### `UNION`

>  The sub-queries have to have the same number and data type of columns to work.

```sql
SELECT first_name AS all_names
FROM employee
UNION
SELECT branch_name
FROM branch;
```



```sql
SELECT client_name, client.branch_id
FROM client
UNION
SELECT supplier_name, branch_supplier.branch_id
FROM branch_supplier;
```



```sql
SELECT salary
FROM employee
UNION
SELECT total_sales
FROM works_with;
```



#### `JOIN` and `ON`

`JOIN` joins queries across different tables and combine them into a single view. `JOIN` and `FOREIGN KEY` works together to comply with database normal forms.



```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch
ON employee.emp_id = branch.mgr_id
-- based on this criterion
```

The above query will query `employee` that happens to be a `mgr` through this criterion `employee.emp_id = branch.mgr_id`. This query will also join other attributes available on `employee` and `branch` in the process.



#### `LEFT JOIN`

```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch -- JOINs branch to employee
ON employee.emp_id = branch.mgr_id
-- based on this criterion
```

The query will query all `employee` and only join `branch` attributes when there is a match. "Join available data to the left".



#### `RIGHT JOIN`

```sql
SELECT employee.emp_id, employee.first_name, branch.branch_name
FROM employee
JOIN branch -- JOINs branch to employee
ON branch.mgr_id = employee.emp_id
-- based on this criterion
```

`RIGHT JOIN` does the exact opposite as `LEFT JOIN`.



#### `FULL OUTER JOIN`

A more advanced join method not available in MySQL, a left join and right join combined.



### 5.3 Nested Queries

Nested queries enable us to query using the result from another query.

#### Example 1: Using Nested Queries with  `IN` 

`employee`s who sold more than 30000.

```sql
SELECT works_with.emp_id
FROM works_with
WHERE total_sales > 30000;
```

Using these results to query `employee.first_name` and `employee.last_name`.

```sql
SELECT employee.first_name, employee.last_name
FROM employee
WHERE employee.emp_id IN (
  SELECT works_with.emp_id
  FROM works_with
  WHERE total_sales > 30000
);
```



#### Example 2: Using Nested Queries with `=`

In this example, the nested query has to return 1 result only. Whilst, the previous example queries based on all the data given by the nested queries.

The `branch` `Scott` manages.

```sql
SELECT branch.branch_id
FROM branch
WHERE branch.branch_id = 102;
```

Find all the `client`s of the branch that `Scott` manages.

```sql
SELECT client.client_name
FROM client
WHERE client.branch_id = (
  SELECT branch.branch_id
  FROM branch
  WHERE branch.branch_id = 102
  LIMIT 1 -- a safeguard so that you only query for the first branch
);
```



### 5.4 `DELETE` Entries Associated with Foreign Keys

If you delete an entry that is associated with other tables by foreign keys, the queries using the foreign keys will lead to missing data.

E.g. When you delete an entry in the `employee` table, which the `employee` happens to be a manager, then the `branch` he/she managed would not be able to refer to its manager using the `mgr_id` foreign key.

This section discusses how to handle this situation.

> Note that the two commands below are defined as an attribute of the `FOREIGN KEY`, instead of a command that you have to execute every time you issue a `DELETE` command.

#### `ON DELETE SET NULL`

When you delete the entry, any foreign keys associated with this entry will be set to `NULL`.

```sql
CREATE TABLE branch (
  -- ...... skipped
  FOREIGN KEY(mgr_id) REFERENCES employee(emp_id)
  ON DELETE SET NULL
);
```

In this case, the `branch` `mgr_id` is set to `NULL`, indicating a missing manager.

#### `ON DELETE CASCADE`

When you delete the entry, any entries with foreign keys associated with this entry will be deleted. In other words, the `DELETE` command `CASCADE`s.

When the `FOREIGN KEY` is part of the `primary key`, you have to use `CASCADE` as `PRIMARY KEY` cannot be `NULL`.

```sql
CREATE TABLE branch_supplier (
  -- ...... skipped
  FOREIGN KEY(branch_id) REFERENCES
  branch(branch_id) ON DELETE CASCADE
);

```

In this case, the `branch` is closed, and thus its `supplier`s no longer matter and can be deleted.



## 6. Related Topics

### 6.1 Triggers

In order to activate triggers, run this code in your SQL command line.

```bash
use DatabaseName
```



```sql
DELIMITER $$
CREATE
  TRIGGER my_trigger
  BEFORE INSERT ON employee --- also works for UPDATE, DELETE, etc.
  FOR EACH ROW BEGIN
    INSERT INTO trigger_test VALUES('added new employee');
    --- Just a code to demonstrate how TRIGGER works
  END$$
DELIMITER ;
```

> Changing the delimiter to `$$` works so that the DBMS will not recognize the `;` in the inner command as its delimiter, and end the command prematurely.

> Since changing the delimiter will only works in the terminal, you can only **create triggers in the terminal**.



#### Trigger Using Conditionals

```sql
DELIMITER $$
CREATE
  TRIGGER my_trigger
  BEFORE INSERT ON employee
  FOR EACH ROW BEGIN
    IF NEW.sex = 'M' THEN
      INSERT INTO trigger_test VALUES('Male employee.');
    ELSEIF NEW.sex = 'F' THEN
      INSERT INTO trigger_test VALUES('Female employee.');
    ELSE
      INSERT INTO trigger_test VALUES('Other employee.');
    END IF;
  END$$
DELIMITER ;
```



#### Removing a Trigger

```sql
DROP TRIGGER my_trigger;
```



### 6.2 Entity Relationship (ER) Diagram

#### 6.2.1 Entities

Objects we want to model and store information about. Denoted in the ER diagram with rectangles.

#### Weak Entities

An entity that cannot be uniquely identified by its attributes alone. Denoted in the ER diagram with double rectangles.

* A class can have an exam, but an exam cannot exist independently.
  * Relationship: Class-has-Exam
* The weak entity must have total participation in a relationship. Refer Section 6.2.3 for more details.

#### 6.2.2 Attributes

Specific pieces of information about an entity. Denoted in the ER diagram with ovals.

#### Primary Keys

Multiple attributes that uniquely identify an entry in the database table. Denoted like an attribute, but underlined.

#### Composite Attributes

An attribute that can be broken up into sub-attributes. Denoted like an attribute, linking between the entity and the sub-attributes.

#### Multi-valued Attributes

An attribute that can have more than one value. E.g.: a student can join multiple clubs. Denoted in the ER diagram with double ovals.

#### Derived Attributes

Attributes that can be derived from other attributes. We are not actually going to keep track of derived attributes, but it's drawn for clarity. Denoted in the ER diagram with dotted-lined ovals.

#### 6.2.3 Relationships

The relationship between two entities, usually a verb. Denoted in the ER diagram with diamonds.

> Weak entity relationship is denoted with double diamonds.

The relationship object has to connect to at least 2 entities. 

* Single-lined connection: partial participation (0 ~ N)
* Double-lined connection: total participation (1 ~ N)

#### Relationship Attributes

A relationship can also have attributes, e.g.: attributes that requires multiple entities. Example: Student-takes-Class: Grade.

#### Relationship Cardinality

The number of instances of an entity from a relation that can be associated with the relation.

* 1:1 cardinality: 1 student can only take 1 class
  * Put your foreign key attribute on the total participation side
* 1:N cardinality: 1 student can take N classes
  * Put your foreign key attribute on the N side so multiple entities can refer to a single other entity
* N:M cardinality: 1 student can take M classes, 1 class can have N students
  * You have to create a new table and use two foreign keys to create its primary key
  * When it so happens that both entities do not have total participation, you may assign the foreign keys to your own design
* Non-binary relationships are not discussed here.

> Relationship cardinality can affect database design. Thus, it is important to define relationship cardinality well.

