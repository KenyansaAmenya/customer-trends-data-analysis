# Customer Shopping Behavior Data Analysis

A retail analytics project that explores customer shopping behavior to help a retail company understand revenue patterns, customer segments, discount usage, subscription behavior, product performance, and purchase drivers. The repository combines Python-based data preparation, PostgreSQL SQL analysis, Power BI dashboarding, and presentation/report deliverables.

## Table of Contents

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Repository Contents](#repository-contents)
- [Dataset Overview](#dataset-overview)
- [Business Questions Answered](#business-questions-answered)
- [Project Workflow](#project-workflow)
- [Data Preparation](#data-preparation)
- [SQL Analysis](#sql-analysis)
- [Power BI Dashboard](#power-bi-dashboard)
- [Key Metrics](#key-metrics)
- [How to Run the Project](#how-to-run-the-project)
- [Database Setup](#database-setup)
- [Recommended Repository Structure](#recommended-repository-structure)
- [Insights and Recommendations](#insights-and-recommendations)
- [Tools and Technologies](#tools-and-technologies)
- [Known Notes and Assumptions](#known-notes-and-assumptions)
- [Author](#author)

## Project Overview

This project analyzes customer shopping behavior for a retail company using a dataset of customer purchases. The goal is to turn raw customer transaction data into business insights that can support better marketing, customer engagement, product strategy, discount planning, and loyalty decisions.

The analysis follows an end-to-end data analytics workflow:

1. **Python / Jupyter Notebook** for data inspection, cleaning, transformation, feature engineering, and database loading.
2. **PostgreSQL SQL** for answering business questions and generating analytical summaries.
3. **Power BI** for interactive dashboarding and stakeholder-friendly visual exploration.
4. **Report and presentation files** for communicating insights and recommendations.

## Business Problem

A leading retail company wants to better understand changes in customer shopping behavior across demographics, products, categories, subscription status, discounts, shipping methods, and repeat-purchase patterns.

The central business question is:

> How can the company leverage consumer shopping data to identify trends, improve customer engagement, and optimize marketing and product strategies?

The analysis is designed to help the business:

- Identify high-value customer groups.
- Understand whether subscription customers spend more.
- Evaluate the impact of discounts and promo-code usage.
- Discover top-performing products and categories.
- Segment customers by loyalty and prior purchase behavior.
- Compare shipping methods and their relationship with purchase value.
- Support marketing and merchandising decisions with data-driven evidence.

## Repository Contents

| File | Description |
| --- | --- |
| `customer_shopping_behavior.csv` | Raw customer shopping behavior dataset used for analysis. |
| `customer_analysis.ipynb` | Jupyter Notebook for data preparation, cleaning, transformation, feature engineering, and PostgreSQL loading. |
| `customer_behaviour.sql` | PostgreSQL queries used to answer the business questions. |
| `customer_behavior_dashboard.pbix` | Power BI dashboard file for interactive data visualization. |
| `Customer Shopping Behavior Analysis.pdf` | Final report/exported analysis document. |
| `Customer-Shopping-Behavior-Analysis.pptx` | Presentation deck for communicating insights to stakeholders. |
| `problem-statement.txt` | Original project problem statement and deliverables. |
| `README.md` | Project documentation. |

## Dataset Overview

The dataset contains **3,900 customer purchase records** and **18 original columns**.

### Original Columns

| Column | Description |
| --- | --- |
| `Customer ID` | Unique identifier for each customer record. |
| `Age` | Customer age. |
| `Gender` | Customer gender. |
| `Item Purchased` | Product bought by the customer. |
| `Category` | Product category. |
| `Purchase Amount (USD)` | Purchase value in US dollars. |
| `Location` | Customer location/state. |
| `Size` | Product size. |
| `Color` | Product color. |
| `Season` | Season associated with the purchase. |
| `Review Rating` | Customer review rating for the purchase/product. |
| `Subscription Status` | Whether the customer is subscribed. |
| `Shipping Type` | Shipping method used. |
| `Discount Applied` | Whether a discount was applied. |
| `Promo Code Used` | Whether a promo code was used. |
| `Previous Purchases` | Number of prior purchases made by the customer. |
| `Payment Method` | Payment method used. |
| `Frequency of Purchases` | Customer purchase frequency label. |

### Dataset Snapshot

- **Total records:** 3,900
- **Total revenue:** `$233,081`
- **Average purchase amount:** `$59.76`
- **Age range:** 18 to 70
- **Categories:** Clothing, Accessories, Footwear, Outerwear
- **Seasons:** Spring, Summer, Fall, Winter
- **Shipping types:** Free Shipping, Standard, Store Pickup, Next Day Air, Express, 2-Day Shipping

## Business Questions Answered

The SQL analysis addresses the following questions:

1. What is the total revenue generated by male vs. female customers?
2. Which customers used a discount but still spent more than the average purchase amount?
3. Which are the top 5 products with the highest average review rating?
4. How do average purchase amounts compare between Standard and Express Shipping?
5. Do subscribed customers spend more? Compare average spend and total revenue between subscribers and non-subscribers.
6. Which 5 products have the highest percentage of purchases with discounts applied?
7. How can customers be segmented into New, Returning, and Loyal groups based on previous purchases?
8. What are the top 3 most purchased products within each category?
9. Are repeat buyers, defined as customers with more than 5 previous purchases, also likely to subscribe?
10. What is the revenue contribution of each age group?

## Project Workflow

```text
Raw CSV Dataset
      |
      v
Python / Jupyter Notebook
- Inspect structure
- Check data types
- Handle missing values
- Rename columns
- Create age groups
- Create purchase frequency feature
- Drop redundant promo-code field
      |
      v
PostgreSQL Database
- Load cleaned dataframe into customer table
- Run SQL business analysis queries
      |
      v
Power BI
- Build interactive dashboard
- Visualize trends and KPIs
      |
      v
Report / Presentation
- Summarize insights
- Provide business recommendations
```

## Data Preparation

The notebook `customer_analysis.ipynb` performs the following preparation steps:

### 1. Import and Load Data

The dataset is loaded from `customer_shopping_behavior.csv` using pandas.

### 2. Inspect Data

The notebook checks:

- First rows of the dataset.
- Column data types.
- Summary statistics for numeric columns.
- Missing values by column.

### 3. Handle Missing Review Ratings

Missing values in `Review Rating` are filled using the median review rating within each product category. Median is used because it is more robust to outliers than the mean.

### 4. Standardize Column Names

Column names are transformed to lowercase and snake case. The column `purchase_amount_(usd)` is renamed to `purchase_amount` for easier SQL and Python referencing.

Example transformed names include:

- `customer_id`
- `item_purchased`
- `purchase_amount`
- `subscription_status`
- `shipping_type`
- `discount_applied`
- `previous_purchases`
- `frequency_of_purchases`

### 5. Create Age Groups

An `age_group` column is created using quartile-based age bins:

- Young Adult
- Adult
- Middle-aged
- Senior

This enables demographic revenue analysis and customer segmentation.

### 6. Create Purchase Frequency Days

A numeric `purchase_frequency_days` column is created from `frequency_of_purchases` values.

| Frequency Label | Mapped Days |
| --- | ---: |
| Weekly | 7 |
| Fortnightly | 14 |
| Bi-Weekly | 14 |
| Monthly | 30 |
| Quarterly / Every 3 Months | 90 |
| Annually | 365 |

### 7. Remove Redundant Promo Code Field

The notebook checks whether `discount_applied` and `promo_code_used` contain matching values. Since they match, `promo_code_used` is dropped as a redundant field.

### 8. Load Cleaned Data to PostgreSQL

The cleaned dataframe is loaded into a PostgreSQL table named `customer` in a database named `customer_behavior`.

## SQL Analysis

The file `customer_behaviour.sql` contains PostgreSQL queries for each analysis question.

### Query Themes

- Revenue by gender.
- Discounted purchases above average purchase value.
- Top-rated products.
- Average spend by shipping type.
- Subscription vs. non-subscription performance.
- Discount rate by product.
- Customer loyalty segmentation.
- Top products within each category.
- Repeat-buyer subscription behavior.
- Revenue contribution by age group.

### Example SQL Query

```sql
select gender, sum(purchase_amount) as total_revenue
from customer
group by gender;
```

This query calculates revenue generated by each gender segment.

## Power BI Dashboard

The Power BI file `customer_behavior_dashboard.pbix` is included for interactive stakeholder reporting.

Suggested dashboard pages or visuals include:

- Revenue overview and KPI cards.
- Revenue by gender, age group, category, and location.
- Top products by revenue and rating.
- Subscription status performance.
- Discount usage and discount rate by product.
- Shipping method comparison.
- Customer loyalty segmentation.
- Seasonal purchase patterns.

## Key Metrics

Based on the raw dataset:

| Metric | Value |
| --- | ---: |
| Total records | 3,900 |
| Total revenue | `$233,081` |
| Average purchase amount | `$59.76` |
| Male customer revenue | `$157,890` |
| Female customer revenue | `$75,191` |
| Subscribed customer records | 1,053 |
| Non-subscribed customer records | 2,847 |
| Discount-applied records | 1,677 |
| Non-discount records | 2,223 |

### Revenue by Category

| Category | Revenue |
| --- | ---: |
| Clothing | `$104,264` |
| Accessories | `$74,200` |
| Footwear | `$36,093` |
| Outerwear | `$18,524` |

### Top Revenue-Generating Products

| Product | Revenue |
| --- | ---: |
| Blouse | `$10,410` |
| Shirt | `$10,332` |
| Dress | `$10,320` |
| Pants | `$10,090` |
| Jewelry | `$10,010` |

## How to Run the Project

### Prerequisites

Install the following tools:

- Python 3.9 or higher
- Jupyter Notebook or JupyterLab
- PostgreSQL
- Power BI Desktop

### Python Dependencies

Install the required Python libraries:

```bash
pip install pandas sqlalchemy psycopg2-binary notebook
```

### Run the Notebook

From the repository root, start Jupyter:

```bash
jupyter notebook
```

Then open:

```text
customer_analysis.ipynb
```

Run the cells in order to:

1. Load the CSV file.
2. Inspect and clean the data.
3. Create derived fields.
4. Load the cleaned table into PostgreSQL.

## Database Setup

The notebook expects a local PostgreSQL database connection similar to:

```python
username = "postgres"
password = "12345"
host = "localhost"
port = "5432"
database = "customer_behavior"
```

Before running the database load step, create the database in PostgreSQL:

```sql
CREATE DATABASE customer_behavior;
```

After the notebook loads the dataframe, the PostgreSQL table should be named:

```text
customer
```

Then run the queries in:

```text
customer_behaviour.sql
```

> Note: Update the username, password, host, port, and database values in the notebook to match your own PostgreSQL environment.

## Recommended Repository Structure

The current repository keeps the main project files in the root folder. For a larger production-style project, the following structure could be used:

```text
customer-trends-data-analysis/
├── data/
│   └── customer_shopping_behavior.csv
├── notebooks/
│   └── customer_analysis.ipynb
├── sql/
│   └── customer_behaviour.sql
├── dashboards/
│   └── customer_behavior_dashboard.pbix
├── reports/
│   ├── Customer Shopping Behavior Analysis.pdf
│   └── Customer-Shopping-Behavior-Analysis.pptx
├── problem-statement.txt
└── README.md
```

## Insights and Recommendations

### 1. Clothing is the strongest revenue category

Clothing generates the highest total revenue in the dataset. The business should prioritize inventory planning, campaign budgets, and merchandising around high-performing clothing products while continuing to cross-sell accessories and footwear.

### 2. Male customers represent a larger revenue share

Male customer records generate more total revenue than female customer records in the dataset. Marketing teams should investigate whether this is due to customer mix, acquisition strategy, product assortment, or campaign targeting.

### 3. Subscription analysis can guide loyalty strategy

The SQL analysis compares subscribed and non-subscribed customers by average spend and total revenue. If subscribers show stronger spending behavior, the company should promote subscription benefits more aggressively. If non-subscribers generate higher total revenue due to volume, the company should build conversion campaigns for this larger base.

### 4. Discount behavior should be optimized, not simply expanded

The analysis identifies customers who used discounts but still spent above the average purchase amount. These customers may be good targets for personalized offers, bundles, and loyalty incentives because discounts appear to support higher-value purchases for them.

### 5. Repeat buyers are important loyalty candidates

Customers with more than 5 previous purchases are analyzed against subscription status. Repeat buyers who are not subscribed are strong candidates for subscription conversion campaigns.

### 6. Shipping preferences can influence customer experience

Comparing Standard and Express Shipping average purchase amounts can help the company understand whether faster delivery options are associated with higher-value orders.

### 7. Product-level discount rates can improve margin control

Products with the highest percentage of discounted purchases should be reviewed to ensure discounts are driving incremental demand rather than unnecessarily reducing margins.

## Tools and Technologies

- **Python**: Data cleaning and transformation.
- **pandas**: Data loading, inspection, feature engineering, and dataframe manipulation.
- **Jupyter Notebook**: Exploratory and reproducible data preparation workflow.
- **PostgreSQL**: Structured querying and business analysis.
- **SQLAlchemy**: Python-to-PostgreSQL database connection.
- **psycopg2**: PostgreSQL adapter for Python.
- **Power BI**: Dashboard creation and visual analytics.
- **PowerPoint + Gamma.app / PDF**: Stakeholder presentation and reporting.

## Known Notes and Assumptions

- SQL queries are written for PostgreSQL.
- The notebook contains placeholder PostgreSQL credentials and should be updated before use.
- The dataset appears to use one row per customer purchase record.
- `discount_applied` and `promo_code_used` are treated as redundant after validation in the notebook.
- Customer loyalty segments are based on `previous_purchases`:
  - `New`: 1 previous purchase
  - `Returning`: 2 to 10 previous purchases
  - `Loyal`: more than 10 previous purchases
- Age groups are generated using quartiles, so exact age boundaries depend on the dataset distribution.

## Author
Felix Amenya Kenyansa

Created as a customer shopping behavior analytics project for retail trend analysis, dashboarding, and business decision support.
