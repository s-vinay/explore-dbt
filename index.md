## dbt - Transform data in your Warehouse

Data build tool (called **dbt** going forward) is a tool for transforming data in your warehouse. The transformed data is the model on which users run their queries for drawing insights to make informed decisions. This learning is a 3-part series and details covered in each part is outlined below.

- Part-1: what dbt can offer and why dbt ?
- Part-2: installing packages with an example
- Part-3: performing tests using popular package dbt-expectations

Before we deep dive into each of these parts it is important to call-out the pre-requisites for this learning series.

- knowledge of SQL

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

The commands are run on the command line shown in the image below.

![image](https://user-images.githubusercontent.com/83854194/127741929-9ec5b5d4-f278-4aa7-b2ff-99d8977e8cc6.png)

Now that you have insights on the why and what of dbt, let us deep dive into packages.

**Part-2: installing packages with an example**

Thinking what are packages and why we need them, here is the answer - packages, like in any other language / tool, are a collection of re-usable libraries which could be called in your transformations instead of building functionalities from scratch. To keep it simple, packages are collection of .sql files which simplifies life of developers.

https://hub.getdbt.com/ has a lot of packages that can be installed to your project and lot more also available on the github.

Some example packages, 
- dbt-utils: has a lot of utilities for handling date / time like "find difference between two dates", "fetch current timestamp" and lot more
- dbt-expectatios: has a lot of out-of-box tests like "expect_column_to_exist - is the input column available in the model?" and many more

Interesting, how to install these packages into our dbt project?
Thanks to dbt, it is as simple as adding the package name to a .yml file and steps have been outlined below.

- step-1: open the packages.yml file, create one in the project root folder if it does not exist already
- step-2: add the content below for adding a package (follow one of the approaches), dbt-expectations is considered as an example here

**Approach-1: Package or hub site**
```
packages:
  - package: calogica/dbt_expectations
    version: [">=0.4.0", "<0.5.0"]
```

**Approach-2: package source from git**

In this approach, the https or the SSH link from the original github source should be copied and this is what we reference to in our .yml file.
Also, instead of the version# we use the branch name that you wanted to use (in our case below it is the master branch).

This method is specifically useful when you have a private repository of your own that can be used in your project.

```
packages:
  - git: git@github.com:calogica/dbt-expectations.git
    revision: master
```
- step-3: now run the command "dbt deps" in the command line. It clones the packages into project and now we are ready to go
