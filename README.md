# INSERT-Laughs-INTO-Project
This project demonstrates the power of  SQL Window Functions on a fictional "Sales Dataset". Using queries enriched with `LAG()`, `LEAD()`, `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, and aggregate functions, we explore how modern SQL can simplify complex analytics with humor and clarity

# Collaborators 
# Baraka Johnson Bright ( 25583 )
# 

# 📘 Introduction
-
Welcome to INSERT Laughs INTO Project – a SQL-based exploration of window functions using a fictional yet realistic Sales Dataset. This project was designed to help us dive deep into powerful SQL analytical tools such as LAG(), LEAD(), RANK(), DENSE_RANK(), ROW_NUMBER(), and aggregate functions with PARTITION BY.

We built this project not only to demonstrate technical understanding but also to highlight how these functions apply to real-world business scenarios — all while having a bit of fun with a punny group name 😄.


# 😂 INSERT Laughs INTO Project

## Overview
This project demonstrates the power of **SQL Window Functions** on a fictional **Sales Dataset**. Using queries enriched with `LAG()`, `LEAD()`, `RANK()`, `DENSE_RANK()`, `ROW_NUMBER()`, and aggregate functions, we explore how modern SQL can simplify complex analytics with humor and clarity.

---

## 📊 Dataset Description
We created a sample sales dataset with the following columns:

| Column         | Description                          |
|----------------|--------------------------------------|
| `sales_id`     | Unique identifier for each sale      |
| `employee_name`| Name of the salesperson              |
| `region`       | Sales region (e.g., East, West)      |
| `amount`       | Sale amount in USD                   |
| `sale_date`    | Date of the sale                     |

---

## 🔍 Queries and Explanations

### 1. Compare Values with Previous or Next Records
**File:** `sql/lag_lead_comparison.sql`

- Used `LAG()` and `LEAD()` to compare each sale’s amount to the previous and next.
- Added a column that states whether the current value is `HIGHER`, `LOWER`, or `EQUAL` compared to the previous record.

💡 *Real-Life Application:* Analyzing trends in sales performance day by day.

📸 Screenshot: `screenshots/lag_lead_result.png`

---

### 2. Ranking Data within a Category
**File:** `sql/rank_dense_rank.sql`

- Ranked salespeople within each region using both `RANK()` and `DENSE_RANK()`.
- Showed how ties are handled differently:
  - `RANK()` skips numbers after ties.
  - `DENSE_RANK()` doesn’t skip any.

💡 *Real-Life Application:* Creating fair leaderboard systems in sales competitions.

📸 Screenshot: `screenshots/rank_vs_dense_rank.png`

---

### 3. Identifying Top Records
**File:** `sql/top_3_per_category.sql`

- Retrieved the **top 3 sales** from each region using window functions.
- Used `RANK()` and a subquery to handle potential duplicate amounts.

💡 *Real-Life Application:* Rewarding top performers or identifying peak sale events.

📸 Screenshot: `screenshots/top_3_result.png`

---

### 4. Finding the Earliest Records
**File:** `sql/earliest_records.sql`

- Pulled the **first 2 sales** from each region based on `sale_date`.
- Used `ROW_NUMBER()` to identify earliest records in each partition.

💡 *Real-Life Application:* Auditing early sales or onboarding activity.

📸 Screenshot: `screenshots/earliest_records_result.png`

---

### 5. Aggregation with Window Functions
**File:** `sql/aggregation_with_window.sql`

- Calculated:
  - The **maximum sale amount per region** using `PARTITION BY`.
  - The **overall maximum sale** without partitioning.

💡 *Real-Life Application:* Benchmarking performance across regions vs company-wide goals.

📸 Screenshot: `screenshots/aggregation_result.png`

---

## 👨‍💻 Team
- Member 1
- Member 2
- Member 3

💬 *Yes, we INSERTed some laughs INTO our project — and learned a lot doing it!*

---

## 💡 Real-Life Applications of Window Functions
- Time-based performance analytics.
- Region-specific leaderboards.
- Trend tracking and KPI benchmarking in sales dashboards.

---

## 📂 Project Structure

