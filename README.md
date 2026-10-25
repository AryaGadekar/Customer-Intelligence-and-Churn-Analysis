**#TL;DR** 
This project explores customer churn using a small relational dataset containing 21 customer records, 21 subscription records, and 9 support records. I used SQLite and SQL to inspect the database tables, and Python with pandas, NumPy, Matplotlib, and Seaborn to clean and combine the data, calculate metrics, and visualize patterns.

In the sample, 6 of 21 customers have a cancellation date, giving an observed churn share of 28.57%. Churn varies across plans in this dataset: Basic is 60%, Standard 22.22%, and Premium 14.29%. These are exploratory sample results, not reliable estimates of broader customer behavior: each plan group is small, the data has no defined reporting window, and the comparisons do not establish what caused a customer to churn.

The notebook also examines cancellation counts by month, churn by state and subscription type, customer tenure and monthly charges, support complaint and escalation records, and the supplied churn score. The “Revenue at Risk” figure is the sum of monthly charges for customers who already have a cancellation date; it is not a forecast of future revenue losses. The project is descriptive and does not train or validate a machine-learning model.

# Customer-Intelligence-and-Churn-Analysis
Exploratory customer churn analysis using SQL and Python to clean and combine customer, subscription, and support data, calculate key metrics, and visualize patterns.



