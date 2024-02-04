# AWS Athena
Amazon Athena is an interactive query service that enables you to analyze and query data directly in Amazon Simple Storage Service (Amazon S3) using SQL statements. It is a serverless service, which means you don't need to manage any infrastructure. Athena allows you to analyze large datasets stored in S3 without the need to set up and manage a complex data warehouse.

## Athena SQL
You can use Athena SQL to query your data in-place in Amazon S3 using the AWS Glue Data Catalog.
In Athena, catalogs, databases, and tables are containers for the metadata definitions that define a schema for underlying source data.

Athena uses the following terms to refer to hierarchies of data objects:
* Data source – a group of databases
* Database – a group of tables
* Table – data organized as a group of rows or columns

Sometimes these objects are also referred to with alternate but equivalent names such as the following:

* A data source is sometimes referred to as a catalog.
* A database is sometimes referred to as a schema.

The following example query in the Athena console uses the awsdatacatalog data source, the default database, and the some_table table.
<img width="815" alt="Screenshot 2024-02-04 at 6 16 49 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/edd397d1-e4e0-4684-955d-fd2ba7f1c400">

Other way is you can create a table automatically using an AWS Glue crawler.





