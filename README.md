# INSERT-Laughs-INTO-Project
This project demonstrates the power of  SQL Window Functions on a fictional "Sales Dataset". Using queries enriched with `LAG()`, `LEAD()`, `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, and aggregate functions, we explore how modern SQL can simplify complex analytics with humor and clarity

# Collaborators 
# Baraka Johnson Bright ( 25583 )
# Ndahindurwa Gael (25512)  

# 📘 Introduction
-
Welcome to INSERT Laughs INTO Project – a SQL-based exploration of window functions using a fictional yet realistic Sales Dataset. This project was designed to help us dive deep into powerful SQL analytical tools such as LAG(), LEAD(), RANK(), DENSE_RANK(), ROW_NUMBER(), and aggregate functions with PARTITION BY.

We built this project not only to demonstrate technical understanding but also to highlight how these functions apply to real-world business scenarios.




---

## 📥 let's Get on Hand

At the initial stage of the assignment, it was imperative to establish a well-defined and logically structured dataset to serve as the basis for all ensuing operations and analyses. Consequently, we developed a simple yet representative dataset focused on employee information, as this subject matter is commonly used in database design due to its relevance and applicability in real-world organizational settings. The dataset was designed to include essential employee attributes such as identification numbers, names, job roles, departments, and salaries, thereby ensuring that it provides a sufficient level of complexity for demonstrating various database functionalities while remaining manageable for analysis. The creation of the corresponding table, which encapsulates the structure of this dataset, adhered to standard database normalization principles and was implemented using SQL data definition commands. The schema of the table, as illustrated below, reflects thoughtful consideration of data types, constraints, and relational integrity to ensure both accuracy and consistency throughout the assignment.


The table  is created as follows:

```sql
CREATE TABLE employees (
    employee_id INT,
    name VARCHAR(70),
    department VARCHAR(70),
    salary INT,
    hire_date DATE
);
```

![creation of Employee table-1](https://github.com/user-attachments/assets/de0f43e5-eebd-46f9-9de8-20f16e02d481)



Following the successful creation and structural definition of the employee-related table, we proceeded to populate the table with a series of data entries. This step involved the systematic insertion of sample records that accurately reflect realistic employee information, thereby enabling us to simulate real-world scenarios for testing and analysis purposes. The data insertion process was carried out using structured SQL INSERT statements, ensuring that each record adhered to the previously defined schema in terms of data types, constraints, and referential integrity. The intention behind this step was to establish a functional and meaningful dataset upon which various database operations and queries could be effectively demonstrated.


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


![Insertion of sample datas](https://github.com/user-attachments/assets/243ac5ef-cc98-411a-9fdc-a8a945da6567)

---

## 🔍 SQL Queries with their Descriptions 

### 1. Comparison between salaries using `LAG()` and `LEAD()`


![Comparison  of Salaries Using LAG() and LEAD()](https://github.com/user-attachments/assets/dcea5c81-764a-446a-ac4e-c1c7a949e282)

The analytical SQL window functions LAG() and LEAD() were utilized to perform a comparative assessment of each employee’s salary in relation to the preceding and succeeding records within a defined order—typically based on employee identification numbers or chronological hiring dates. The purpose of employing these functions was to enable row-wise comparisons that reveal patterns or shifts in salary distribution across consecutive employees.

Additionally, an auxiliary column was introduced to the result set. This column dynamically evaluates and categorizes the current employee’s salary by comparing it to the previous record retrieved using the LAG() function. Based on this comparison, a textual label is assigned to each row, indicating whether the current salary value is HIGHER, LOWER, or EQUAL relative to its immediate predecessor. This enhances interpretability and provides a qualitative understanding of salary progression.




- Used `LAG()` and `LEAD()` to compare each Employment’s Salary to the previous and next.
- Added a column that states whether the current value is `HIGHER`, `LOWER`, or `EQUAL` compared to the previous record.

💡 Practical Application in Real-World Contexts:
A comparable use case in industry would involve tracking and analyzing daily sales performance metrics over time. By comparing each day’s revenue to that of the previous and following days, business analysts can detect upward or downward trends, identify anomalies, and make data-driven decisions to optimize performance and resource allocation.


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


![Rank Within Departments Using RANK and DENSE_RANK](https://github.com/user-attachments/assets/f56e9714-e7c7-4c64-b0e2-1792da01bc8f)




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


![Top 3 Paid Employees Per Department](https://github.com/user-attachments/assets/102c68a4-fc62-40f2-a7e2-016056cb4506)




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

![First 2 Employees Per Department (based on hire_date)](https://github.com/user-attachments/assets/1ae48b58-959a-426a-89e8-b496b364074b)


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

![Maximum Salary Per Department and Overall](https://github.com/user-attachments/assets/92e0ad64-7850-4aff-bc08-85f38dc9f486)


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

