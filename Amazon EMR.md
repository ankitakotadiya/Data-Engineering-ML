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
   * For Action if step fails, accept the default option Continue. This way, if the step fails, the cluster continues to run.
4. Choose Add to submit the step. The step should appear in the console with a status of Pending.
5. Monitor the step status. It should change from Pending to Running to Completed. To refresh the status in the console, choose the refresh icon to the right of Filter. The script takes about one minute to run. When the status changes to Completed, the step has completed successfully.

### View results
To view the results of health_violations.py
1. Choose the Bucket name and then the output folder that you specified when you submitted the step. For example, DOC-EXAMPLE-BUCKET and then myOutputFolder.
2. Verify that the following items appear in your output folder:
   * A small-sized object called _SUCCESS.
   * A CSV file starting with the prefix part- that contains your results.
3. Choose the object with your results, then choose Download to save the results to your local file system.

##### The following is an example of health_violations.py results.
```
name, total_red_violations
SUBWAY, 322
T-MOBILE PARK, 315
WHOLE FOODS MARKET, 299
PCC COMMUNITY MARKETS, 251
TACO TIME, 240
MCDONALD'S, 177
THAI GINGER, 153
SAFEWAY INC #1508, 143
TAQUERIA EL RINCONSITO, 134
HIMITSU TERIYAKI, 128
```

## Step 3: Clean up your Amazon EMR resources
### Terminate your cluster
Now that you've submitted work to your cluster and viewed the results of your PySpark application, you can terminate the cluster. Terminating a cluster stops all of the cluster's associated Amazon EMR charges and Amazon EC2 instances.

## Amazon EMR Studio
Amazon EMR Studio is a web-based integrated development environment (IDE) for fully managed Jupyter notebooks that run on Amazon EMR clusters. You can set up an EMR Studio for your team to develop, visualize, and debug applications written in R, Python, Scala, and PySpark. EMR Studio is integrated with AWS Identity and Access Management (IAM) and IAM Identity Center so users can log in using their corporate credentials.

##### Key features of EMR Studio
* Authenticate users with AWS Identity and Access Management (IAM), or with AWS IAM Identity Center with or without trusted identity propagation and your enterprise identity provider.
* Access and launch Amazon EMR clusters on-demand to run Jupyter Notebook jobs.
* Connect to Amazon EMR on EKS clusters to submit work as job runs.
* Explore and save example notebooks. For more information about example notebooks, see the [EMR Studio Notebook examples GitHub repository](https://github.com/aws-samples/emr-studio-notebook-examples).
* Analyze data using Python, PySpark, Spark Scala, Spark R, or SparkSQL, and install custom kernels and libraries.
* Collaborate in real time with other users in the same Workspace.
* Use the EMR Studio SQL Explorer to browse your data catalog, run SQL queries, and download results before you work with the data in a notebook.
* Run parameterized notebooks as part of scheduled workflows with an orchestration tool such as Apache Airflow or Amazon Managed Workflows for Apache Airflow. For more information, see Orchestrating analytics jobs on EMR Notebooks using [MWAA](https://aws.amazon.com/blogs/big-data/orchestrating-analytics-jobs-on-amazon-emr-notebooks-using-amazon-mwaa/) in the AWS Big Data Blog.
* Link code repositories such as GitHub and BitBucket.
* Track and debug jobs using the Spark History Server, Tez UI, or YARN timeline server.

### Create an EMR Studio
1. Open the Amazon [EMR console](https://console.aws.amazon.com/emr).
2. Under EMR Studio on the left navigation, choose Getting started. You can also create a new Studio from the Studios page.
3. Amazon EMR provides default settings for you if you're creating a EMR Studio for interactive workloads, but you can edit these settings. Configurable settings include the EMR Studio's name, the S3 location for your Workspace, the service role to use, the Workspace(s) you want to use, EMR Serverless application name, and the associated runtime role.
4. Choose Create Studio and launch Workspace to finish and navigate to the Studios page. Your new Studio appears in the list with details such as Studio name, Creation date, and Studio access URL. Your Workspace opens in a new tab in your browser.

### Create EMR Notebooks
You can use Amazon EMR Notebooks along with Amazon EMR clusters running Apache Spark to create and open Jupyter Notebook and JupyterLab interfaces within the Amazon EMR console. An EMR notebook is a "serverless" notebook that you can use to run queries and code.

1. Open the [Amazon EMR console](https://console.aws.amazon.com/elasticmapreduce/).
2. Choose Notebooks, Create notebook.
3. Enter a Notebook name and an optional Notebook description.
4. If you have an active cluster to which you want to attach the notebook, leave the default Choose an existing cluster selected, click Choose, select a cluster from the list, and then click Choose cluster. For information about cluster requirements for EMR Notebooks, see Considerations when using EMR Notebooks.
   —or—
   Choose Create a cluster, enter a Cluster name and choose options according to the following guidelines. The cluster is created in the default VPC for the account using On-Demand instances.
5. For Security groups, choose Use default security groups. Alternatively, choose Choose security groups and select custom security groups that are available in the VPC of the cluster. You select one for the primary instance and another for the notebook client instance.
6. For AWS Service Role, leave the default or choose a custom role from the list. The client instance for the notebook uses this role.
7. For Notebook location choose the location in Amazon S3 where the notebook file is saved, or specify your own location. If the bucket and folder don't exist, Amazon EMR creates it.
   Amazon EMR creates a folder with the Notebook ID as folder name, and saves the notebook to a file named NotebookName.ipynb. For example, if you specify the Amazon S3 location s3://MyBucket/MyNotebooks for a notebook named MyFirstEMRManagedNotebook, the notebook file is saved to s3://MyBucket/MyNotebooks/NotebookID/MyFirstEMRManagedNotebook.ipynb.
   If you specify an encrypted location in Amazon S3, you must set up the Service role for EMR Notebooks as a key user. The default service role is EMR_Notebooks_DefaultRole. If you are using an AWS KMS key for encryption, see Using key policies in AWS KMS in the AWS Key Management Service Developer Guide and the support article for adding key users.
8. Optionally, if you have added a Git-based repository to Amazon EMR that you want to associate with this notebook, choose Git repository, select Choose repository and then select a repository from the list.
9. Optionally, choose Tags, and then add any additional key-value tags for the notebook.
10. Choose Create Notebook.






























	







