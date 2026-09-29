# Veda-Technology-Task-22
# Cohort Retention Analysis

Monthly customer retention analysis by first-purchase cohort, built with **Python** and **SQL**.

**Author:** Akshat Srivastava
**Organisation:** Veda Technology (Data Analytics Internship)
**Task:** Task 22, Cohort Retention Basics (Level 2, Day 22)

---

## Overview

This project answers one question: **how long do customers keep buying after their first purchase?**

Customers are grouped into cohorts by the month of their first order. For each cohort, we track the share of customers who place at least one order in each following month. The result is a cohort retention table and heatmap, plus business insights.

## Deliverables

| Deliverable | Where to find it |
|---|---|
| Cohort table | Notebook and PDF report (Section 3) |
| Heatmap | Notebook and PDF report (Section 4) |
| Insights | Notebook and PDF report (Sections 5-8) |
| SQL version of the analysis | Notebook (SQL section) |

## Key Results

| Metric | Value |
|---|---|
| Customers analysed | 5,877 |
| Month-1 retention | 29.0% |
| Month-3 retention | 27.2% |
| Month-6 retention | 24.4% |
| Month-12 retention | 20.2% |

- About **71% of customers buy once and never return**. The biggest drop is in the first month.
- After that, retention flattens near **20%**, a loyal core that keeps buying for two years.
- Repeat purchases are **seasonal**, with clear peaks around September.
- Cohorts acquired in **Aug-Sep retain best** (Month-1 up to 42.5%). Off-season cohorts such as Feb and Apr retain worst (Month-1 as low as 18.2%).

![Cohort retention heatmap](cohort_heatmap.png)

## Repository Structure

```
.
├── README.md
├── online_retail_II.csv            # Dataset (about 23 MB)
├── Cohort_Retention_Task22.ipynb   # Python + SQL analysis notebook
├── Cohort_Retention_Report.pdf     # Full written report
└── cohort_heatmap.png              # Heatmap image used in this README
```

## Dataset

The data follows the **Online Retail II** schema (UCI Machine Learning Repository) and covers **Dec 2009 to Dec 2011**.

| Column | Description |
|---|---|
| `Invoice` | Invoice number (starts with `C` if cancelled) |
| `StockCode` | Product code |
| `Description` | Product name |
| `Quantity` | Units per line item |
| `InvoiceDate` | Date and time of the order |
| `Price` | Unit price |
| `Customer ID` | Unique customer identifier |
| `Country` | Customer country |

> **Note:** The CSV in this repository is a **simulated dataset** built to mirror the structure and data-quality issues of the real Online Retail II data (missing customer IDs, cancelled invoices, duplicates, zero prices). Exact figures would differ on the real dataset. The code works unchanged on the original data.

## Methodology

**1. Data cleaning**
- Removed exact duplicate rows
- Dropped rows with a missing `Customer ID`
- Removed cancelled invoices (`Invoice` starting with `C`)
- Kept only rows with `Quantity > 0` and `Price > 0`
- Excluded non-product codes (`POST`, `DOT`)

Result: 272,626 raw rows reduced to **261,431** clean rows (95.9%), covering 5,877 customers and 38,880 invoices.

**2. Cohort logic**
- `CohortMonth`: month of a customer's first purchase
- `CohortIndex`: months between a purchase and the customer's cohort month (0 = first month)
- `Retention %`: customers active in month N divided by cohort size (month 0)

**3. Visualisation:** a seaborn heatmap of retention by cohort and month, plus an average retention curve.

**4. SQL validation:** the same retention table was rebuilt in SQL (SQLite, using CTEs) and compared with the pandas result. The maximum difference was **0.0**, so both methods agree exactly.

## SQL Approach

The SQL version uses four CTEs:
1. `orders`: distinct customer and invoice month
2. `cohort`: each customer's first month (`MIN`)
3. `activity`: month index between each order and the cohort month
4. `cohort_counts`: active customers per cohort and index, joined to cohort size for retention %

## How to Run
1. Open `Cohort_Retention_Task22.ipynb` in [Google Colab](https://colab.research.google.com).
2. Upload `online_retail_II.csv` to the Colab session.
3. Run all cells from top to bottom.

**Requirements:** `pandas`, `numpy`, `matplotlib`, `seaborn` (all preinstalled in Colab). SQL uses Python's built-in `sqlite3`.

## Recommendations
- **Improve onboarding:** send a second-purchase incentive within 30 days of the first order, where the largest loss occurs.
- **Time acquisition campaigns:** increase marketing spend in Aug-Sep, when new customers retain best.
- **Support off-season cohorts:** run win-back campaigns for customers acquired in slow months.
- **Reward the loyal core:** the roughly 20% who stay long term likely drive a large share of revenue.

## Limitations
- Recent cohorts (2011-10 onwards) have incomplete data and should not be compared directly with older ones.
- Retention is defined as "any purchase in the month". Revenue-based retention could tell a different story.
- The dataset is simulated, so exact figures would differ on real data.

## Tools
Python, pandas, NumPy, Matplotlib, Seaborn, SQL (SQLite), Google Colab
