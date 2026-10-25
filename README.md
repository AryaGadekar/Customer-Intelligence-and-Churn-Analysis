## TL;DR
This project explores customer churn using a small relational dataset containing 21 customer records, 21 subscription records, and 9 support records. I used SQLite and SQL to inspect the database tables, and Python with pandas, NumPy, Matplotlib, and Seaborn to clean and combine the data, calculate metrics, and visualize patterns.

In the sample, 6 of 21 customers have a cancellation date, giving an observed churn share of 28.57%. Churn varies across plans in this dataset: Basic is 60%, Standard 22.22%, and Premium 14.29%. These are exploratory sample results, not reliable estimates of broader customer behavior: each plan group is small, the data has no defined reporting window, and the comparisons do not establish what caused a customer to churn.

The notebook also examines monthly cancellation counts, churn by state and subscription type, customer tenure and monthly charges, support complaint and escalation records, and the supplied churn score. The “Revenue at Risk” figure is the sum of monthly charges for customers who already have a cancellation date; it is not a forecast of future revenue losses. The project is descriptive and does not train or validate a machine-learning model.

# Customer-Intelligence-and-Churn-Analysis
Exploratory customer churn analysis using SQL and Python to clean and combine customer, subscription, and support data, calculate key metrics, and visualize patterns.

## Introduction

Customer retention is an important part of understanding how a subscription business performs. This project brings together customer profiles, subscription details, and support records to explore where cancellations appear in the data and how churn patterns vary across plans, subscription types, and locations.

Using SQLite and SQL, I inspected the relational tables and loaded the data for analysis. In Python, I cleaned and prepared the fields, joined the tables, created churn-related metrics, and built visualizations with Matplotlib and Seaborn. The notebook explores overall churn and retention, customer tenure and monthly charges, cancellation counts over time, and how support records appear alongside churn.

The project uses a 21-customer sample, so its segment comparisons are exploratory. Its main focus is demonstrating a clear customer analytics workflow from relational data preparation through metric calculation and visual communication.

## Table of Contents

