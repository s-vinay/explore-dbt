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

Let us make it even more interesting by installing package dbt-expectations and perform some tests that it offers.

**Part-3: perform tests using dbt-expectations**

Considering that you have been following this series and finished installing package dbt-expectations as explained in part-2, we will straight away get into action.

By the way, is testing important even for data engineering / analytics / transformation?
The simple answer is yes. Testing plays a vital role even in data engineering to enable developers / testers to ensure requirement coverage and in turn avoids production defects. Still do not agree? let us understand this through a business situation.

_Business ask: We need to draw insights on different payment methods used by our customers
Purpose of the ask: this will enable us to learn the customer payment patterns and work with associated bank partners to enable some offers / cashbacks to attract more buying

_Now, we have done a great job and transformed multiple tables into a model that could be queried into a report and deployed to production. Assume there was a glitch that data for couple of payment methods were filtered out in our transformed model through some logic unexpectedly. Since this was never tested, it was not found as well. Users have started using the report and analytics have been drawn that the two missing payment methods were never used by customers. What insights this could lead to now is left to your imagination and there are high chances that the defect is not found for a very long time.

It is important to understand that testing is important and above scenario explains why it is even more important for data.

dbt-expectations offer a wide variety of tests but we will show one as an example and the rest can be experimented in a similar fashion. To support you further the github documentation is also provided towards the end.

test: expect_table_column_count_to_equal
description of the test: The test allows us to validate if the columns in the transformed model matches the business requirement (which is provided as an input to the test).
steps to perform the test:

- step-1: create a tests.yml file in your models directory. The file name could be any based on your naming convention.
- step-2: add below code to your .yml file. The name of your model for which you wanted to test should go into the key "name" and my model name is "stg_customers". The tests section will list all the tests that you would like to perform on this model. It is important to note each test has one or more input parameters to be supplied, like in this case there is only one input called "value" which holds the count of columns that you are expecting in the model

```
version: 2

models:
    - name: stg_customers
      tests:
        - dbt_expectations.expect_table_column_count_to_equal:
            value: 3
```

if you now wanted to add another test for the same model "stg_customers", below is the code and important to note that we are neither creating a new .yml file nor we create new tests section.

```
version: 2

models:
    - name: stg_customers
      tests:
        - dbt_expectations.expect_table_column_count_to_equal:
            value: 3
        - dbt_expectations.expect_table_column_count_to_be_between:
            min_value: 3
            max_value: 5
```

- step-3: now that you are ready with the test configurations, we simply run the command "dbt test" to start the validations as per the inputs provided. The logs are shown as below in the run section

![image](https://user-images.githubusercontent.com/83854194/127744806-9708f5dd-2c3c-4f25-a822-c9f07db2e2b1.png)

The github repository https://github.com/calogica/dbt-expectations has all the documentation to enable you in using the package.

Happy transformation !
