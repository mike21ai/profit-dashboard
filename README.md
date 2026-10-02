# Maven Toys Profit Dashboard

A three-page Power BI report on the Maven Toys retail dataset. Each page answers one question: where profit comes from, which products carry margin, and how customers buy.

I started this project on my own, using what I learned in an analytics course. I built it with Power BI and DAX.

## Introduction

Revenue and profit tell different stories. A category can bring in the most revenue and still contribute a smaller share of profit, because its margin is lower. This report separates the two, groups stores into clusters so a store is compared with similar stores, and looks at how many units a typical sale contains.

## Dataset

Maven Toys is a sample dataset for a toy store chain in Mexico. Prices and costs are in USD, as listed in the data dictionary.

| Item | Value |
|---|---|
| Sale records | 829,262, one row per sale |
| Period | 1 January 2022 to 30 September 2023 |
| Stores | 50, in 29 cities, in four location types (Downtown, Commercial, Residential, Airport) |
| Products | 35, in 5 categories (Toys, Electronics, Art & Crafts, Games, Sports & Outdoors) |
| Inventory | stock on hand per store and product, 1,593 rows |

The source files (sales, products, stores, inventory, calendar and a data dictionary) are in `dataset/Maven+Toys.zip`.

## Data model

`FactSales` holds the sales and links many-to-one to `stores` and `products` through Store_ID and Product_ID. `inventory` links to the same two tables. Two tables are derived inside Power BI: `ClusterMappingTable`, which assigns each store to a cluster, and `TransactionSummary`, which gives units per transaction.

## Main measures

| Measure | Definition |
|---|---|
| Total Revenue | sum of units x price |
| Total Cost | sum of units x product cost |
| Total Profit | sum of units x (price - cost) |
| Avg Profit Margin % | total profit divided by total revenue |
| Total Transactions | distinct sale IDs |
| Avg Units per Transaction | total units divided by total transactions |
| Avg Revenue per Transaction | total revenue divided by total transactions |
| Profit Last Year | profit for the same period one year earlier |
| YoY Profit Growth % | change in profit against the same period last year |
| 30-Day Moving Average Profit | average daily profit over the trailing 30 days |

Store clusters come from Power BI's k-means clustering on each store's total profit and total revenue. It produces three clusters.

## Dashboard pages

### 1. Profit Dashboard

Where profit comes from. Four KPI cards, a scatter of store revenue against profit coloured by cluster, profit by city, profit share by product category, and a 30-day moving average of profit with a forecast band. Filters for product category and store location.

<img width="1132" height="643" alt="image" src="https://github.com/user-attachments/assets/e1e921cc-e973-4816-b797-dc8887d82b6c" />

### 2. Margin Dashboard

Which products carry margin. A scatter of average margin against units sold for each product, split into four quadrants by reference lines. Average margin by category, a treemap of profit and margin by product, and margin over time by day, month, quarter and year. Filters for city, year and quarter, category and location.

<img width="1107" height="628" alt="image" src="https://github.com/user-attachments/assets/ad33a525-3c49-4537-ad09-c9286dfb54e2" />

### 3. Sales Pattern Dashboard

How customers buy. Units per transaction, each city's profit split by price band (cheap under $10, mid-range $10 to $20, expensive over $20), average units per transaction by store location, and a decomposition tree from price band to store location, city and product category. The price band labels in the report are in Indonesian (Murah, Menengah, Mahal).

<img width="1095" height="612" alt="image" src="https://github.com/user-attachments/assets/9f5eeeae-5242-496c-855b-707cde4e1a53" />

## What the data shows

Figures below are calculated from the sales table with the same formulas as the measures above.

| Category | Share of profit | Profit margin |
|---|---|---|
| Toys | 26.9% | 21.2% |
| Electronics | 24.9% | 44.6% |
| Art & Crafts | 18.8% | 27.8% |
| Games | 16.8% | 30.3% |
| Sports & Outdoors | 12.6% | 23.3% |

Toys bring in the largest share of profit and have the lowest margin. Electronics have the highest margin and contribute almost as much profit with about half the units sold. Across all categories the average margin is 27.8%.

83.1% of sale records contain a single unit.

The 50 stores fall into three clusters of 34, 15 and 1 store. The single-store cluster is one high-profit store that sits apart from the rest in the scatter.

## Notes on reading the report

The sales table has one row per sale ID, so units per transaction means units on one sale record. It does not describe a basket of different products, and questions about what is bought together cannot be answered from this data.

The forecast band on the first page comes from Power BI's built-in forecasting, not from a separate model.

The KPI cards show the value for the last date on their trend axis, which is 30 September 2023 (profit 8.83K, revenue 34.72K, 2,567 units). They are not totals for the whole period.

## Opening the report

Open `Maven Toys Profitability Dashboard.pbix` in Power BI Desktop. The data is stored inside the file, so the report opens without any setup. To refresh from source, unzip `dataset/Maven+Toys.zip` and point the queries at the extracted CSV files.

## Repository contents

| File | Purpose |
|---|---|
| `Maven Toys Profitability Dashboard.pbix` | Power BI report, three pages |
| `dataset/Maven+Toys.zip` | source CSV files and data dictionary |
