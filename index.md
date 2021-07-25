## dbt - Transform data in your Warehouse

Data build tool (called **dbt** going forward) is a cloud based tool for transforming data in your warehouse. The transformed data is the model on which users run their queries for drawing insights to make informed decisions. This learning is a 3-part series and details covered in each part is outlined below.

- Part-1: what dbt can offer and why dbt ?
- Part-2: installing packages with an example
- Part-3: performing tests using popular package dbt-expectations

Before we deep dive into each of these parts it is important to call-out the pre-requisites for this learning series.

- knowledge of SQL
- Python data structures like list and dictionaries
- knowledge of Jinja template design is an added advantage

Let us now get into action.

**Part-1: What dbt can offer and why dbt?**

Business users always need some historical data to run some analytics for their day-to-day decisions. Some of the examples of analytical decisions are potential customer churn, top / bottom-10 customers, product return analysis, price forecasting and many more. The source data for all such reporting / analytics is in general available in business transactional systems like an ERP or a custom-designed application. This source data needs to be **E**xtracted, **T**ransformed and **L**oaded (called ETL) into data warehouses such as Google Bigquery, Snowflake, AWS Redshift or others.

dbt plays the role of tranforming data which is the **"T"** of the ETL. The traditional methodology of sequentially performing ETL is now transformed by dbt into ELT. In other words, dbt reads the loaded data, performs necessary transformations and generates models into warehouse.

The transformations performed on data could range from a simple user understood column name to complex mathematical calculations into new columns which could be queried directly by the users for analysis.

Considering that we now know what and why dbt let us now get into some technical details.

Technically, dbt are simply set of .sql files which when executed through some commands generates transformed models in the warehouse. Below is a simple .sql file and commands used in dbt.

- Example, dbt .sql transformation

_with customers as (
    select id as cust_id,
           first_name,
           last_name
    from {{ source('jaffle_shop', 'Customers') }}
)_

_select * from customers_

The code above is simply preparing a temporary customers table by renaming "id" column as "cust_id", first_name and last_name as it is from the data warehouse source table "Customers". Then selects all data from the temporary table using the select * query.

- Commands in dbt are used to perform actions on the .sql files. To generate a model in the data warehouse use "dbt run". Similary to run some tests on a model the command is "dbt test". There are other options that can be used with each of the commands and several other commands also exist, but we keep it simple for now.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/s-vinay/explore-dbt/settings/pages). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
