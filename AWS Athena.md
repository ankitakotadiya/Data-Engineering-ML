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

Other way is you can create a table automatically using an AWS Glue crawler. Make sure you've setup query result location path in your s3 bucket.
<img width="1323" alt="Screenshot 2024-02-04 at 6 40 55 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/bdf74555-f65c-4a99-88b1-a25f4278db56">

You can use Amazon Athena to query data stored in different locations and formats in a dataset. This dataset might be in CSV, JSON, Avro, Parquet, or some other format.

### Integration with AWS Glue
AWS Glue is a fully managed ETL (extract, transform, and load) AWS service. One of its key abilities is to analyze and categorize data. You can use AWS Glue crawlers to automatically infer database and table schema from your data in Amazon S3 and store the associated metadata in the AWS Glue Data Catalog.

### Using Athena Data Connector for External Hive Metastore
The workflow for using external Hive metastores from Athena includes the following steps.

1. You create a Lambda function that connects Athena to the Hive metastore that is inside your VPC.
2. You register a unique catalog name for your Hive metastore and a corresponding function name in your account.
3. When you run an Athena DML or DDL query that uses the catalog name, the Athena query engine calls the Lambda function name that you associated with the catalog name.
4. Using AWS PrivateLink, the Lambda function communicates with the external Hive metastore in your VPC and receives responses to metadata requests. Athena uses the metadata from your external Hive metastore just like it uses the metadata from the default AWS Glue Data Catalog.

<img width="661" alt="Screenshot 2024-02-05 at 11 08 54 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/3bdf0de0-a453-4930-9ce9-8408c492379f">








