# company-analysis-with-sql

--Problem Statement:
--You are tasked with analyzing the employee data of a company to gain insights into various aspects 
--of its workforce. The dataset provided contains information about employees, their departments, 
--salaries, titles, and managerial positions. Data set schema is available in excel file.
--Using SQL queries and data manipulation techniques, you are expected to extract relevant 
--information from the dataset and present meaningful insights to support decision-making processes 
--within the organization.
--Here is the list of 30 Questions:
--1. Retrieve the first name and last name of all employees.
SELECT first_name, last_name
FROM [company techassignment].[dbo].[Employees]
--2. Find the department numbers and names.
SELECT TOP (1000) [dep_no]
      ,[dep_name]
  FROM [company techassignment].[dbo].[department]
--3. Get the total number of employees.
SELECT COUNT(DISTINCT emp_no)
FROM [company techassignment].[dbo].[Employees]
--4. Find the average salary of all employees.
SELECT  AVG (salary) AS AVG_SALARY
FROM [company techassignment].[dbo].[salaries]
--5. Retrieve the birth date and hire date of employee with emp_no 10003.
SELECT birth_date, hire_date
FROM  [company techassignment].[dbo].[Employees]
WHERE emp_no='10003'
--SQL Server
--In SQL Server, you can use the CONVERT function or the CAST function to format the date.
SELECT CONVERT(date, birth_date ) AS formatted_date
FROM [company techassignment].[dbo].[Employees]

SELECT CAST(birth_date AS date ) AS formatted_date
FROM [company techassignment].[dbo].[Employees]   
--6. Find the titles of all employees.
SELECT emp_no,title
FROM [company techassignment].[dbo].[employee_titles]

--7. Get the total number of departments.
SELECT count(dep_no) AS NO_OF_DEPARTMENTS
FROM [company techassignment].[dbo].[department]
--8. Retrieve the department number and name where employee with emp_no 10004 works.
SELECT dep_name, dep_no, emp_no
FROM [company techassignment].[dbo].[department] as dp
JOIN  [company techassignment].[dbo].[dept_emp] AS de
ON dp.dep_no =de.dept_no
WHERE emp_no ='10004'

--9. Find the gender of employee with emp_no 10007.
SELECT  gender
FROM [company techassignment].[dbo].[Employees]
WHERE emp_no='10007'
--10. Get the highest salary among all employees.
SELECT MAX(salary)
FROM [company techassignment].[dbo].[salaries]
--11. Retrieve the names of all managers along with their department names.
SELECT first_name, last_name, dep_name
FROM [company techassignment].[dbo].[Employees] as E
INNER JOIN [company techassignment].[dbo].[dept_manager] AS M ON E.emp_no= M.emp_no
INNER JOIN [company techassignment].[dbo].[department] AS D ON d.dep_no=M.dept_no

--12. Find the department with the highest number of employees.
SELECT dept_no,  COUNT(*) AS num_employees
 FROM [company techassignment].[dbo].[dept_emp]
 GROUP BY dept_no
ORDER BY num_employees DESC

--13. Retrieve the employee number, first name, last name, and salary of employees earning more
--than $60,000.
SELECT first_name, last_name, salary
FROM [company techassignment].[dbo].[Employees] as E
join [company techassignment].[dbo].[salaries] as S
ON E.emp_no=S.emp_no
WHERE salary > '60000'
--14. Get the average salary for each department.
SELECT dep_name, AVG(salary) AS AVG_SALARY
FROM [company techassignment].[dbo].[salaries], [company techassignment].[dbo].[department]
GROUP BY dep_name
ORDER BY AVG_SALARY
--15. Retrieve the employee number, first name, last name, and title of all employees who are
--currently managers

SELECT e.emp_no, e.first_name, e.last_name, t.title
FROM [company techassignment].[dbo].[Employees] AS E
INNER JOIN [company techassignment].[dbo].[employee_titles] AS T
ON e.emp_no = t.emp_no -- Join employees with their titles
WHERE t.title = 'Manager' AND t.to_date = '1/1/9999'; -- Filter for current managers

--16. Find the total number of employees in each department.
SELECT dept_no, count(emp_no) as totalemployees
FROM [company techassignment].[dbo].[dept_emp]
group by dept_no
order by totalemployees

--17. Retrieve the department number and name where the most recently hired employee works.
SELECT dep_no, dep_name, MAX(hire_date) as most_recent_hired_emplyee
  FROM [company techassignment].[dbo].[department], [company techassignment].[dbo].[Employees] 
  GROUP BY dep_no, dep_name
  ORDER BY max(hire_date)
--18. Get the department number, name, and average salary for departments with more than 3
--employees.
 SELECT  dep_no, dep_name, avg(salary)
  FROM [company techassignment].[dbo].[department], [company techassignment].[dbo].[salaries]
  WHERE dep_no >'3'
  group by dep_no, dep_name
  order by avg(salary)

--19. Retrieve the employee number, first name, last name, and title of all employees hired in 2005.
SELECT E.emp_no, E.first_name, E.last_name, E.hire_date, T.title
FROM [company techassignment].[dbo].[Employees] AS E
INNER JOIN [company techassignment].[dbo].[employee_titles] AS T
ON  e.emp_no = t.emp_no
WHERE YEAR(E.hire_date) ='2005'
--20. Find the department with the highest average salary.
SELECT  dep_name, AVG (salary) AS average_salary
  FROM [company techassignment].[dbo].[department], 
 [company techassignment].[dbo].[salaries] 
 GROUP BY dep_name
 ORDER BY average_salary DESC
 

