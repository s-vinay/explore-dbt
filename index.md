## dbt - Transform data in your Warehouse

Data build tool (called **dbt** going forward) is a tool for transforming data in your warehouse. The transformed data is the model on which users run their queries for drawing insights to make informed decisions. This learning is a 3-part series and details covered in each part is outlined below.

- Part-1: what dbt can offer and why dbt ?
- Part-2: installing packages with an example
- Part-3: performing tests using popular package dbt-expectations

Before we deep dive into each of these parts it is important to call-out the pre-requisites for this learning series.

- knowledge of SQL
- data structures like list and dictionaries
- knowledge of Jinja template design is an added advantage

Let us now get into action.

**Part-1: What dbt can offer and why dbt?**

Business users always need some historical data to run some analytics for their day-to-day decisions. Some of the examples of analytical decisions are potential customer churn, top / bottom-10 customers, product return analysis, price forecasting and many more. The source data for all such reporting / analytics is in general available in business transactional systems like an ERP or a custom-designed application. This source data needs to be **E**xtracted, **T**ransformed and **L**oaded (called ETL) into data warehouses such as Google Bigquery, Snowflake, AWS Redshift or others.

dbt plays the role of tranforming data which is the **"T"** of the ETL. The traditional methodology of sequentially performing ETL is now transformed by dbt into ELT. In other words, dbt reads the loaded data, performs necessary transformations and generates models into warehouse.

The transformations performed on data could range from a simple user understood column name to complex mathematical calculations into new columns which could be queried directly by the users for analysis.

Considering that we now know what and why dbt let us now get into some technical details.

Technically, dbt are simply set of .sql files which when executed through some commands generates transformed models in the warehouse. Below is a simple .sql file and commands used in dbt.

- Example, dbt .sql transformation

```
with customers as (
    select id as cust_id,
           first_name,
           last_name
    from jaffle_shop.Customers
)

select * from customers
```

`The code above is simply preparing a temporary customers table by renaming "id" column as "cust_id", first_name and last_name as it is from the data warehouse source table "Customers" (jaffle_shop is the database name in the data warehouse which holds the Customers table). Then selects all data from the temporary table using the select * query.`

- Commands in dbt are used to perform actions on the .sql files. To generate a model in the data warehouse use "dbt run". Similary to run some tests on a model the command is "dbt test". There are other options that can be used with each of the commands and several other commands also exist, but we keep it simple for now.
