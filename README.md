# Monthly Sales Growth Analysis Dashboard

## Project Overview

The **Monthly Sales Growth Analysis Dashboard** is a data analytics project developed to analyze sales performance, track monthly revenue trends, understand customer and product performance, and generate meaningful business insights.

The main objective of this project is to transform raw sales data into meaningful information using Python, SQL, and Power BI.

Python is used for data cleaning, preprocessing, and exploratory data analysis (EDA). MySQL can be used to store and manage the processed data, while Power BI is used to create interactive dashboards and visual reports.

This project helps businesses monitor sales performance, identify profitable products and regions, and make better data-driven decisions.

## Project Objectives

- Analyze monthly sales performance and growth trends.
- Clean and preprocess raw sales data.
- Identify duplicate records and missing or invalid values.
- Calculate total sales, total cost, and profit.
- Analyze sales performance by customer, product, and region.
- Create meaningful KPIs for business performance monitoring.
- Build an interactive Power BI dashboard.
- Prepare structured datasets for database integration and reporting.

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Data processing and analysis |
| Pandas | Data cleaning and manipulation |
| NumPy | Numerical operations |
| Matplotlib | Data visualization |
| Jupyter Notebook | Python development and analysis |
| Excel | Dataset storage and review |
| MySQL | Relational database storage and querying (planned integration) |
| Power BI | Interactive dashboards and reporting |
| Git & GitHub | Version control and project hosting |

## Dataset Description

The project uses a sales dataset containing transaction-level information.

The original dataset includes the following columns:

| Column Name | Description |
|---|---|
| Order_ID | Unique order identifier |
| Order_Date | Date when the order was placed |
| Customer | Customer name |
| Region | Sales region |
| Product | Product name |
| Sales | Sales amount |
| Cost | Cost of the product or order |

The dataset is used to analyze sales, cost, profit, customer performance, product performance, and monthly trends.

## Project Workflow

The project follows this data analytics workflow:

1. Collect the raw sales dataset.
2. Load the dataset into Python using Pandas.
3. Understand the dataset using basic data inspection.
4. Clean missing values, invalid dates, and duplicate records.
5. Perform exploratory data analysis.
6. Calculate profit and other business metrics.
7. Prepare fact and dimension tables.
8. Export processed datasets for reporting and database use.
9. Build an interactive Power BI dashboard.
10. Analyze the results and identify business insights.

## 1. Data Loading Using Python

The raw Excel dataset is loaded into Python using Pandas.

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_excel("sales_raw_500.xlsx")

print(df.head())
print(df.shape)
print(df.info())
print(df.describe())
```

These commands help understand the dataset structure, data types, records, and numerical values.

## 2. Data Cleaning and Preprocessing

Data cleaning is an important step because raw business data may contain duplicate records, missing values, incorrect formats, and invalid dates.

The following operations are used:

- Inspect missing values.
- Identify and remove duplicate records.
- Convert Order_Date into a proper date format.
- Validate sales and cost values.
- Check data types.
- Prepare a clean dataset for further analysis.

Example:

```python
# Check missing values
print(df.isnull().sum())

# Check duplicate records
print(df.duplicated().sum())

# Remove duplicate records
df = df.drop_duplicates()

# Convert date column
df["Order_Date"] = pd.to_datetime(
    df["Order_Date"],
    errors="coerce"
)

# Remove rows with invalid dates
df = df.dropna(subset=["Order_Date"])

# Calculate profit
df["Profit"] = df["Sales"] - df["Cost"]
```

The cleaning process helps improve data quality and the reliability of the analysis.

## 3. Exploratory Data Analysis (EDA)

Exploratory Data Analysis is performed to understand sales patterns and discover useful business insights.

The analysis focuses on:

- Overall sales performance.
- Monthly sales trends.
- Product-wise sales performance.
- Region-wise sales performance.
- Customer-wise sales performance.
- Profitability analysis.
- Identification of high-performing and low-performing areas.

Example:

```python
# Total sales
total_sales = df["Sales"].sum()

# Total cost
total_cost = df["Cost"].sum()

# Total profit
total_profit = df["Profit"].sum()

print("Total Sales:", total_sales)
print("Total Cost:", total_cost)
print("Total Profit:", total_profit)
```

## 4. Data Modeling

After cleaning and analysis, the data is organized into separate tables to support structured reporting.

The project prepares the following tables:

| Table | Description |
|---|---|
| Fact_Sales | Main sales transaction records |
| Dim_Date | Date-related information |
| Dim_Customer | Customer details |
| Dim_Product | Product details |
| Dim_Region | Region details |
| Monthly_Summary | Monthly sales summary |
| KPI_Summary | Key business performance metrics |
| Data_Quality | Data quality information |

A star-schema-style structure helps organize the sales data and makes reporting easier.

## 5. MySQL Database Integration

MySQL is intended to be used as the relational database layer between Python and Power BI.

The planned data flow is:

**Excel → Python (Pandas) → MySQL → Power BI**

The processed datasets can be loaded into MySQL using SQLAlchemy and PyMySQL.

Example database creation:

```sql
CREATE DATABASE sales_analytics;
```

Example Python connection:

```python
from sqlalchemy import create_engine, URL