- [Introduction](#introduction)
- [Project Background](#project-background)
- [Key Metrics](#key-metrics)
- [Data Overview and Validation](#data-overview-and-validation)
- [Executive Summary](#executive-summary)
- [Insights Deep Dive](#insights-deep-dive)
- [Strategic Recommendations and Performance Measures](#strategic-recommendations-and-performance-measures)
- [Tools and Methods](#tools-and-methods)
- [Assumptions and Limitations](#assumptions-and-limitations)
- [Next Steps](#next-steps)
- [How to Run](#how-to-run)
- [Project Files](#project-files)
- [Data Source and Attribution](#data-source-and-attribution)

## Project Background

Subscription-based businesses rely on recurring customer relationships. When customers cancel, the business loses future billing and needs to understand where cancellations occur, which customer groups are affected, and what experiences may be associated with churn. Customer churn analysis helps organize these questions by connecting customer profiles, subscription details, and support records.

## Project Background

Subscription businesses need to understand which customers cancel and how cancellations vary across plans, customer groups, and support experiences. This project explores those questions using a small relational customer dataset.

The database contains three tables connected by `customerid`: customer details, subscription details, and support records. The notebook uses SQLite to connect to the database, list its tables, and inspect their columns. It then uses Python and pandas to load, clean, summarize, and combine the data for analysis.

The notebook explores the following questions:

- What proportion of customers in the sample have a cancellation date?
- How does observed churn vary by plan type, subscription type, and state?
- What are the average monthly charges and customer tenure in the sample?
- How many cancellations are recorded in each month?
- What support records and escalation information are available for customers?

The analysis creates a churn flag from the cancellation date, summarizes support records, calculates descriptive metrics, and visualizes selected patterns. The dataset contains 21 customers, so the segment comparisons are exploratory observations from this sample.

## Key Metrics

All metrics below are calculated in Python with pandas after the notebook loads the SQLite tables. SQLite is used to connect to the database, list its tables, and inspect their columns. The notebook does not use SQL queries to calculate these metrics.

### Churn Flag

The notebook derives a binary `churn_flag` from `cancellation_date`:

- `1` if `cancellation_date` is present
- `0` if `cancellation_date` is missing

This is the outcome used in the notebook’s churn calculations.

### Overall Churn Rate

The notebook calculates the mean of `churn_flag` and multiplies it by 100:

`Churn rate = (customers with a cancellation date / customers in the analysis table) × 100`

Six of the 21 customers have a cancellation date, so the sample churn rate is **28.57%**.

### Retention Rate

Retention is calculated as the complement of the churn rate:

`Retention rate = 100% - churn rate`

For this sample, the resulting retention rate is **71.43%**. It uses the same customer population and cancellation-date definition as the churn rate.

### Churn Rate by Segment

The notebook groups customers by `plan_type`, `state`, and `subscription_type`. Within each group, it calculates the mean of `churn_flag` and converts it to a percentage:

`Segment churn rate = (customers in the segment with a cancellation date / customers in the segment) × 100`

For plan type, the notebook reports:

| Plan | Observed churn rate |
| --- | ---: |
| Basic | 60.00% |
| Standard | 22.22% |
| Premium | 14.29% |

The state and subscription-type summaries also include customer counts. Their monthly-charge totals are sums of the available `monthly_charges` values within each group.

### Average Monthly Charges

The notebook calculates the mean of `monthly_charges` across the customer records:

`Average monthly charges = sum of monthly charges / number of customer records`

The notebook labels this metric **ARPU**. It represents the average listed monthly charge per customer record in this sample.

### Customer Age and Churn by Age Group

Age is calculated from `dob` using the date the notebook is run. Customers are placed into these age bands: Under 18, 18–30, 31–45, 46–60, and 60+.

For each band, the notebook calculates the average `churn_flag` as a percentage and counts unique customers. Because age is calculated using the run date, the age values and group assignments can change when the notebook is run later.

### Average Customer Tenure

Tenure is calculated in days using the subscription start date:

- If a customer has a cancellation date, tenure is the number of days between `subscription_start_date` and `cancellation_date`.
- If the customer has no cancellation date, tenure is the number of days between `subscription_start_date` and the date the notebook is run.

The notebook then calculates the mean of `tenure_days` across the customer records. The tenure value for customers without a cancellation date changes as time passes.

### “Revenue at Risk”

The notebook sums `monthly_charges` for customers whose `churn_flag` is `1`:

`Revenue at Risk = sum of monthly charges for customers with a cancellation date`

This is the total listed monthly charge associated with customers who have already churned. It is not a forecast of revenue from customers who may churn in the future.

### Escalation Rate

The notebook calculates the percentage of rows in the final analysis table where `escalations` equals `Y`:

`Escalation rate = rows marked Y / rows in the final analysis table × 100`

The support table can contain multiple records per customer. The notebook first calculates each customer's support-record count, then retains the latest support record per customer for the final join. As a result, this escalation percentage reflects the retained latest record for each customer, not the share of all support tickets that were escalated. Customers without a joined support record are included in the final customer table and do not match `Y`.

### Average Complaints per Customer

The notebook counts support records for each customer, then divides the total complaint count by the number of unique customers in the final analysis table:

`Average complaints per customer = total recorded support complaints / unique customers`

Customers with no support records contribute no complaints to the total but remain included in the customer denominator.

### Escalation and Churn Correlation

The notebook converts the escalation field to a binary indicator, assigning `1` to `Y` and `0` to other values, then calculates its correlation with `churn_flag`.

This is a descriptive correlation between the retained escalation indicator and churn status in this sample. It does not show that escalations cause churn.

### Churn Risk Categories

The notebook uses the provided `churn_score` field to assign categories:

| Churn score | Category |
| --- | --- |
| Below 50 | Low |
| 50 to below 70 | Medium |
| 70 or above | High |

It then calculates the mean `churn_flag` within each category and reports it as an observed churn percentage. The notebook uses a score supplied in the data; it does not create, train, or validate the score.

### Monthly Churn Trend

The notebook groups customers with a cancellation date by cancellation month and counts unique `customerid` values. It fills months with no recorded cancellations with zero and plots the result.

This chart reports the **number of recorded cancellations per month**. It does not calculate a monthly churn rate because the number of customers active and eligible to churn in each month is not included in the calculation.

## Data Overview and Validation

The analysis uses three tables from `customer_churn.db`. They are loaded into pandas and joined using `customerid`.

| Table | Purpose | Rows | Grain |
| --- | --- | ---: | --- |
| `db_customer` | Customer profile and demographic fields | 21 | One row per customer |
| `db_subscription` | Subscription details, charges, and cancellation information | 21 | One row per customer subscription |
| `db_support` | Customer support records | 9 | One row per support record |

### Source fields

- **Customer table:** `customerid`, `name`, `country`, `state`, `gender`, `dob`, `interests`, and `pincode`
- **Subscription table:** `customerid`, subscription start and renewal dates, subscription and plan types, contract type, cancellation date and reason, monthly charges, CLTV, and churn score
- **Support table:** `customerid`, complaint date, escalation flag, CSAT score, and additional fields

### Initial inspection

The notebook connects to the SQLite database, retrieves the table names from the database metadata, and inspects each table's column names with `PRAGMA table_info`. It then loads the tables into pandas and uses `head()`, `tail()`, and `info()` to review sample rows, data types, and missing values.

The notebook also compares row counts and distinct `customerid` counts. The customer and subscription tables each contain 21 rows and 21 distinct customer IDs. The support table contains 9 rows across 7 distinct customer IDs, so some customers have more than one support record.

### Cleaning and preparation

The notebook makes the following changes before analysis:

- Renames `name` in the customer table to `customer name`.
- Drops `interests` and `pincode` from the customer table.
- Converts `dob` to a datetime value.
- Standardizes the gender values `Men` and `Women` to `Male` and `Female`.
- Fills missing country values using a mapping from state to country built from rows where country is present.
- Converts subscription start, renewal, and cancellation dates to datetime values.
- Drops `col_1` and `comment` from the support table.
- Converts support complaint dates to datetime values.
- Creates a support-record count for each customer, sorts support records by complaint date, and retains the latest support record per customer for the final join.
- Creates `churn_flag` from whether `cancellation_date` is populated.

### Join and row-count checks

The notebook joins subscription records to customer records on `customerid`, then joins the support records. Before the final customer-level join, it reduces the support table to one latest record per customer. It checks table shapes and distinct customer counts to help identify row changes during merging.

This approach keeps the final analysis table at the subscription/customer level rather than allowing multiple support records to multiply customer rows. The complaint count is calculated before retaining the latest support record, so the final table can include both a total support-record count and fields from the latest retained record.

### Validation scope

These checks are exploratory diagnostics. The notebook reviews table shapes, IDs, data types, sample rows, and missing values, but it does not implement automated assertions for all possible data-quality issues. In particular, it does not formally test every join key for unmatched records, validate all date sequences, or enforce a database schema with constraints.

The country imputation assumes that each state maps to one country. That assumption should be reviewed if the data is reused in another context. The dataset contains 21 customers and 9 support records, so findings from the cleaned and joined data remain limited to this sample.
