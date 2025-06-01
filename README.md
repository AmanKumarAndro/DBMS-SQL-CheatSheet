# Interview - DBMS & SQL Cheat Sheet

## 📊 DATABASE FUNDAMENTALS

### What is a Database?
- **Database**: Organized collection of structured data stored electronically
- **DBMS**: Software that manages databases (MySQL, Oracle, PostgreSQL, etc.)
- **RDBMS**: Relational Database Management System using tables with rows and columns

### Key Database Terms
- **Table**: Collection of related data in rows and columns
- **Row/Tuple**: Single record in a table
- **Column/Attribute**: Field in a table
- **Primary Key**: Unique identifier for each row
- **Foreign Key**: Links two tables together
- **Schema**: Structure/blueprint of database

---

## 🔑 DATABASE KEYS

### Types of Keys
1. **Primary Key**
   - Uniquely identifies each record
   - Cannot be NULL
   - Only one per table

2. **Foreign Key**
   - References primary key of another table
   - Can have duplicates
   - Can be NULL

3. **Candidate Key**
   - Potential primary keys
   - Minimal set of attributes that uniquely identify records

4. **Super Key**
   - Set of attributes that uniquely identify records
   - Can have extra attributes

5. **Composite Key**
   - Primary key made of multiple columns

6. **Alternate Key**
   - Candidate keys not chosen as primary key

---

## 🎯 NORMALIZATION

### Normal Forms
1. **1NF (First Normal Form)**
   - Eliminate duplicate columns
   - Each cell contains single value
   - Each row is unique

2. **2NF (Second Normal Form)**
   - Must be in 1NF
   - Remove partial dependencies
   - Non-key attributes fully depend on primary key

3. **3NF (Third Normal Form)**
   - Must be in 2NF
   - Remove transitive dependencies
   - Non-key attributes don't depend on other non-key attributes

4. **BCNF (Boyce-Codd Normal Form)**
   - Stricter version of 3NF
   - Every determinant is a candidate key

### Benefits of Normalization
- Reduces data redundancy
- Eliminates update anomalies
- Saves storage space
- Maintains data integrity

---

## 💾 SQL BASICS

### SQL Categories
1. **DDL (Data Definition Language)**
   - CREATE, ALTER, DROP, TRUNCATE

2. **DML (Data Manipulation Language)**
   - SELECT, INSERT, UPDATE, DELETE

3. **DCL (Data Control Language)**
   - GRANT, REVOKE

4. **TCL (Transaction Control Language)**
   - COMMIT, ROLLBACK, SAVEPOINT

### Basic SQL Syntax
```sql
-- CREATE TABLE
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    salary DECIMAL(10,2),
    department_id INT
);

-- INSERT DATA
INSERT INTO employees (id, name, salary, department_id)
VALUES (1, 'John Doe', 50000, 1);

-- SELECT DATA
SELECT * FROM employees;
SELECT name, salary FROM employees WHERE salary > 40000;

-- UPDATE DATA
UPDATE employees SET salary = 55000 WHERE id = 1;

-- DELETE DATA
DELETE FROM employees WHERE id = 1;
```

---

## 🔍 SQL JOINS

### Types of Joins
1. **INNER JOIN**
   ```sql
   SELECT e.name, d.dept_name
   FROM employees e
   INNER JOIN departments d ON e.department_id = d.id;
   ```

2. **LEFT JOIN (LEFT OUTER JOIN)**
   ```sql
   SELECT e.name, d.dept_name
   FROM employees e
   LEFT JOIN departments d ON e.department_id = d.id;
   ```

3. **RIGHT JOIN (RIGHT OUTER JOIN)**
   ```sql
   SELECT e.name, d.dept_name
   FROM employees e
   RIGHT JOIN departments d ON e.department_id = d.id;
   ```

4. **FULL OUTER JOIN**
   ```sql
   SELECT e.name, d.dept_name
   FROM employees e
   FULL OUTER JOIN departments d ON e.department_id = d.id;
   ```

