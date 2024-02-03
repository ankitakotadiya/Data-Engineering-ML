# Data-Engineering
Data engineering is a field within data science and analytics that focuses on the practical application of data collection, processing, storage, and analysis. It involves designing and developing systems and infrastructure for managing large volumes of data efficiently and ensuring its reliability, availability, and accessibility.

Data engineering is a field within data science and analytics that focuses on the practical application of data collection, processing, storage, and analysis. It involves designing and developing systems and infrastructure for managing large volumes of data efficiently and ensuring its reliability, availability, and accessibility.

## Amazon S3
Amazon S3, a versatile and highly scalable storage service. S3 serves as a central repository, providing a seamless solution for storing both structured and unstructured data. Its accessibility and flexibility make it an ideal choice for accommodating data from various sources, simplifying the often intricate task of managing diverse datasets.

Setting up a good system for handling data is crucial these days. It gets tricky when data is scattered all over the place. So, I'm going with Amazon S3 – a handy storage service from AWS. It's like a neat, centralized bucket where we can toss in all kinds of data, whether it's well-organized or a bit messy.

Now, getting meaningful info from your data is a big deal. It's even trickier when your data is playing hide-and-seek in different spots. But with Amazon S3, we're bringing all those scattered sources together in one easy-to-reach spot. This not only makes handling data way simpler but also paves the way for doing more cool stuff with it.

First of all, I'll create a bucket and inside will set up a couple of folders within the S3 bucket. One will be called 'Input' to store incoming data, and the other will be 'Output' for the transformed data.

S3 bucket
<img width="1002" alt="Screenshot 2024-02-03 at 12 03 15 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/a7e84173-984f-4a33-9113-b1887dd745ea">

Folders inside bucket
<img width="1000" alt="Screenshot 2024-02-03 at 12 03 33 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/373b5a52-45cb-4d7e-a47c-cd4f2dc3a3cc">

Dataset
<img width="1001" alt="Screenshot 2024-02-03 at 12 03 56 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/d5d0cd07-ab68-47a3-aac9-9b4acaa2723e">

Creating an S3 bucket is simple. During the setup, you just need to give it a unique name and assign an IAM role for data security. This ensures that only authorized folks can access the data. Plus, there's a cool feature called "Query with S3 Select" that lets you sneak a preview of the data. It's simple yet powerful!

Once data from various sources store now it's required to aggregate and perform processing steps before it hits analysis stage. AWS Glue service meet that requirements and allow us to create virutal database, tables, and run ETL jobs to prepate data. Cool right? lets dive in.

## Amazon Glue
Amazon Glue is a fully managed extract, transform, and load (ETL) service offered by Amazon Web Services (AWS). It is designed to simplify and automate the process of preparing and loading data for analysis. Glue provides a serverless environment, eliminating the need for users to manage the underlying infrastructure. The key components of Amazon Glue include:

### Glue Data Catalogs
The AWS Glue Data Catalog contains references to data that is used as sources and targets of your extract, transform, and load (ETL) jobs in AWS Glue. To create your data warehouse or data lake, you must catalog this data. The AWS Glue Data Catalog is an index to the location, schema, and runtime metrics of your data. You use the information in the Data Catalog to create and monitor your ETL jobs. Information in the Data Catalog is stored as metadata tables, where each table specifies a single data store.

<img width="1402" alt="Screenshot 2024-02-03 at 12 07 01 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/57140e5d-e36d-4319-b22e-0f35f8d0d01e">

To create a database and tables is manual process another way is the AWS Crawler.

### Glue Crawler
You can use a crawler to populate the AWS Glue Data Catalog with tables. This is the primary method used by most AWS Glue users. A crawler can crawl multiple data stores in a single run. Upon completion, the crawler creates or updates one or more tables in your Data Catalog. Extract, transform, and load (ETL) jobs that you define in AWS Glue use these Data Catalog tables as sources and targets. The ETL job reads from and writes to the data stores that are specified in the source and target Data Catalog tables. Here are the some steps to create crawler.

Set crawler properties
<img width="1120" alt="Screenshot 2024-02-03 at 12 08 07 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/364eecfb-341e-4f9b-a8f3-4369b94e48ab">

Add Data Source
I have store my data in S3 bucket so source is S3.
<img width="602" alt="Screenshot 2024-02-03 at 12 08 38 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/a39b6354-1005-4792-83c6-dad83add25dd">

Choose data source and Classifier
Define in case you want to use custom classifier otherwise it will automatically classify your datasource formate.
<img width="1118" alt="Screenshot 2024-02-03 at 12 08 58 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/3b02eb8f-362a-402b-95cc-e6fb649a3186">

Configure Security Settings
Here you can define IAM role for glue service.
<img width="1124" alt="Screenshot 2024-02-03 at 12 09 19 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/eabe6387-05b8-4a9a-a7e5-b6d3c5045ff2">

Setup Output and Scheduling
Here you can define database and setup how frequently do you want to extract the data. I have selected on demand so once I hit 'Run' then it will start only.
<img width="1142" alt="Screenshot 2024-02-03 at 12 09 49 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/2cb17ccf-32ce-4b49-975e-f4f6fdac76aa">

Review and Create
<img width="1179" alt="Screenshot 2024-02-03 at 12 10 18 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/4186a99c-01a0-42e9-81a4-6a22e7adf0dc">