connection_url = URL.create(
    drivername="mysql+pymysql",
    username="my_username",
    password="my_password",
    host="localhost",
    port=3306,
    database="sales_analytics"
)

engine = create_engine(connection_url)
```

Example of loading a DataFrame:

```python
df.to_sql(
    name="fact_sales",
    con=engine,
    if_exists="replace",
    index=False
)
```

Database credentials should be stored securely in environment variables and should never be committed to GitHub.

**Note:** MySQL integration is a planned extension of the current project. The existing Excel datasets and Power BI files can be used independently until the database connection is configured and tested.

## 6. Power BI Dashboard

Power BI is used to develop an interactive dashboard that presents sales performance in a clear and understandable format.

The dashboard can include:

- Total Sales KPI.
- Total Cost KPI.
- Total Profit KPI.
- Profit Margin KPI.
- Total Orders KPI.
- Average Order Sales KPI.
- Monthly Sales Trend.
- Region-wise Sales Analysis.
- Product-wise Sales Analysis.
- Customer Performance Analysis.

### Dashboard Features

- Interactive charts and visualizations.
- KPI cards for business metrics.
- Date-based filtering.
- Product and region analysis.
- Monthly performance monitoring.
- Easy-to-understand business reports.

### Sample DAX Measures

**Total Sales**

```dax
Total Sales = SUM(fact_sales[Sales])
```

**Total Cost**

```dax
Total Cost = SUM(fact_sales[Cost])
```

**Total Profit**

```dax
Total Profit = SUM(fact_sales[Profit])
```

**Profit Margin**

```dax
Profit Margin =
DIVIDE([Total Profit], [Total Sales])
```

**Total Orders**

```dax
Total Orders =
DISTINCTCOUNT(fact_sales[Order_ID])
```

**Average Order Sales**

```dax
Average Order Sales =
DIVIDE([Total Sales], [Total Orders])
```

## 7. Business Benefits

This project provides useful information to support business decisions.

- Helps management understand monthly sales performance.
- Identifies products generating higher revenue.
- Highlights profitable and low-performing regions.
- Helps track business growth over time.
- Provides a clear view of revenue and profitability.
- Reduces manual reporting through data preparation and dashboard automation.

## Project Folder Structure

```text
Monthly-Sales-Growth-PowerBI/
│
├── data/
│   ├── sales_raw_500.xlsx
│   └── Sales_PowerBI_Datasets.xlsx
│
├── notebooks/
│   ├── Cleaned_Original.ipynb
│   └── Sales_PowerBI_Continue.ipynb
│
├── powerbi/
│   └── Monthly_Sales_Growth_Dashboard.pbix
│
├── README.md
│
└── .gitignore
```

## How to Run the Project

### Step 1: Clone the Repository

```bash
git clone https://github.com/my_USERNAME/Monthly-Sales-Growth-PowerBI.git
```

### Step 2: Open the Project

```bash
cd Monthly-Sales-Growth-PowerBI
```

### Step 3: Install Required Python Libraries

```bash
pip install pandas numpy matplotlib openpyxl jupyter
```

### Step 4: Start Jupyter Notebook

```bash
jupyter notebook
```

Open the notebooks folder and execute the notebook cells in order.

### Step 5: Open the Power BI Dashboard

1. Open Power BI Desktop.
2. Open the `Monthly_Sales_Growth_Dashboard.pbix` file.
3. Review the dashboard visuals and reports.
4. Refresh the data if the source files have changed.

## Future Enhancements

- Complete Python-to-MySQL database integration.
- Connect Power BI directly to MySQL.
- Automate data refresh and reporting.
- Add year-over-year and month-over-month growth analysis.
- Develop advanced sales forecasting.
- Add more business KPIs and analytical reports.

## Conclusion

The Monthly Sales Growth Analysis Dashboard demonstrates an end-to-end data analytics workflow, starting from raw data collection and cleaning to exploratory analysis, data modeling, and interactive business intelligence reporting.

By using Python, SQL, and Power BI, this project demonstrates practical skills in data preparation, database concepts, data analysis, visualization, and business reporting.

It provides a foundation for transforming raw sales data into meaningful insights that support better business decisions.

---

**Project Category:** Data Analytics / Business Intelligence

**Tools:** Python | Pandas | NumPy | SQL | MySQL (planned) | Power BI | Excel
