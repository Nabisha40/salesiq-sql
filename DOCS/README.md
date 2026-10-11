# SalesIQ: Sales Intelligence and SQL Analytics

## Project overview

SalesIQ is a sales analytics portfolio project built around a MySQL relational database and an Excel dashboard. It models sales transactions, products, customers, regions, salespeople, and returns, then uses SQL queries to turn operational records into business metrics.

**Dashboard workbook:** [`SalesIQ_Excel_Dashboard.xlsx`](SalesIQ_Excel_Dashboard.xlsx)  
**Database:** MySQL 8  
**Dashboard:** Microsoft Excel  
**Data:** Sample records supplied with this project. The workbook is a static snapshot, not a live connection to MySQL.

<img width="1624" height="979" alt="dashboard_preview" src="https://github.com/user-attachments/assets/789adaca-24be-4e67-8adb-b9ba1eb17246" />

## Business problem

Sales information is only useful when decision-makers can quickly understand where revenue comes from, which products contribute most, how customer groups behave, whether monthly performance is consistent, and how salespeople are tracking against targets. When these answers require manually checking multiple tables, it becomes harder to identify priorities and explain performance clearly.

SalesIQ addresses this by organizing the sales data in a structured database, querying it with SQL, and presenting the resulting metrics in a consolidated Excel dashboard.

## Objectives

1. Design a normalized relational schema for regions, products, customers, salespeople, sales transactions, and returns.
2. Demonstrate practical SQL skills, including filtering, aggregation, joins, subqueries, CTEs, window functions, views, stored procedures, and data validation.
3. Calculate core business KPIs such as completed revenue, order count, units sold, average order value, and gross profit.
4. Compare performance by zone, product, category, customer segment, payment method, month, and salesperson.
5. Present the results through charts that can be explained to a business stakeholder.

## Dataset and database design

The project includes the following tables:

| Table | Approx. sample rows | Purpose |
|---|---:|---|
| `dim_region` | 8 | Region and zone details |
| `dim_product` | 12 | Product catalogue and cost prices |
| `dim_customer` | 15 | Customer attributes and segment |
| `dim_salesperson` | 10 | Sales team, targets, and manager hierarchy |
| `fact_sales` | 36 | Sales transaction records |
| `fact_returns` | 2 | Return and refund records |
| `raw_sales_staging` | 8 | Staging data used for cleaning and validation examples |

The schema follows a dimension-and-fact design: descriptive entities are kept in dimension tables, while transaction-level measures are stored in `fact_sales`. Foreign keys connect sales to relevant product, customer, region, and salesperson records. This reduces duplicated descriptive data and makes multi-table analysis possible.

## Methodology: how the problem was solved

1. **Inspect the business questions.** Identify the measures and comparisons a sales manager would need: revenue, order count, product contribution, customer mix, trends, and target attainment.
2. **Model the data.** Use dimension tables for descriptive attributes and fact tables for sales and returns. Define primary and foreign keys to preserve relationships.
3. **Load sample records.** Use the supplied SQL insert script to populate the schema with demonstration data.
4. **Validate and prepare.** Review statuses and transaction values; use staging data and SQL cleaning/validation examples to demonstrate how inconsistent or incomplete records can be detected. For the dashboard's headline revenue KPIs, only records with `Completed` status are included.
5. **Query the database.** Apply `SUM`, `COUNT`, `AVG`, `GROUP BY`, `CASE`, joins, subqueries, CTEs, and window functions to produce summarized datasets.
6. **Calculate business measures.** Revenue is aggregated from completed sales; average order value is completed revenue divided by completed order count. Gross profit in this workbook is estimated as transaction amount minus quantity multiplied by product cost price.
7. **Visualize the results.** Summarized results are arranged in Excel sheets and shown as charts for fast comparison.
8. **Interpret and communicate.** Convert the chart patterns into findings and potential follow-up actions, while recognizing that the dataset is a small sample rather than a full production history.

## Dashboard KPIs

| KPI | Result | Definition |
|---|---:|---|
| Total revenue | ₹25,64,470 | Sum of completed transaction amounts |
| Completed orders | 34 | Count of completed sales records in the sample |
| Units sold | 89 | Sum of quantity on completed sales records |
| Average order value | ₹75,426 | Completed revenue divided by completed orders |
| Estimated gross profit | ₹10,61,470 | Completed transaction amount less quantity × product cost |

