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

This project examines a small customer dataset containing three related tables: customer information, subscription information, and customer support records. The tables are linked by `customerid`. The analysis uses the cancellation date to identify customers marked as churned, then compares the available churn results across plan type, subscription type, and state. It also explores customer tenure, monthly charges, cancellation timing, and support information.

The project follows a practical analysis workflow:

1. Inspect the SQLite database and review the tables and columns.
2. Load the customer, subscription, and support data into Python.
3. Clean selected fields, standardize categories, and convert date columns.
4. Create a churn flag from the cancellation date.
5. Summarize support records and join the tables for customer-level analysis.
6. Calculate descriptive metrics and compare churn across available segments.
7. Create visualizations to communicate the observed patterns.

The goal is to demonstrate how SQL and Python can be used together to prepare relational customer data and answer focused business questions. The dataset contains 21 customers, so the segment comparisons are exploratory and are presented as observations from this sample.