5. **SELF JOIN**
   ```sql
   SELECT e1.name, e2.name AS manager
   FROM employees e1
   JOIN employees e2 ON e1.manager_id = e2.id;
   ```

---

## 📈 SQL AGGREGATE FUNCTIONS

### Common Functions
```sql
-- COUNT
SELECT COUNT(*) FROM employees;
SELECT COUNT(DISTINCT department_id) FROM employees;

-- SUM
SELECT SUM(salary) FROM employees;

-- AVG
SELECT AVG(salary) FROM employees;

-- MAX/MIN
SELECT MAX(salary), MIN(salary) FROM employees;

-- GROUP BY
SELECT department_id, COUNT(*), AVG(salary)
FROM employees
GROUP BY department_id;

-- HAVING
SELECT department_id, AVG(salary)
FROM employees
GROUP BY department_id
HAVING AVG(salary) > 50000;
```

---

## 🎲 SQL CONSTRAINTS

### Types of Constraints
1. **NOT NULL**: Column cannot be empty
2. **UNIQUE**: All values must be unique
3. **PRIMARY KEY**: Combination of NOT NULL and UNIQUE
4. **FOREIGN KEY**: References another table's primary key
5. **CHECK**: Validates data based on condition
6. **DEFAULT**: Sets default value

```sql
CREATE TABLE employees (
    id INT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    email VARCHAR(100) UNIQUE,
    age INT CHECK (age >= 18),
    salary DECIMAL(10,2) DEFAULT 30000,
    department_id INT,
    FOREIGN KEY (department_id) REFERENCES departments(id)
);
```

---

## 🔄 TRANSACTIONS & ACID PROPERTIES

### ACID Properties
1. **Atomicity**: All operations succeed or all fail
2. **Consistency**: Database remains in valid state
3. **Isolation**: Concurrent transactions don't interfere
4. **Durability**: Committed changes are permanent

### Transaction Commands
```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT; -- or ROLLBACK;
```

---

## 🏛️ DATABASE ARCHITECTURE

### Three-Schema Architecture
1. **External/View Level**: User views
2. **Conceptual/Logical Level**: Overall database structure
3. **Internal/Physical Level**: How data is stored physically

### Data Independence
- **Logical Data Independence**: Changes in logical schema don't affect external schema
- **Physical Data Independence**: Changes in physical schema don't affect logical schema

---

## 📋 COMMON INTERVIEW QUESTIONS & ANSWERS

### Q1: What is the difference between DELETE, DROP, and TRUNCATE?
**Answer:**
- **DELETE**: Removes rows, can use WHERE clause, can be rolled back, slower
- **DROP**: Removes entire table structure and data, cannot be rolled back
- **TRUNCATE**: Removes all rows quickly, cannot use WHERE, cannot be rolled back, faster than DELETE

### Q2: What is the difference between Primary Key and Unique Key?
**Answer:**
- **Primary Key**: Only one per table, cannot be NULL, creates clustered index
- **Unique Key**: Multiple allowed per table, can be NULL (only one), creates non-clustered index

### Q3: What are the differences between SQL and NoSQL?
**Answer:**
- **SQL**: Structured, ACID compliant, fixed schema, vertical scaling, complex queries
- **NoSQL**: Flexible schema, horizontal scaling, eventual consistency, better for unstructured data

### Q4: What is a View in SQL?
**Answer:**
- Virtual table based on SQL query result
- Doesn't store data physically
- Provides security and simplifies complex queries
```sql
CREATE VIEW employee_view AS
SELECT name, salary FROM employees WHERE salary > 50000;
```

### Q5: What are Indexes?
**Answer:**
- Data structures that improve query performance
- **Clustered Index**: Physically reorders table data
- **Non-Clustered Index**: Separate structure pointing to table rows
- Trade-off: Faster SELECT but slower INSERT/UPDATE/DELETE