**Important interpretation note:** These are results from the included sample inserts and the workbook's current aggregation rules. Gross profit is a simplified estimate and does not include overhead, discounts not represented in the transaction amount, taxes, or other operating costs. The workbook is not currently refreshing directly from MySQL.

## Charts and what they show

### 1. Revenue by zone
<img width="1484" height="792" alt="revenue_by_zone" src="https://github.com/user-attachments/assets/88edd432-043f-4777-b885-5e9dd1c27c43" />


Compares completed sales revenue across the zones represented in the sample. The South zone leads with ₹15,42,420, followed by West at ₹6,22,150 and North at ₹3,99,900. This points to a strong concentration of sample revenue in the South zone; it does not by itself explain why the difference exists.

### 2. Product revenue
<img width="1485" height="791" alt="top_products_revenue" src="https://github.com/user-attachments/assets/23bf3d88-49a1-471d-bb0a-ec2988c9eb35" />


Ranks products by revenue. Laptop Pro X1 contributes ₹15,07,050, making it the largest revenue-generating product in this sample by a wide margin. The next product, Monitor 27 inch 4K, contributes ₹2,76,920. Management could investigate whether this concentration is expected, sustainable, and dependent on a small number of transactions or customers.

### 3. Monthly revenue trend
<img width="1484" height="702" alt="monthly_revenue_trend" src="https://github.com/user-attachments/assets/4abd1cdb-ee02-4692-91ec-8a7038c607fe" />


Shows monthly revenue across 2024. March records ₹3,40,400, August ₹3,36,200, and December ₹3,75,600; December is the highest month in the sample. September is the lowest at ₹44,000. The uneven pattern suggests that monthly performance should be monitored and investigated for timing effects, large orders, or seasonality. The sample is too small to establish a reliable seasonal pattern.

### 4. Revenue by customer segment
<img width="701" height="733" alt="customer_segments" src="https://github.com/user-attachments/assets/7a166e81-419f-45ee-a212-790f977ea81c" />


Enterprise customers contribute ₹22,83,570 of the ₹25,64,470 completed revenue, approximately 89.0%. SMB contributes ₹1,85,400, Retail ₹66,000, and Government ₹29,500. Revenue is therefore highly concentrated in the Enterprise segment. This may reflect the business model, but customer concentration and retention risk are worth monitoring.

### 5. Gross profit by product
<img width="1485" height="791" alt="top_products_revenue" src="https://github.com/user-attachments/assets/bb9778a3-9675-4923-8a2c-ad815b816d21" />


Compares estimated gross profit by product. Laptop Pro X1 has the highest estimated gross profit at ₹3,67,050. CRM Software License contributes ₹1,74,000 and Annual Support Plan contributes ₹1,59,000. Software and support offerings appear to contribute meaningful profit in addition to hardware. Profit here is calculated using transaction amount minus quantity multiplied by listed unit cost.

### 6. Revenue by product category
<img width="1184" height="657" alt="category_revenue" src="https://github.com/user-attachments/assets/6f18ab39-88ba-4170-a599-a6419c1bf7d3" />


Electronics contributes ₹18,59,970, followed by Software at ₹4,09,200, Services at ₹1,77,000, and Furniture at ₹1,18,300. Electronics dominates sample revenue, so the business could review category concentration while evaluating cross-selling opportunities in software and services.

### 7. Salesperson revenue versus target
<img width="1483" height="807" alt="salesperson_performance" src="https://github.com/user-attachments/assets/d1af9c69-de96-4285-904b-11eec3f63bd4" />


Compares completed revenue with each salesperson's recorded target. Priti Shah has the highest sample revenue at ₹10,45,900 and 10 completed deals, while the workbook shows several salespeople with zero completed revenue in this small dataset. This should trigger validation of whether they had no sales in the sample period, whether their target periods match, and whether the dataset is complete. The chart should not be treated as a full employee performance evaluation without context.

### 8. Additional workbook views

The Excel workbook also includes dedicated tables for payment methods and order status. Bank Transfer accounts for ₹22,63,770 in completed revenue. The sample contains 34 Completed transactions, one Pending transaction, and one Returned transaction. Because returned and pending transactions are excluded from the headline completed-revenue KPI, those statuses should be monitored separately.

## Key insights and results

