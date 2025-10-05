# Elevate_Labs_Task6
Subqueries, Filtering

1. What is a subquery?

Definition:
A subquery is a query written inside another SQL query to provide data for it.

Explanation:

It is also called a nested query or inner query.

The outer query uses the result of the subquery.

Subqueries are commonly used in WHERE, FROM, or SELECT clauses.

Example:

SELECT EmployeeName
FROM Employees
WHERE DepartmentID = (
    SELECT DepartmentID FROM Departments WHERE DepartmentName = 'HR'
);


→ The inner query finds the DepartmentID of HR, and the outer query lists employees in that department.

In short:
A subquery is a query inside another query that helps fetch results dynamically.

2. Difference between subquery and join
Aspect	Subquery	Join
Definition	Query inside another query	Combines data from two or more tables directly
Execution	Inner query runs first, result is used by outer query	All tables are processed together
Performance	Sometimes slower for large data	Usually faster due to optimized join algorithms
Readability	Easier to understand for small queries	Better for complex data relationships
Use case	When you need to compute or filter data based on another query	When you need columns from multiple tables together

Example (Subquery):

SELECT * FROM Employees
WHERE DepartmentID IN (SELECT DepartmentID FROM Departments WHERE Location = 'Chennai');


Example (Join):

SELECT e.* FROM Employees e
JOIN Departments d ON e.DepartmentID = d.DepartmentID
WHERE d.Location = 'Chennai';


In short:
Subquery = query inside query; Join = directly combining tables.

3. What is a correlated subquery?

Definition:
A correlated subquery is a subquery that depends on values from the outer query for its execution.

Explanation:

The inner query runs once per row of the outer query.

It references a column from the outer query’s table.

Example:

SELECT e.EmployeeName
FROM Employees e
WHERE e.Salary > (
    SELECT AVG(Salary)
    FROM Employees
    WHERE DepartmentID = e.DepartmentID
);


→ For each employee, the subquery calculates the average salary of their department.

In short:
A correlated subquery runs repeatedly for each row of the outer query and depends on its values.

4. Can subqueries return multiple rows?

Answer:
Yes, subqueries can return multiple rows, depending on how they are used.

Explanation:

If used with IN, ANY, or ALL operators, multiple rows are allowed.

If used with =, it must return only one row, or you’ll get an error.

Example:

SELECT EmployeeName
FROM Employees
WHERE DepartmentID IN (
    SELECT DepartmentID FROM Departments WHERE Location = 'Hyderabad'
);


→ Subquery returns multiple DepartmentIDs.

In short:
Yes — multiple rows allowed if used with IN, ANY, or ALL.

5. How does EXISTS work?

Definition:
The EXISTS operator checks whether a subquery returns any rows.

Explanation:

It returns TRUE if the subquery finds at least one matching row, otherwise FALSE.

Often used for checking record existence efficiently.

Example:

SELECT DepartmentName
FROM Departments d
WHERE EXISTS (
    SELECT 1 FROM Employees e WHERE e.DepartmentID = d.DepartmentID
);


→ Lists departments that have at least one employee.

In short:
EXISTS checks if a subquery returns any result (TRUE/FALSE).

6. How is performance affected by subqueries?

Explanation:

Simple subqueries (especially uncorrelated) usually perform fine.

Correlated subqueries can slow performance because they run once per outer row.

Joins are often faster for large datasets because the database can optimize them better.

Optimization tips:
✅ Use joins instead of correlated subqueries when possible.
✅ Ensure indexes exist on filtering columns.
✅ Avoid deeply nested subqueries.

In short:
Uncorrelated subqueries are fine; correlated ones can slow performance.

7. What is a scalar subquery?

Definition:
A scalar subquery returns exactly one value (one row and one column).

Explanation:
It is often used inside a SELECT or WHERE clause where a single value is expected.

Example:

SELECT EmployeeName,
       (SELECT DepartmentName FROM Departments WHERE DepartmentID = e.DepartmentID) AS Department
FROM Employees e;


→ The subquery returns one DepartmentName per employee.

In short:
A scalar subquery returns a single value, like a function call inside another query.

8. Where can we use subqueries?

Answer:
Subqueries can be used in multiple SQL clauses:

WHERE – for filtering based on another query.

FROM – as a derived table or inline view.

SELECT – to return computed or related values.

HAVING – to filter grouped data.

INSERT/UPDATE/DELETE – to use another query’s results.

Example (in WHERE):

SELECT * FROM Employees
WHERE DepartmentID = (SELECT DepartmentID FROM Departments WHERE DepartmentName = 'HR');


In short:
Subqueries can appear in WHERE, FROM, SELECT, HAVING, or DML statements.

9. Can a subquery be in FROM clause?

Answer:
Yes, a subquery can be used in the FROM clause — this is called a derived table or inline view.

Example:

SELECT DepartmentID, AVG(Salary) AS AvgSalary
FROM (
    SELECT DepartmentID, Salary FROM Employees WHERE Salary > 30000
) AS HighEarners
GROUP BY DepartmentID;


→ The subquery (HighEarners) acts like a temporary table.

In short:
Yes — subqueries in FROM act as temporary tables for the outer query.

10. What is a derived table?

Definition:
A derived table is a temporary result set created by a subquery in the FROM clause.

Explanation:

It exists only while the main query runs.

You must give it an alias to reference its columns.

Example:

SELECT t.DepartmentID, COUNT(*) AS TotalEmployees
FROM (
    SELECT DepartmentID FROM Employees WHERE Salary > 40000
) AS t
GROUP BY t.DepartmentID;


→ The inner query forms a derived table t.

In short:
A derived table is a subquery in the FROM clause used like a temporary table
