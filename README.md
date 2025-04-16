# INSERT-Laughs-INTO-Project
This project demonstrates the power of  SQL Window Functions on a fictional "Sales Dataset". Using queries enriched with `LAG()`, `LEAD()`, `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, and aggregate functions, we explore how modern SQL can simplify complex analytics with humor and clarity

# Collaborators 
# Baraka Johnson Bright ( 25583 )
# 

# 📘 Introduction
-
Welcome to INSERT Laughs INTO Project – a SQL-based exploration of window functions using a fictional yet realistic Sales Dataset. This project was designed to help us dive deep into powerful SQL analytical tools such as LAG(), LEAD(), RANK(), DENSE_RANK(), ROW_NUMBER(), and aggregate functions with PARTITION BY.

We built this project not only to demonstrate technical understanding but also to highlight how these functions apply to real-world business scenarios.




---

## 📥 Get on Hand
To begin the assignment, we created a simple dataset related to employees. The table  is created as follows:

```sql
CREATE TABLE employees (
    employee_id INT,
    name VARCHAR(50),
    department VARCHAR(50),
    salary INT,
    hire_date DATE
);
```

![alt text](<creation of Employee table-1.png>)

Then, we inserted  data into the table:

INSERT INTO employees VALUES 

```sql

INSERT ALL
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (1, 'Niyonzima', 'Finance', 800000, TO_DATE('2020-01-15', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (2, 'Uwase', 'Finance', 750000, TO_DATE('2021-06-12', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (3, 'Habimana', 'Finance', 900000, TO_DATE('2019-03-08', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (4, 'Mugisha', 'IT', 950000, TO_DATE('2018-07-22', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (5, 'Ingabire', 'IT', 850000, TO_DATE('2021-01-30', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (6, 'Tuyishime', 'IT', 800000, TO_DATE('2020-10-10', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (7, 'Uwimana', 'HR', 700000, TO_DATE('2020-04-14', 'YYYY-MM-DD'))
    INTO employees (employee_id, name, department, salary, hire_date) 
    VALUES (8, 'Ishimwe', 'HR', 720000, TO_DATE('2021-08-01', 'YYYY-MM-DD'))
SELECT * FROM dual;

```

![alt text](<Insertion of sample datas.png>)

---

## 🔍 SQL Queries and Explanations

### 1. Compare Salaries Using `LAG()` and `LEAD()`

To analyze how each employee's salary compares to others, we use the LAG() and LEAD() window functions. LAG(salary) retrieves the salary of the previous employee in the ordered list, while LEAD(salary) retrieves the salary of the next employee. By using a CASE statement, we label whether the current employee's salary is HIGHER, LOWER, or EQUAL compared to the previous one. This method helps to understand salary progression and identify significant differences between employees in terms of pay.

```sql

SELECT 
    name, department, salary,
    LAG(salary) OVER (ORDER BY salary) AS previous_salary,
    LEAD(salary) OVER (ORDER BY salary) AS next_salary,
    CASE 
        WHEN salary > LAG(salary) OVER (ORDER BY salary) THEN 'HIGHER'
        WHEN salary < LAG(salary) OVER (ORDER BY salary) THEN 'LOWER'
        ELSE 'EQUAL'
    END AS comparison_with_previous
FROM employees;
```
![alt text](<Comparison  of Salaries Using LAG() and LEAD().png>)



**Explanation:** This compares each employee’s salary with the one before and after them in the list.

---

### 2. Rank Within Departments (`RANK()` and `DENSE_RANK()`)

To rank employees based on their salary within each department, we use RANK() and DENSE_RANK() functions. Both functions partition the data by department and order it by salary in descending order. The difference is that RANK() skips ranking numbers when there are ties, while DENSE_RANK() assigns consecutive numbers even if salaries are equal. For example, if two employees are tied at rank 1, RANK() will give the next one rank 3, while DENSE_RANK() will assign rank 2. These rankings help evaluate employee positions fairly within each department.


```sql
SELECT 
    name, department, salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_method,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank_method
FROM employees;
```

![alt text](<Rank Within Departments Using RANK and DENSE_RANK.png>)


**Explanation:** This ranks employees by salary **within their department**.

---

### 3. Top 3 Paid Employees Per Department

To find the top 3 highest-paid employees in each department, we use a subquery with RANK() over a partition by department and ordered by salary descending. We then filter the results where the rank is less than or equal to 3. This ensures that we correctly capture the top earners in each group, even if there are salary ties. Using ranking functions like RANK() allows us to handle duplicate values appropriately, ensuring fairness in the results.


```sql
SELECT *
FROM (
    SELECT *, 
           RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
    FROM employees
)
WHERE rnk <= 3;
```

![alt text](<Top 3 Paid Employees Per Department.png>)


**Explanation:** Helps HR know the highest paid 3 employees in each department.

---

### 4. First 2 Employees Per Department by Hire Date

To identify the first two employees hired in each department, we use ROW_NUMBER() over a partition by department and ordered by hire_date in ascending order. ROW_NUMBER() assigns a unique sequence to each employee based on when they joined. We then filter where the row number is less than or equal to 2 to get only the first two hires. This approach helps trace departmental history and gives insight into long-term staff members.

```sql
SELECT *
FROM (
    SELECT *, 
           ROW_NUMBER() OVER (PARTITION BY department ORDER BY hire_date ASC) AS row_num
    FROM employees
)
WHERE row_num <= 2;
```

![alt text](<First 2 Employees Per Department (based on hire_date).png>)


**Explanation:** Finds the earliest 2 employees hired in each department.

---

### 5. Max Salary Per Department and Overall

For this task, we calculate both the maximum salary within each department and the overall maximum salary across the entire company using the MAX() window function. MAX(salary) OVER (PARTITION BY department) returns the highest salary within each department, while MAX(salary) OVER () returns the overall maximum. By using PARTITION BY, we can separate results by category (departments) and also assess company-wide statistics, all while keeping full detail in the query output.


```sql
SELECT 
    name, department, salary,
    MAX(salary) OVER (PARTITION BY department) AS max_per_department,
    MAX(salary) OVER () AS overall_max
FROM employees;

```
![alt text](<Maximum Salary Per Department and Overall.png>)



**Explanation:** Displays the highest salary in each department and overall.

---

## 📌 Findings

- `Mugisha` has the highest salary (950,000 RWF) in the IT department.
- The Finance department has a salary range between 750,000 and 900,000 RWF.
- We can clearly track promotions or changes in salary ranking over time.
- Hiring trends show that most HR employees were hired after 2020.

---

## 🌍 Real-Life Applications

- **HR Systems:** Track top performers and salary fairness.
- **Payroll Auditing:** Compare salaries with history.
- **Hiring Reports:** Identify longest-serving employees.
- **Business Analytics:** Help with budgeting and planning.

---

### 🧠 Conclusion

In this assignment, we used SQL window functions to get deeper insights into employee data. We saw how RANK(), ROW_NUMBER(), LAG(), and LEAD() can help us understand salary comparisons, employee rankings, and hiring patterns across departments.

This kind of analysis is very useful in real companies. It helps managers make smart decisions about promotions, hiring, and salaries. Even though we used simple data, these techniques are used in big organizations to manage thousands of employees.

Learning how to use window functions is an important skill for anyone interested in data analysis, human resources, or software development.

👏 Thank you for reading!