- **Revenue concentration by zone:** South contributes approximately 60.1% of completed revenue in the sample.
- **Product concentration:** Laptop Pro X1 contributes approximately 58.8% of completed revenue, so overall performance is sensitive to this product's sales.
- **Customer mix:** Enterprise customers contribute approximately 89.0% of completed revenue, creating a clear focus for account retention and concentration monitoring.
- **Category mix:** Electronics contributes approximately 72.5% of completed revenue.
- **Payment method:** Bank Transfer represents approximately 88.3% of completed revenue.
- **Monthly variability:** December is the strongest month and September the weakest in this 2024 sample; investigate transaction timing and one-off large orders before assuming seasonality.
- **Sales performance:** Priti Shah has the highest recorded revenue in the sample. Zero-revenue salesperson records require context and data validation before conclusions are drawn.
- **Profit contribution:** Laptop Pro X1 leads estimated gross profit, while CRM Software License and Annual Support Plan also contribute materially.

These findings are descriptive, not causal. The sample is intentionally small and should be treated as a project demonstration rather than a statistically representative business forecast.

## SQL techniques demonstrated

| Technique | Business use |
|---|---|
| `SELECT`, `WHERE`, `ORDER BY`, `LIMIT` | Filter and inspect transactions |
| `GROUP BY` and aggregate functions | Summarize revenue, quantity, and order counts |
| `CASE`, `COALESCE`, `NULLIF`, `IFNULL` | Categorization and safe handling of missing values |
| `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`, `SELF JOIN` | Combine facts with dimensions and inspect relationships |
| Subqueries and correlated subqueries | Compare values with aggregate or related results |
| CTEs (`WITH`) | Break complex analysis into readable steps |
| Window functions (`RANK`, `ROW_NUMBER`, `LAG`) | Rank products and compare period-to-period performance |
| Views and stored procedures | Package reusable reporting logic |
| Validation and staging queries | Find inconsistent, missing, or duplicate-like records |

## Project files

```text
SalesIQ/
├── Schema/01_schema.sql
├── Inserts/02_inserts.sql
├── Queries/
│   ├── DQL/03_dql_basics.sql
│   ├── Joins/04_joins.sql
│   ├── SubQueries/05_subqueries.sql
│   ├── Advanced/06_advanced.sql
│   └── Analysis/07_analysis.sql
├── DOCS/README.md
└── SalesIQ_Excel_Dashboard.xlsx
```

The image files referenced by this README are stored in `assets/` in this README package. Copy the README and `assets/` folder together to keep all embedded images visible on GitHub.

## How to run the SQL project

1. Install MySQL 8 and open MySQL Workbench or another MySQL client.
2. Create or select a project database.
3. Run `Schema/01_schema.sql` to create the tables.
4. Run `Inserts/02_inserts.sql` to load the demonstration records.
5. Execute the query files in `Queries/` to explore basic queries, joins, subqueries, advanced analysis, and validation.
6. Open `SalesIQ_Excel_Dashboard.xlsx` in Microsoft Excel to review the prepared charts and supporting summary sheets.

Check the SQL files for database-name assumptions before executing them in your own environment.

## Limitations and next steps

- The workbook is a prepared snapshot; it does not currently refresh from MySQL or Google Sheets automatically.
- The included dataset is small and intended for demonstrating SQL analysis and dashboard storytelling.
- Gross profit is a simplified estimate and excludes operating expenses and other accounting adjustments.
- Revenue concentration should be tested against more months of data before making forecasts or strategic commitments.
- A future version could use a scheduled MySQL-to-Excel/Google Sheets refresh, add return-rate and customer-lifetime-value views, and include filters for date, region, category, and salesperson.

## Presentation summary

“SalesIQ is a MySQL-based sales analytics project that converts transaction-level data into business insights. I designed a relational schema for customers, products, regions, salespeople, transactions, and returns, then used SQL joins, aggregations, subqueries, CTEs, and window functions to analyze the data. I summarized the outputs in an Excel dashboard with KPIs and charts covering regional revenue, product performance, monthly trends, customer segments, gross profit, category mix, and salesperson targets. In the sample data, the South zone, Laptop Pro X1, and Enterprise segment are the largest revenue contributors. These results demonstrate how SQL and dashboarding can help stakeholders spot concentration, performance variation, and areas for further investigation.”

---

**Author note:** Update the project owner details and add a screenshot from your own Excel application if you want to personalize the repository presentation.
