# Data Analysis using Power BI

# Table of Contents
- [Introduction](#introduction)
- [Technology/framework used](#technologyframework-used)
- [File Structure](#file-structure)
1. [Step 1: Importing and Transforming Data](#step-1-importing-and-transforming-data)
1. [Step 2: Creating the Star Data Model](#step-2-creating-the-star-data-model)
1. [Step 3: Setting up the Report](#step-3-setting-up-the-report)
1. [Step 4: Building the Customer Detail Page](#step-4-building-the-customer-detail-page)
1. [Step 5: Building the Executive Summary Page](#step-5-building-the-executive-summary-page)
1. [Step 6: Building the Product Detail Page](#step-6-building-the-product-detail-page)
1. [Step 7: Buiding a the Stores Map Page](#step-7-building-the-stores-map-page)
1. [Step 8: Cross Filtering and Navigation](#step-8-cross-filtering-and-navigation)
1. [Step 9: Creating Metrics for Users Outside the Company Using SQL](#step-9-creating-metrics-for-users-outisde-the-company-using-sql)

## Introduction:
 This is a project assigned to me by AiCore as part of my Data Analytics Bootcamp, during this project I have been tasked with creating a report, by importing and cleaning data from various sources in Power BI.

---

## Technology/framework used:
- Power BI Desktop
- DAX Language
- M Language
- VSCode (SQLTools) 
- CLI

---
## File Structure:
```
src
│   .gitignore
│   Power_BI_Report.pbix
│   README.md
├───navigation_bar_images
│       Customer_Icon.png
│       Customer_Icon_Blue.png
│       Dashboard_Icon.png
│       Dashboard_Icon_Blue.png
│       Filter_Icon.png
│       Filter_Icon_Blue.png
│       Map_Icon.png
│       Map_Icon_Blue.png
│       Product_Icon.png
│       Product_Icon_Blue.png
│       stores_default.png
│       stores_hover.png
│
├───sql_metrics
│       question_1.csv
│       question_1.sql
│       question_2.csv
│       question_2.sql
│       question_3.csv
│       question_3.sql
│       question_4.csv
│       question_4.sql
│       question_5.csv
│       question_5.sql
│
└───table_column_names
        country_region_columns.csv
        dim_customer_columns.csv
        dim_date_columns.csv
        dim_product_columns.csv
        dim_store_columns.csv
        forquerying2_columns.csv
        forview_columns.csv
        orders_columns.csv
        table_column_names.sql
        table_names.csv
```

## Step 1: Importing and Transforming Data.

1. First I used the SQL Server Import Wizard to import and transform the Orders Table from an Azure SQL Database. Then Using the Power Query Editor I removed any privacy violating columns, and split order date and shipping date into 2 seperate column, and using the built in column visualisations to see the Column Quality, I could remove rows with NULL values.

1. I then used the CSV import wizard to import and transform the Products table. Using the column from example function in the Power Query Editor I created a [unit] and [values] column from the [weights] column. As well as from the Data view I created a converted value which finds any weights not in kg and converts them to kg.

1. To import the Stores dimension table I used to azure blob storage import wizard with an account key to securely extract the data.

1. For the customers table I used the folder import wizard, this was so I could merge 3 csv files to create one table. Using the column from examples function I created a new column to combine the first and last name to get the [Full Name].

---

## Step 2: Creating the Star Data Model.

1. First I created a Date table with different forms of dates, this is to have a continuous date table use Power BI's Time Intelligence.

1. After creating the Date table I went to the model view so I could create the relationships to different tables and made sure the correct relationships were active.
![Model View](https://i.imgur.com/z39WCnp.png)
1. I then created a Measures Table and filled them with Key Measures.   
![Measures Table](https://i.imgur.com/ISJMZnG.png)
1. After I was finished with these Measures, I created a new calculated column in Stores table which used the SWITCH function to turn the Stores[Country Code] into their respective country. Then I made sure each column had the right data category assigned. Tghen I finally create Date and Geography hierarchies.

---

## Step 3: Setting up the Report.

1. This step is relatively simple, in the **Data View** I created 4 pages for each level of detail.
![pages](https://i.imgur.com/LjIyMkC.png)

1. I then added a side bar to each page that will be used for navigation (we will come back to this at a later step).

---

## Step 4: Building the Customer Detail Page.

1. I began with selecting a theme for my report using the in-built themes. After that I created 2 headline cards, one for the total unique customers, and one for the revenue per customer.

![cards](https://i.imgur.com/xVUKhZL.png)
1. I then created 2 summary charts, one is a donut chart and the other a column chart, to show respectively the "Customers by Country", and the "Customers by Category".
![charts](https://i.imgur.com/PcWVzFH.png)
1. Following this I created a line chart with a trend line and forecasting into the future with a confidence interval of 95% of the total customers by a date hierarchy.
1. After the line graph, I created a table to show the full names total orders and Total Revenue of different customers, and by using the TopN Method I filtered it out to the top 20 customers.
![table of customers](https://i.imgur.com/Sn3k4Vc.png)
1. I created 3 more cards to show the top customer in more depth.
![Top Customer](https://i.imgur.com/H8F7XAF.png)
1. Finally I Created a Date Slicer to give the user a more customisable experience in the report.
1. After tweaking the page I ended up with a Customer Detail Page looking as follows:
![Customer Page](https://i.imgur.com/tVpQnrm.png)
---
## Step 5: Building the Executive Summary Page.

1. To kick off the Executive Summary I created 3 cards, corresponding to Total Revenue, Total Profit and Total Orders
![Exec Cards](https://i.imgur.com/khYNPiE.png)

1. After those cards I created a total revenue line graph, this is the same as the customers line graph but for revenue.
![Revenue line graph](https://i.imgur.com/1s03Kdf.png)
1. I then added the Summary Charts.
![Donut charts](https://i.imgur.com/dn7lq9h.png)
![Bar Chart](https://i.imgur.com/AxOjOiu.png)
1. Moving on from the summary charts I wanted to create some KPI visualisations. In order to do this I first had to create some quarterly measures (for revenue, profit and orders as that was the KPI indicators I chose) as well as target measures for those categories.
![KPI](https://i.imgur.com/G9qeDlz.png)
1. Putting it all together I got an executive summary page looking like this:
![Exec Summary](https://i.imgur.com/MX1SFTg.png)
---
## Step 6: Building the Product Detail Page.

1. I began this page with 3 gauge visualisations. Each for the Quarterly Targets for Orders, Profit and Revenue.
![Gauge](https://i.imgur.com/rEFeit6.png)

1. I then created 2 cards to show the selections the user made. (How the selections would be made will come later).
![Cards](https://i.imgur.com/x4x71lq.png)
1. I simply added an area chart of revenue by category.
![area chart](https://i.imgur.com/eecWR0a.png)
1. Similar to the customer detail page, I created a TopN (this time top 10) table but for the products instead of customers with more columns.
![table](https://i.imgur.com/zF6qYCg.png)
1. I followed up by creating a Scatter Plot of Quantity vs Profitability.
![Scatter Plot](https://i.imgur.com/hB4IgXp.png)
1. After creating all my plots and graphs, I created a slicer tool page by creating a large rectangle, adding two slicers  and a back button, in order to function properly I had to ensure that the buttons that were to link to each instance of the page were bookmarked correctly.

![closed](https://i.imgur.com/4OROws8.png) ![open](https://i.imgur.com/u0co5Z8.png) ![bookmarks](https://i.imgur.com/b0Ndvju.png)

1. I ended up with a page like this:
![page](https://i.imgur.com/jqUp9DF.png)

---
## Step 7: Building the Stores Map Page.

1. I began with a Map Visual, with the Regional Hierarchy being the Location.
![Stores Map](https://i.imgur.com/lx90pKL.png)

1. To allow the user a more refined experience I added a country slicer.
![country slicer](https://i.imgur.com/sfeYB8Y.png)
1. To make it easy for the region managers to check on the progress of a given store, I made a stores drill through page which allowed them to pick a specific location to drill through, with various visuals.
![drill through page](https://i.imgur.com/2xOu3Jy.png)
1. the drill through field was the country region.     
![field](https://i.imgur.com/F4iLG4b.png)
1. I then created a stores tooltip page, with the profit gauge visual.
![tooltip](https://i.imgur.com/g1ZDENk.png)

---
## Step 8: Cross Filtering and Navigation.

1. First I fixed the cross filtering by making sure some visualisations dont filter with others which could potentially break the visualisation.

1. After finishing with the cross filtering i fully completed the navigation bar
![Navigation bar](https://i.imgur.com/ybRwTWM.png)

## Step 9: Creating Metrics for Users Outisde the Company Using SQL

1. First I connected to the Postgres Server using the SQLTools extension in VSCode.

1. Then using sql queries I made a list of the table and column names you can find these in the [table_column_names](./table_column_names/) folder.

1. Finally I used SQL to query for different insights and questions to be answered, these questions were:
-  1. How many staff are there in all of the UK stores? ([Query](./sql_metrics/question_1.sql)) ([Answer](./sql_metrics/question_1.csv))
-   2. Which month in 2022 has had the highest revenue? ([Query](./sql_metrics/question_2.sql))([Answer](./sql_metrics/question_2.csv))
- 3. Which German store type had the highest revenue for 2022? ([Query](./sql_metrics/question_3.sql))([Answer](./sql_metrics/question_3.csv))
- 4. Create a view where the rows are the store types and the columns are the total sales, percentage of total sales and the count of orders ([Query](./sql_metrics/question_4.sql))([Answer](./sql_metrics/question_4.csv))
- 5. Which product category generated the most profit for the "Wiltshire, UK" region in 2021? ([Query](./sql_metrics/question_5.sql))([Answer](./sql_metrics/question_5.csv))

you can view all the queries/answers in the [sql_metrics](./sql_metrics/) folder