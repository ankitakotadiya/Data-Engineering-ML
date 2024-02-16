# Amazon EMR
Amazon EMR (previously called Amazon Elastic MapReduce) is a managed cluster platform that simplifies running big data frameworks, such as Apache Hadoop and Apache Spark, on AWS to process and analyze vast amounts of data. Using these frameworks and related open-source projects, you can process data for analytics purposes and business intelligence workloads. Amazon EMR also lets you transform and move large amounts of data into and out of other AWS data stores and databases, such as Amazon Simple Storage Service (Amazon S3) and Amazon DynamoDB.

With Amazon EMR you can set up a cluster to process and analyze data with big data frameworks in just a few minutes. This tutorial shows you how to launch a sample cluster using Spark, and how to run a simple PySpark script stored in an Amazon S3 bucket. It covers essential Amazon EMR tasks in three main workflow categories: Plan and Configure, Manage, and Clean Up.

<img width="553" alt="Screenshot 2024-02-16 at 11 42 43 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/ae26c398-2973-4ba6-bc9f-a5ffb1f55956">

## Step 1: Plan and configure an Amazon EMR cluster
### Prepare storage for Amazon EMR
When you use Amazon EMR, you can choose from a variety of file systems to store input data, output data, and log files. In this tutorial, you use EMRFS to store data in an S3 bucket. EMRFS is an implementation of the Hadoop file system that lets you read and write regular files to Amazon S3.

### Prepare an application with input data for Amazon EMR
The most common way to prepare an application for Amazon EMR is to upload the application and its input data to Amazon S3. Then, when you submit work to your cluster you specify the Amazon S3 locations for your script and data.
##### To prepare the example PySpark script for EMR
1. Copy the example code below into a new file in your editor of choice. I have used Visual Studio Code. 

```
import argparse

from pyspark.sql import SparkSession

def calculate_red_violations(data_source, output_uri):
    """
    Processes sample food establishment inspection data and queries the data to find the top 10 establishments
    with the most Red violations from 2006 to 2020.

    :param data_source: The URI of your food establishment data CSV, such as 's3://DOC-EXAMPLE-BUCKET/food-establishment-data.csv'.
    :param output_uri: The URI where output is written, such as 's3://DOC-EXAMPLE-BUCKET/restaurant_violation_results'.
    """
    with SparkSession.builder.appName("Calculate Red Health Violations").getOrCreate() as spark:
        # Load the restaurant violation CSV data
        if data_source is not None:
            restaurants_df = spark.read.option("header", "true").csv(data_source)

        # Create an in-memory DataFrame to query
        restaurants_df.createOrReplaceTempView("restaurant_violations")

        # Create a DataFrame of the top 10 restaurants with the most Red violations
        top_red_violation_restaurants = spark.sql("""SELECT name, count(*) AS total_red_violations 
          FROM restaurant_violations 
          WHERE violation_type = 'RED' 
          GROUP BY name 
          ORDER BY total_red_violations DESC LIMIT 10""")

        # Write the results to the specified output URI
        top_red_violation_restaurants.write.option("header", "true").mode("overwrite").csv(output_uri)

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument(
        '--data_source', help="The URI for you CSV restaurant data, like an S3 bucket location.")
    parser.add_argument(
        '--output_uri', help="The URI where output is saved, like an S3 bucket location.")
    args = parser.parse_args()

    calculate_red_violations(args.data_source, args.output_uri)
			
```
2. Save the file as health_violations.py.
3. Upload health_violations.py to Amazon S3 into the bucket you created for this tutorial.

##### To prepare the sample input data for EMR
1. Download the zip file, [food_establishment_data.zip](https://docs.aws.amazon.com/emr/latest/ManagementGuide/samples/food_establishment_data.zip).
2. Unzip and save food_establishment_data.zip as food_establishment_data.csv on your machine.
3. Upload the CSV file to the S3 bucket that you created for this tutorial.

### Launch an Amazon EMR cluster
After you prepare a storage location and your application, you can launch a sample Amazon EMR cluster.

1. Under EMR on EC2 in the left navigation pane, choose Clusters, and then choose Create cluster.
2. On the Create Cluster page, note the default values for Release, Instance type, Number of instances, and Permissions. These fields automatically populate with values that work for general-purpose clusters.
3. In the Cluster name field, enter a unique cluster name to help you identify your cluster, such as My first cluster. Your cluster name can't contain the characters <, >, $, |, or ` (backtick).
4. Under Applications, choose the Spark option to install Spark on your cluster.
5. Under Cluster logs, select the Publish cluster-specific logs to Amazon S3 check box. Replace the Amazon S3 location value with the Amazon S3 bucket you created, followed by /logs. For example, s3://DOC-EXAMPLE-BUCKET/logs. Adding /logs creates a new folder called 'logs' in your bucket, where Amazon EMR can copy the log files of your cluster.
6. Under Security configuration and permissions, choose your EC2 key pair. In the same section, select the Service role for Amazon EMR dropdown menu and choose EMR_DefaultRole. Then, select the IAM role for instance profile dropdown menu and choose EMR_EC2_DefaultRole.
7. Choose Create cluster to launch the cluster and open the cluster details page.
8. Find the cluster Status next to the cluster name. The status changes from Starting to Running to Waiting as Amazon EMR provisions the cluster. You may need to choose the refresh icon on the right or refresh your browser to see status updates.

Your cluster status changes to Waiting when the cluster is up, running, and ready to accept work.

## Step 2: Manage your Amazon EMR cluster
### Submit work to Amazon EMR
After you launch a cluster, you can submit work to the running cluster to process and analyze data. You submit work to an Amazon EMR cluster as a step. A step is a unit of work made up of one or more actions. For example, you might submit a step to compute values, or to transfer and process data. You can submit steps when you create a cluster, or to a running cluster. In this part of the tutorial, you submit health_violations.py as a step to your running cluster.

1. Under EMR on EC2 in the left navigation pane, choose Clusters, and then select the cluster where you want to submit work. The cluster state must be Waiting.
2. Choose the Steps tab, and then choose Add step.
3. Configure the step according to the following guidelines:
   * For Type, choose Spark application. You should see additional fields for Deploy mode, Application location, and Spark-submit options.
   * For Name, enter a new name. If you have many steps in a cluster, naming each step helps you keep track of them.
   * For Deploy mode, leave the default value Cluster mode. For more information on Spark deployment modes.
   * For Application location, enter the location of your health_violations.py script in Amazon S3, such as s3://DOC-EXAMPLE-BUCKET/health_violations.py.
   * Leave the Spark-submit options field empty.
   * In the Arguments field, enter the following arguments and values:
     ```
     --data_source s3://DOC-EXAMPLE-BUCKET/food_establishment_data.csv
     --output_uri s3://DOC-EXAMPLE-BUCKET/myOutputFolder						
     ```