### Q6: What is the difference between HAVING and WHERE?
**Answer:**
- **WHERE**: Filters rows before grouping, cannot use aggregate functions
- **HAVING**: Filters groups after grouping, can use aggregate functions

### Q7: What are Stored Procedures?
**Answer:**
- Pre-compiled SQL code stored in database
- Benefits: Performance, security, code reusability
- Can accept parameters and return values

### Q8: What is Denormalization?
**Answer:**
- Process of adding redundancy to improve read performance
- Trade-off between storage space and query speed
- Used in data warehousing and reporting systems

### Q9: What are Database Triggers?
**Answer:**
- Special stored procedures that execute automatically
- Types: BEFORE, AFTER, INSTEAD OF
- Events: INSERT, UPDATE, DELETE
- Used for auditing, validation, automatic updates

### Q10: What is the difference between UNION and UNION ALL?
**Answer:**
- **UNION**: Combines results and removes duplicates, slower
- **UNION ALL**: Combines results keeping duplicates, faster

---

## 🎯 SQL OPTIMIZATION TIPS

### Performance Best Practices
1. Use indexes on frequently queried columns
2. Avoid SELECT * in production
3. Use LIMIT for large result sets
4. Optimize JOIN operations
5. Use EXISTS instead of IN for subqueries
6. Avoid functions in WHERE clauses
7. Use appropriate data types
8. Regular database maintenance (update statistics, rebuild indexes)

---

## 🔧 ADVANCED SQL CONCEPTS

### Window Functions
```sql
-- ROW_NUMBER
SELECT name, salary, 
       ROW_NUMBER() OVER (ORDER BY salary DESC) as rank
FROM employees;

-- RANK and DENSE_RANK
SELECT name, salary,
       RANK() OVER (ORDER BY salary DESC) as rank,
       DENSE_RANK() OVER (ORDER BY salary DESC) as dense_rank
FROM employees;
```

### Common Table Expressions (CTE)
```sql
WITH high_earners AS (
    SELECT * FROM employees WHERE salary > 50000
)
SELECT * FROM high_earners WHERE department_id = 1;
```

### Subqueries
```sql
-- Scalar Subquery
SELECT name FROM employees 
WHERE salary = (SELECT MAX(salary) FROM employees);

-- Correlated Subquery
SELECT name FROM employees e1
WHERE salary > (SELECT AVG(salary) FROM employees e2 
                WHERE e1.department_id = e2.department_id);
```

---

## 📚 QUICK REFERENCE

### Data Types (MySQL)
- **Numeric**: INT, DECIMAL, FLOAT, DOUBLE
- **String**: VARCHAR, CHAR, TEXT
- **Date/Time**: DATE, TIME, DATETIME, TIMESTAMP
- **Boolean**: BOOLEAN (TINYINT(1))

### String Functions
- CONCAT(), LENGTH(), SUBSTRING(), UPPER(), LOWER()
- TRIM(), REPLACE(), LEFT(), RIGHT()

### Date Functions
- NOW(), CURDATE(), CURTIME(), DATE_ADD(), DATE_SUB()
- YEAR(), MONTH(), DAY(), HOUR(), MINUTE(), SECOND()

### Logical Operators
- AND, OR, NOT, IN, BETWEEN, LIKE, IS NULL, EXISTS

---

## 💡 FINAL TIPS FOR INTERVIEW

1. **Practice Basic Queries**: Focus on SELECT, JOIN, GROUP BY, ORDER BY
2. **Understand Concepts**: Don't just memorize, understand the logic
3. **Know Differences**: Be clear about DELETE vs TRUNCATE, PRIMARY vs UNIQUE KEY
4. **Real-world Examples**: Think of practical applications
5. **Be Confident**: Explain your thought process clearly
6. **Ask Questions**: If unsure, ask for clarification

### Common Questions Pattern
- Basic SQL syntax and operations
- Difference between various concepts
- Normalization and its benefits
- Join types and when to use them
- Index usage and optimization
- Transaction concepts

**Good Luck with your Interview! 🚀**
