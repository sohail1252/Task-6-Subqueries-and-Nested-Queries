# Task-6-Subqueries-and-Nested-Queries
CREATE TABLE employees (
emp_id INT PRIMARY KEY,
name VARCHAR(50),
department VARCHAR(50),
salary DECIMAL(10,2)
);

INSERT INTO employees (emp_id, name, department, salary) VALUES
(1, 'John',  'HR',    45000),
(2, 'Alice', 'HR',    52000),
(3, 'Raj',   'IT',    60000),
(4, 'Maria', 'IT',    62000),
(5, 'Ahmed', 'Sales', 40000),
(6, 'Priya', 'Sales', 55000),
(7, 'Karan', 'IT',    58000);

SELECT name, salary
FROM employees
WHERE salary > (
SELECT AVG(salary) FROM employees
);

SELECT name, department
FROM employees
WHERE department IN (
SELECT department
FROM employees
WHERE salary > 60000
);

SELECT e1.name, e1.department, e1.salary
FROM employees e1
WHERE NOT EXISTS (
SELECT 1
FROM employees e2
WHERE e2.department = e1.department
AND e2.salary > e1.salary
);
