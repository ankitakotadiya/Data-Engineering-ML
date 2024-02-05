# Amazon QuickSight
Amazon QuickSight is a cloud-based business intelligence (BI) service provided by Amazon Web Services (AWS). It is designed to enable organizations to easily analyze, visualize, and share insights from their data. QuickSight allows users to create interactive dashboards, reports, and perform ad-hoc analysis, making it a powerful tool for data visualization and business intelligence.

Here are some of the benefits of using Amazon QuickSight for analytics, data visualization, and reporting:
* The in-memory engine, called SPICE, responds with blazing speed.
* No upfront costs for licenses and a low total cost of ownership (TCO).
* Collaborative analytics with no need to install an application.
* Combine a variety of data into one analysis.
* Publish and share your analysis as a dashboard.
* Control features available in a dashboard.
* No need to manage granular database permissions—dashboard viewers can see only what you share.

How to sign in to Amazon QuickSight
First you have to complete signup process if you don't have any QuickSight account where it will ask you few steps about account name, region, IAM role, and email in which you want to receive notifications. Once successfully signup you will see the QuickSight dashboard below.
<img width="1433" alt="Screenshot 2024-02-05 at 4 08 05 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/2ef8f5b4-2a8b-4a5d-8e11-a0dc1c63fe0e">

## Connecting to data in Amazon QuickSight
People in many different roles use Amazon QuickSight to help them do analysis and advanced calculations, design data dashboards, embed analytics, and make better-informed decisions. Before any of that can happen, someone who understands your data needs to add it to QuickSight. QuickSight supports direct connections and uploads from a variety of sources.
<img width="1090" alt="Screenshot 2024-02-05 at 4 08 21 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/55aba1ba-af21-40b2-b587-f6b633db761a">

After your data is available in QuickSight Standard edition, you can do the following:
* Transform the dataset with field formatting, hierarchies, data type conversions, and calculations.
* Create one or more data analyses based on your newly created dataset.
* Share your analysis with other people so they can help design it.
* Add charts, graphs, more datasets, and multiple pages (called sheets) to your data analysis.
* Create visual appeal with customized formatting and themes.
* Make them interactive by using parameters, controls, filters, and custom actions.
* Combine data from multiple data sources, and then build new hierarchies for drilling down and calculations only available during analytics, like aggregations, window functions, and more.
* Publish your analysis as an interactive data dashboard.
* Share the dashboard so other people can use the dashboard, even if they don't use the analysis that it's based on.
* Add more data to create more analyses and dashboards.

### Creating a dataset using Amazon Athena data
To create a new Athena connection profile (less common), use the following steps:
* In the FROM NEW DATA SOURCES section, choose the Athena data source card.
* For Data source name, enter a descriptive name.
* For Athena workgroup, choose your workgroup.
* Choose Validate connection to test the connection.
* Choose Create data source.
* (Optional) Select an IAM role ARN for queries to run as.

On the Choose your table screen, do the following:
a. For Catalog, choose one of the following:
   * If you are using Athena Federated Query, choose the catalog you want to use.
   * Otherwise, choose AwsDataCatalog.
     
b. Choose one of the following:
   * To write a SQL query, choose Use custom SQL.
   * To choose a database and table, choose your catalog that contains your databases from the dropdown under Catalog. Then, choose a database from the dropdown under Database and choose a table from the Tables list that appears for your database.

