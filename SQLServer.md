- [DATEADD](#dateadd)
- [GETDATE, GETUTCDATE](#getdate-getutcdate)
- [SUBSTRING](#substring)
  - [Syntax:](#syntax)
  - [Examples:](#examples)
    - [Basic Example:](#basic-example)
    - [Extract a Substring from a Column:](#extract-a-substring-from-a-column)
    - [Example with Table Data:](#example-with-table-data)
  - [Practical Use Case:](#practical-use-case)
- [Use SUBSTRING in WHERE](#use-substring-in-where)
  - [Example:](#example)
    - [Using a Sample Table:](#using-a-sample-table)
    - [SQL Query with `SUBSTRING` in `WHERE` Clause:](#sql-query-with-substring-in-where-clause)
  - [Explanation:](#explanation)
    - [Result:](#result)
- [Truncate vs Drop](#truncate-vs-drop)
  - [`TRUNCATE`](#truncate)
  - [Example:](#example-1)
  - [`DROP TABLE`](#drop-table)
  - [Example:](#example-2)
  - [Key Differences](#key-differences)
  - [Summary Table](#summary-table)
  - [Example Scenario](#example-scenario)
- [Q. select top n-th salary](#q-select-top-n-th-salary)
- [rank vs dense\_rank sql server](#rank-vs-dense_rank-sql-server)
- [When should I use RANK versus DENSE\_RANK?](#when-should-i-use-rank-versus-dense_rank)
- [paginated search](#paginated-search)
  - [**Example Query:**](#example-query)
  - [**Explanation:**](#explanation-1)
  - [**Use Case:**](#use-case)
  - [**Dynamic Pagination:**](#dynamic-pagination)
  - [**Explanation:**](#explanation-2)
  - [**Why Use `OFFSET` and `FETCH NEXT`:**](#why-use-offset-and-fetch-next)
- [Questions on CTE](#questions-on-cte)
- [an interview scenario.](#an-interview-scenario)
  - [Problem: Employee Hierarchy and Salary Analysis](#problem-employee-hierarchy-and-salary-analysis)
  - [Task:](#task)
- [salaries).](#salaries)
  - [Expected Output:](#expected-output)
- [60000.00    |](#6000000----)
  - [Solution Hint:](#solution-hint)
- [`DepartmentName`.](#departmentname)
  - [Suggested Query Skeleton:](#suggested-query-skeleton)

## DATEADD
```sql
DATEADD(DAY, 1, '2025-01-01')
```

## GETDATE, GETUTCDATE
```sql
SELECT GETDATE()
SELECT GETUTCDATE()
```

## SUBSTRING
The `SUBSTRING` function in SQL Server is used to extract a portion of a string. You specify the string, the starting position, and the length of the substring you want to extract.

### Syntax:
```sql
SUBSTRING(expression, start, length)
```

- **expression**: The string from which the substring will be extracted.
- **start**: The starting position of the substring (1-based index).
- **length**: The number of characters to extract from the string.

### Examples:

#### Basic Example:
```sql
SELECT SUBSTRING('Hello, World!', 8, 5) AS ExtractedString;
```
- **Result**: `World` (extracts 5 characters starting from the 8th character).

#### Extract a Substring from a Column:
```sql
SELECT SUBSTRING(Name, 1, 3) AS ShortName
FROM Customers;
```
- **Explanation**: This query extracts the first three characters from the `Name` column for each row in the `Customers` table.

#### Example with Table Data:
Suppose you have a table `Employees` with a column `FullName`, and you want to extract the first name and last name separately:

```sql
CREATE TABLE Employees (
    EmployeeID INT,
    FullName VARCHAR(100)
);

INSERT INTO Employees (EmployeeID, FullName)
VALUES (1, 'John Smith'), (2, 'Jane Doe');

SELECT 
    SUBSTRING(FullName, 1, CHARINDEX(' ', FullName) - 1) AS FirstName,
    SUBSTRING(FullName, CHARINDEX(' ', FullName) + 1, LEN(FullName)) AS LastName
FROM Employees;
```
- **Explanation**:
  - `FirstName`: Extracts the substring from the start of `FullName` to the first space character.
  - `LastName`: Extracts the substring from the first space character to the end of `FullName`.

### Practical Use Case:
Extracting the area code from a phone number stored as a string:
```sql
SELECT SUBSTRING(PhoneNumber, 1, 3) AS AreaCode
FROM Contacts;
```
- **Explanation**: Assumes `PhoneNumber` is in the format `123-456-7890` and extracts the first three characters as the area code.

These examples should give you a good understanding of how to use the `SUBSTRING` function in SQL Server.

## Use SUBSTRING in WHERE
You can use the `SUBSTRING` function in the `WHERE` clause to filter results based on a substring of a column's value. Here's an example of how to use `SUBSTRING` in the `WHERE` clause in SQL Server:

### Example:

#### Using a Sample Table:
Suppose we have a table named `Employees` with a column `EmployeeCode` that contains strings, and we want to filter records where the substring of `EmployeeCode` starts with certain characters.

```sql
CREATE TABLE Employees (
    EmployeeID INT PRIMARY KEY,
    EmployeeCode VARCHAR(10),
    Name VARCHAR(100)
);

INSERT INTO Employees (EmployeeID, EmployeeCode, Name)
VALUES 
(1, 'ABC123', 'John Doe'),
(2, 'DEF456', 'Jane Smith'),
(3, 'ABC789', 'Alice Johnson');
```

#### SQL Query with `SUBSTRING` in `WHERE` Clause:
We want to filter employees whose `EmployeeCode` starts with 'ABC'.

```sql
SELECT EmployeeID, EmployeeCode, Name
FROM Employees
WHERE SUBSTRING(EmployeeCode, 1, 3) = 'ABC';
```

### Explanation:
- **Table Setup**: Creates a sample `Employees` table and inserts some records.
- **Query**:
  - `SELECT EmployeeID, EmployeeCode, Name`: Selects the columns we want to retrieve.
  - `WHERE SUBSTRING(EmployeeCode, 1, 3) = 'ABC'`: Uses the `SUBSTRING` function to extract the first three characters of `EmployeeCode` and compares it to 'ABC'.
  
#### Result:
```plaintext
EmployeeID | EmployeeCode | Name
---------------------------------
1          | ABC123       | John Doe
3          | ABC789       | Alice Johnson
```

In this example, the query retrieves employees whose `EmployeeCode` starts with 'ABC'.

Using `SUBSTRING` in the `WHERE` clause can be very powerful when you need to filter results based on parts of a string.

## Truncate vs Drop
The `TRUNCATE` and `DROP TABLE` commands in SQL Server are used for different purposes. Here's a comparison of both commands:

### `TRUNCATE`
- **Purpose**: Removes all rows from a table, but the table structure and its columns, constraints, indexes, and so on remain.
- **Usage**: Often used when you want to delete all data in a table quickly.
- **Effect on Data**: Deletes all rows, but the table structure remains intact.
- **Transaction Log**: Generates less transaction log data than `DELETE` because it deallocates data pages rather than deleting rows one by one.
- **Identity Columns**: Resets the identity value back to the seed value (if there is an identity column in the table).
- **Constraints and Triggers**: Cannot be used if the table is referenced by a foreign key constraint. Does not fire any triggers.

### Example:
```sql
TRUNCATE TABLE Employees;
```

### `DROP TABLE`
- **Purpose**: Completely removes a table and all its data, as well as the table structure, constraints, triggers, and indexes.
- **Usage**: Used when you want to completely remove a table from the database.
- **Effect on Data**: Deletes all rows and the table itself, including its structure.
- **Transaction Log**: Generates more transaction log data compared to `TRUNCATE` because it records the deletion of table metadata and its data.
- **Constraints and Triggers**: The table cannot be referenced by foreign key constraints when using `DROP TABLE`. All triggers, indexes, and constraints associated with the table are also removed.

### Example:
```sql
DROP TABLE Employees;
```

### Key Differences
- **Data Removal**: `TRUNCATE` removes all rows, but keeps the table structure; `DROP TABLE` removes the table itself.
- **Speed**: `TRUNCATE` is generally faster than `DROP TABLE` because it deallocates the data pages used to store the table's data.
- **Identity Reset**: `TRUNCATE` resets identity columns, while `DROP TABLE` does not have this concept since the table is removed.
- **Constraints**: `TRUNCATE` cannot be used on tables that are referenced by a foreign key constraint, while `DROP TABLE` can be used to drop such tables after removing the foreign key constraints.

### Summary Table
| Feature                      | TRUNCATE                        | DROP TABLE                          |
|------------------------------|---------------------------------|-------------------------------------|
| Removes Data                 | Yes                             | Yes                                 |
| Removes Table Structure      | No                              | Yes                                 |
| Speed                        | Generally faster                | Slower                              |
| Resets Identity Column       | Yes                             | N/A                                 |
| Constraints                  | Cannot be used if referenced    | Can be used after removing constraints |
| Triggers                     | Does not fire                   | N/A                                 |

### Example Scenario
**Scenario**: You have a table named `Employees` and you want to clear all its data but keep the table structure for future use.

- **Using TRUNCATE**:
  ```sql
  TRUNCATE TABLE Employees;
  ```

**Scenario**: You want to completely remove the `Employees` table and all its data, including its structure, constraints, and indexes.

- **Using DROP TABLE**:
  ```sql
  DROP TABLE Employees;
  ```

Each command has its use cases, and it's important to choose the right one based on your needs.

## Q. select top n-th salary
```sql
Select top 1 t1.salary from
(Select distinct top n salary from employees
Order by salary desc) t1
Order by salary asc
```

```sql
Select top 1 salary from
(Select salary, DENSE_RANK() over (order by salary desc) as rank
From employee) t1
Where rank = 2;
```

## rank vs dense_rank sql server
Ah, diving into the world of SQL Server rankings! Let's break down the difference between `RANK` and `DENSE_RANK`:

1. **RANK**: This function assigns a unique rank to each distinct value in the result set. If there are ties, it leaves gaps in the ranking sequence.

   For example:
   ```
   Value | RANK
   ------|-----
     10  |  1
     20  |  2
     20  |  2
     30  |  4
   ```

2. **DENSE_RANK**: This function also assigns a unique rank to each distinct value, but it does not leave gaps in the ranking sequence when there are ties.

   For example:
   ```
   Value | DENSE_RANK
   ------|---------
     10  |    1
     20  |    2
     20  |    2
     30  |    3
   ```

In summary, both functions rank your result set based on the order you specify, but `RANK` leaves gaps for ties, whereas `DENSE_RANK` does not.

## When should I use RANK versus DENSE_RANK?
Great question! The choice between `RANK` and `DENSE_RANK` depends on what you're trying to achieve with your ranking:

**Use `RANK` when:**
- You want to know the exact position of each item in your dataset, and gaps between ranks are acceptable when there are ties.
- You're interested in seeing how items stack up against one another, even if it means some positions will be skipped. For example, in a competition setting where you want to show the actual rank order, including ties.

**Use `DENSE_RANK` when:**
- You need a continuous ranking without gaps, even if there are ties.
- It's important to maintain a sequence where the next rank follows immediately after the previous one, irrespective of ties. For example, when creating a leaderboard where you want a clear, unbroken sequence of ranks.

Here's a quick comparison:
- **RANK**: Allows gaps in ranks when there are ties (useful for accurate positioning).
- **DENSE_RANK**: No gaps in ranks, even with ties (useful for continuous ranking).

## paginated search
In SQL Server, **pagination** can be achieved using the **`OFFSET`** and **`FETCH NEXT`** clauses. Here's how to do it:

### **Example Query:**
```sql
SELECT * 
FROM occupations
ORDER BY name ASC
OFFSET 10 ROWS             -- Skip the first 10 rows
FETCH NEXT 10 ROWS ONLY;   -- Get the next 10 rows
```

### **Explanation:**
- **`ORDER BY name ASC`** → Required for using `OFFSET` and `FETCH NEXT`. Ensures consistent row ordering.
- **`OFFSET 10 ROWS`** → Skips the first 10 rows.
- **`FETCH NEXT 10 ROWS ONLY`** → Returns the next 10 rows.

### **Use Case:**
- To get the **first page** (rows 1-10):  
  ```sql
  OFFSET 0 ROWS FETCH NEXT 10 ROWS ONLY;
  ```
- To get the **second page** (rows 11-20):  
  ```sql
  OFFSET 10 ROWS FETCH NEXT 10 ROWS ONLY;
  ```

### **Dynamic Pagination:**
You can use variables for **page number** and **page size**:
```sql
DECLARE @PageNumber INT = 2;
DECLARE @PageSize INT = 10;

SELECT * 
FROM occupations
ORDER BY name ASC
OFFSET (@PageNumber - 1) * @PageSize ROWS 
FETCH NEXT @PageSize ROWS ONLY;
```

### **Explanation:**
- **`@PageNumber - 1`** → Zero-based indexing for pagination.
- This example **gets rows 11-20** because `PageNumber` is 2 and `PageSize` is 10.

### **Why Use `OFFSET` and `FETCH NEXT`:**
- It's **efficient** and **clean** compared to older methods like `ROW_NUMBER()` or `TOP`.

---
## Questions on CTE

Here’s a **T-SQL problem** designed to test your knowledge of
**CTEs**, **aggregation**, **joins**, and **recursion**, perfect for
an interview scenario.
---
### Problem: Employee Hierarchy and Salary Analysis
You are given the following tables:
1. **Employees**
  ```sql
  CREATE TABLE Employees (
     EmployeeID INT PRIMARY KEY,
     Name NVARCHAR(50),
     ManagerID INT NULL, -- References EmployeeID in the same
table
     DepartmentID INT,
     Salary DECIMAL(10, 2)
  );
  ```
  Example Data:
  ```sql
  INSERT INTO Employees (EmployeeID, Name, ManagerID, DepartmentID,
Salary) VALUES
  (1, 'Alice', NULL, 101, 120000.00), -- CEO (no manager)
  (2, 'Bob', 1, 101, 80000.00),       -- Reports to Alice
  (3, 'Charlie', 1, 102, 90000.00),   -- Reports to Alice
  (4, 'David', 2, 101, 70000.00),     -- Reports to Bob
  (5, 'Eve', 2, 103, 60000.00);       -- Reports to Bob
  ```
2. **Departments**
  ```sql
  CREATE TABLE Departments (
     DepartmentID INT PRIMARY KEY,
     DepartmentName NVARCHAR(50)
  );
  ```
  Example Data:
  ```sql
  INSERT INTO Departments (DepartmentID, DepartmentName) VALUES
  (101, 'Engineering'),
  (102, 'HR'),
  (103, 'Finance');
  ```
---
### Task:
Write a query to:
1. Use a **recursive CTE** to find the full **hierarchy of
employees**, starting from the CEO.
2. Join with the **Departments** table to include the
`DepartmentName` for each employee.
3. Calculate the **total salary** managed by each employee
(including their subordinates).
4. For each employee, show the following columns:
  - `EmployeeID`, `Name`, `ManagerID`, `DepartmentName`, `Level`
(hierarchy level starting from 1 for the CEO),
  - `TotalSalary` (sum of their salary and all their subordinates'
salaries).
---
### Expected Output:
For the given data, the output should look like this:
| EmployeeID | Name      | ManagerID | DepartmentName | Level |
TotalSalary |
|------------|-----------|-----------|----------------|-------|-----
--------|
| 1          | Alice     | NULL      | Engineering    | 1     |
420000.00   |
| 2          | Bob       | 1         | Engineering    | 2     |
210000.00   |
| 3          | Charlie   | 1         | HR             | 2     |
90000.00    |
| 4          | David     | 2         | Engineering    | 3     |
70000.00    |
| 5          | Eve       | 2         | Finance        | 3     |
60000.00    |
---
### Solution Hint:
To solve this:
1. Use a **recursive CTE** to calculate the hierarchy levels.
  - The base case will start with employees who don’t have a
manager (`ManagerID IS NULL`).
  - The recursive step will join the CTE with the `Employees` table
to find subordinates.
2. Use an **aggregate function** (e.g., `SUM`) with a **GROUP BY**
to calculate the `TotalSalary` for each employee.
3. Use a **JOIN** with the `Departments` table to include the
`DepartmentName`.
---
### Suggested Query Skeleton:
```sql
WITH EmployeeHierarchy AS (
   -- Base case: CEO (employees with no manager)
   SELECT
      e.EmployeeID,
      e.Name,
      e.ManagerID,
      e.DepartmentID,
      e.Salary,
      1 AS Level
   FROM Employees e
   WHERE e.ManagerID IS NULL
   UNION ALL
   -- Recursive case: Employees reporting to the ones in the
previous level
   SELECT
      e.EmployeeID,
      e.Name,
      e.ManagerID,
      e.DepartmentID,
      e.Salary,
      eh.Level + 1 AS Level
   FROM Employees e
   INNER JOIN EmployeeHierarchy eh
      ON e.ManagerID = eh.EmployeeID
)
-- Calculate TotalSalary and join with Departments
SELECT
   eh.EmployeeID,
   eh.Name,
   eh.ManagerID,
   d.DepartmentName,
   eh.Level,
   SUM(eh2.Salary) AS TotalSalary
FROM EmployeeHierarchy eh
LEFT JOIN Departments d
   ON eh.DepartmentID = d.DepartmentID
LEFT JOIN EmployeeHierarchy eh2
   ON eh.EmployeeID = eh2.ManagerID OR eh.EmployeeID =
eh2.EmployeeID
GROUP BY
   eh.EmployeeID, eh.Name, eh.ManagerID, d.DepartmentName, eh.Level
ORDER BY
   eh.Level, eh.EmployeeID;
```
---
# Scenarios where self-join is used
### **Scenarios Where Self-Joins are Useful in SQL:**

A **self-join** is when a table is joined with itself. This is useful when you need to compare rows within the same table. Here are some common scenarios with examples:

---

## **1. Hierarchical Data (Employee-Manager Relationship)**
### **Scenario:**  
You have an **Employees** table where each employee has a `ManagerID` pointing to another `EmployeeID`. You want to find each employee's manager's name.

### **Table Structure:**
```plaintext
Employees:
+------------+----------+-----------+
| EmployeeID | Name     | ManagerID |
+------------+----------+-----------+
| 1          | Alice    | NULL      |
| 2          | Bob      | 1         |
| 3          | Charlie  | 1         |
| 4          | David    | 2         |
+------------+----------+-----------+
```

### **Query:**
```sql
SELECT 
    e1.Name AS Employee,
    e2.Name AS Manager
FROM Employees e1
LEFT JOIN Employees e2 
    ON e1.ManagerID = e2.EmployeeID;
```

### **Output:**
```plaintext
+-----------+---------+
| Employee  | Manager |
+-----------+---------+
| Alice     | NULL    |
| Bob       | Alice   |
| Charlie   | Alice   |
| David     | Bob     |
+-----------+---------+
```

---

## **2. Finding Duplicates in a Table**
### **Scenario:**  
You want to find duplicate rows based on certain columns (e.g., Name and Email).

### **Table Structure:**
```plaintext
Users:
+----+--------+---------------+
| ID | Name   | Email         |
+----+--------+---------------+
| 1  | John   | john@email.com|
| 2  | Jane   | jane@email.com|
| 3  | John   | john@email.com|
| 4  | Mike   | mike@email.com|
+----+--------+---------------+
```

### **Query:**
```sql
SELECT 
    u1.ID,
    u1.Name,
    u1.Email
FROM Users u1
JOIN Users u2 
    ON u1.Name = u2.Name 
    AND u1.Email = u2.Email
    AND u1.ID < u2.ID;
```

### **Output:**
```plaintext
+----+--------+---------------+
| ID | Name   | Email         |
+----+--------+---------------+
| 1  | John   | john@email.com|
+----+--------+---------------+
```

---

## **3. Comparing Rows in the Same Table**
### **Scenario:**  
You want to find employees who have a higher salary than their manager.

### **Query:**
```sql
SELECT 
    e1.Name AS Employee,
    e1.Salary AS EmployeeSalary,
    e2.Name AS Manager,
    e2.Salary AS ManagerSalary
FROM Employees e1
JOIN Employees e2 
    ON e1.ManagerID = e2.EmployeeID
WHERE e1.Salary > e2.Salary;
```

---

## **4. Finding Sequential Records (e.g., Events or Logs)**
### **Scenario:**  
You need to find the time between consecutive events for the same user.

### **Table Structure:**
```plaintext
Events:
+--------+--------+---------------------+
| EventID| UserID | EventTime            |
+--------+--------+---------------------+
| 1      | 101    | 2025-02-14 10:00:00  |
| 2      | 101    | 2025-02-14 11:00:00  |
| 3      | 102    | 2025-02-14 09:30:00  |
| 4      | 101    | 2025-02-14 12:00:00  |
+--------+--------+---------------------+
```

### **Query:**
```sql
SELECT 
    e1.UserID,
    e1.EventTime AS CurrentEvent,
    e2.EventTime AS PreviousEvent,
    DATEDIFF(HOUR, e2.EventTime, e1.EventTime) AS TimeDifference
FROM Events e1
JOIN Events e2 
    ON e1.UserID = e2.UserID 
    AND e1.EventTime > e2.EventTime
ORDER BY e1.EventTime;
```

---

## **5. Combining Rows to Create Pairs**
### **Scenario:**  
You want to find all possible pairs of employees for a team-building exercise.

### **Query:**
```sql
SELECT 
    e1.Name AS Employee1,
    e2.Name AS Employee2
FROM Employees e1
JOIN Employees e2 
    ON e1.EmployeeID < e2.EmployeeID;
```

### **Output:**
```plaintext
+-----------+-----------+
| Employee1 | Employee2 |
+-----------+-----------+
| Alice     | Bob       |
| Alice     | Charlie   |
| Bob       | Charlie   |
+-----------+-----------+
```

---

## **6. Detecting Gaps in Sequential Data**
### **Scenario:**  
You want to find missing numbers in a sequential order (e.g., invoice numbers).

### **Query:**
```sql
SELECT 
    e1.InvoiceNumber + 1 AS MissingNumber
FROM Invoices e1
LEFT JOIN Invoices e2 
    ON e1.InvoiceNumber + 1 = e2.InvoiceNumber
WHERE e2.InvoiceNumber IS NULL;
```

---

### **Key Points to Remember:**
- **Alias** is mandatory in self-joins to distinguish between the same table referenced multiple times.
- Always use **appropriate join conditions** to avoid Cartesian products.
- **Performance Tip**: Ensure indexes are in place on the columns used in the join conditions to optimize performance.

---