--21. Retrieve the employee number, first name, last name, and salary of employees hired before the
--year 2005.
SELECT E.emp_no, E.first_name, E.last_name, E.hire_date, S.salary
FROM [company techassignment].[dbo].[Employees] AS E
INNER JOIN [company techassignment].[dbo].[salaries] S
ON  e.emp_no = S.emp_no
WHERE YEAR(E.hire_date) <'2005'

--22. Get the department number, name, and total number of employees for departments with a
--female manager.
SELECT COUNT(E.emp_no ), D.dep_name, D.dep_no
FROM 
    [company techassignment].[dbo].[Employees] AS E
INNER JOIN 
    [company techassignment].[dbo].[employee_titles] AS T
ON 
    E.emp_no = T.emp_no
CROSS JOIN 
    [company techassignment].[dbo].[department] AS D

WHERE gender='F' AND T.title = 'Manager'
GROUP BY  D.dep_name, D.dep_no
ORDER BY D.dep_no


--23. Retrieve the employee number, first name, last name, and department name of employees who
--are currently working in the Finance department.
SELECT e.emp_no, e.first_name, e.last_name, d.dep_name
FROM   [company techassignment].[dbo].[Employees] as e
JOIn [company techassignment].[dbo].[dept_emp] de ON e.emp_no = de.emp_no
JOIN [company techassignment].[dbo].[department] as d ON de.dept_no = d.dep_no
WHERE d.dep_name = 'Finance'
  AND de.to_date = '9999-01-01';

--24. Find the employee with the highest salary in each department.
SELECT E.emp_no, E.first_name, E.last_name, MAX(S.salary)
FROM [company techassignment].[dbo].[Employees] AS E
INNER JOIN [company techassignment].[dbo].[salaries] AS S
ON E.emp_no=S.emp_no
CROSS JOIN 
    [company techassignment].[dbo].[department] AS D
GROUP BY E.emp_no, E.first_name, E.last_name
ORDER BY E.emp_no
--CORRECT
WITH DepartmentSalaries AS (
    SELECT e.emp_no, e.first_name, e.last_name, d.dep_name, s.salary,
           ROW_NUMBER() OVER (PARTITION BY d.dep_name ORDER BY s.salary DESC) AS rn
    FROM  [company techassignment].[dbo].[Employees] e
    JOIN  [company techassignment].[dbo].[dept_emp] de ON e.emp_no = de.emp_no
    JOIN [company techassignment].[dbo].[salaries] s ON e.emp_no = s.emp_no
    JOIN [company techassignment].[dbo].[department] d ON de.dept_no = d.dep_no
    WHERE de.to_date = '9999-01-01' AND s.to_date = '9999-01-01'
)
SELECT emp_no, first_name, last_name, dep_name, salary
FROM DepartmentSalaries
WHERE rn = 1;

--25. Retrieve the employee number, first name, last name, and department name of employees who
--have held a managerial position.
SELECT e.emp_no, e.first_name, e.last_name, d.dep_name
FROM [company techassignment].[dbo].[Employees]  e
JOIN [company techassignment].[dbo].[dept_manager] dm ON e.emp_no = dm.emp_no
JOIN [company techassignment].[dbo].[department] d ON dm.dept_no = d.dep_no;


--26. Get the total number of employees who have held the title "Senior Manager."
SELECT COUNT(DISTINCT emp_no) AS total_senior_managers
FROM [company techassignment].[dbo].[employee_titles]
WHERE title = 'Senior Manager';

--27. Retrieve the department number, name, and the number of employees who have worked there
--for more than 5 years.

SELECT d.dep_no, d.dep_name, COUNT(de.emp_no) AS num_employees
FROM [company techassignment].[dbo].[department] d
JOIN [company techassignment].[dbo].[dept_emp] de ON d.dep_no = de.dept_no
JOIN [company techassignment].[dbo].[Employees] e ON de.emp_no = e.emp_no
WHERE DATEDIFF(YEAR, de.from_date, GETDATE()) >= 5
GROUP BY d.dep_no, d.dep_name;

--28. Find the employee with the longest tenure in the company

SELECT emp_no, first_name ,last_name, hire_date,
       DATEDIFF(day, hire_date, GETDATE()) AS tenure_days
FROM [company techassignment].[dbo].[Employees]
ORDER BY tenure_days DESC
OFFSET 0 ROWS FETCH FIRST 1 ROWS ONLY;


--29. Retrieve the employee number, first name, last name, and title of employees whose hire date is
--between '2005-01-01' and '2006-01-01'.
SELECT e.emp_no, e.first_name, e.last_name, t.title
FROM [company techassignment].[dbo].[Employees] e
join [company techassignment].[dbo].[employee_titles] t
on e.emp_no=t.emp_no
WHERE hire_date BETWEEN '2005-01-01' AND '2006-01-01';

--30. Get the department number, name, and the oldest employee's birth date for each department
SELECT d.dep_name, d.dep_no, MIN(e.birth_date) AS oldest_birth_date
FROM   [company techassignment].[dbo].[department] d
JOIN [company techassignment].[dbo].[dept_emp] de ON d.dep_no = de.dept_no
JOIN [company techassignment].[dbo].[Employees] e ON de.emp_no = e.emp_no
GROUP BY d.dep_name, d.dep_no;
