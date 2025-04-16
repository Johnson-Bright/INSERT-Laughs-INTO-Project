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
    name VARCHAR(70),
    department VARCHAR(70),
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

## 🔍 SQL Queries with their Descriptions 

### 1. Comparison between salaries using `LAG()` and `LEAD()`

![alt text](<Comparison  of Salaries Using LAG() and LEAD().png>)



- Used `LAG()` and `LEAD()` to compare each Employment’s Salary to the previous and next.
- Added a column that states whether the current value is `HIGHER`, `LOWER`, or `EQUAL` compared to the previous record.

💡 *Real-Life Application:* Analyzing trends in sales performance day by day.


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



**Real-Life Application:** This is the Analyzing promtions in Employee's Salary day by day.


---

### 2. Rank Within Departments (`RANK()` and `DENSE_RANK()`)


![alt text](<Rank Within Departments Using RANK and DENSE_RANK.png>)


- Ranked Employees within each region using both `RANK()` and `DENSE_RANK()`.
- Showed how ties are handled differently:
  - `RANK()` skips numbers after ties.
  - `DENSE_RANK()` doesn’t skip any.


```sql
SELECT 
    name, department, salary,
    RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rank_method,
    DENSE_RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS dense_rank_method
FROM employees;
```



**Real-Life Application:** This is Creating fair leaderboard systems in Employee's salary in there Departments.



---

### 3. Identifying Top Records


- Retrieved the **top 3 Employees** from each region using window functions.
- Used `RANK()` and a subquery to handle potential duplicate amounts.


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


**Real-Life Application:** Rewarding top performers or identifying peak Emplyees Salary.
---

### 4. Finding the Earliest Records

- Pulled the **first 2 Employees** from each region based on `sale_date`.
- Used `ROW_NUMBER()` to identify earliest records in each partition.

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

**Real-Life Application:**  Auditing early Employees or onboarding activity.
---

### 5. Aggregation with Window Functions


- Calculated:
  - The **maximum Salary per Department** using `PARTITION BY`.
  - The **overall maximum Salary** without partitioning.

```sql
SELECT 
    name, department, salary,
    MAX(salary) OVER (PARTITION BY department) AS max_per_department,
    MAX(salary) OVER () AS overall_max
FROM employees;

```
![alt text](<Maximum Salary Per Department and Overall.png>)


**Real-Life Application:**  Highlighting High salary across all Employees.

---

## 📌 Overview

- `Mugisha` has the highest salary (950,000 RWF) in the IT department.
- The Finance department has a salary range between 750,000 and 900,000 RWF.
- We can clearly track promotions or changes in salary ranking over time.
- Hiring trends show that most HR employees were hired after 2020.

---

## 🌍 Real-Life Applications

- Time-based performance analytics.
- Departments-specific leaderboards.
- Highlight Salaries and KPI benchmarking in Employees Salary dashboards.


---

### 🧠 Concluding

This project demonstrated the power and versatility of SQL Window Functions through a sales dataset in the part of Employees Departments. By applying analytical functions like LAG(), LEAD(), RANK(), DENSE_RANK(), ROW_NUMBER(), and window-based aggregations, we gained deeper insights into Employees Salaries, top performers, and High salaries than others.

 Whether it’s RANK()ing the best sellers or using LAG() to reflect on past records,
 these techniques are not only useful for academic purposes but also play a crucial role in real-world data analysis and reporting. 
 
 The project helped us understand how to compare records, identify patterns, and generate business intelligence—all directly from SQL.

Mission accomplished — with data, clarity, and a dash of fun!

