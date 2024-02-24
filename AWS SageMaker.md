# AWS SageMaker
Amazon SageMaker is a fully managed machine learning (ML) service. With SageMaker, data scientists and developers can quickly and confidently build, train, and deploy ML models into a production-ready hosted environment. It provides a UI experience for running ML workflows that makes SageMaker ML tools available across multiple integrated development environments (IDEs).

With SageMaker, you can store and share your data without having to build and manage your own servers. This gives you or your organizations more time to collaboratively build and develop your ML workflow, and do it sooner. SageMaker provides managed ML algorithms to run efficiently against extremely large data in a distributed environment. With built-in support for bring-your-own-algorithms and frameworks, SageMaker offers flexible distributed training options that adjust to your specific workflows. Within a few steps, you can deploy a model into a secure and scalable environment from the SageMaker console.

The following diagram illustrates the typical workflow for creating a machine learning model. It includes three stages in a circular flow that we will cover in more detail below: generate example data, train a model, and deploy the model.

<img width="640" alt="Screenshot 2024-02-21 at 11 12 07 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/8a4214a9-7201-491f-bcf8-e4c702b696d7">

When you create a Domain, Amazon SageMaker automatically associates it with an Amazon Elastic File System (Amazon EFS) volume that SageMaker creates for you. You also have the option to associate the Domain with a custom Amazon EFS file system that you've created in your AWS account. This file system is available to any users who belong to the Domain when they use Amazon SageMaker Studio.

## Amazon SageMaker Studio
Amazon SageMaker Studio is the latest web-based experience for running ML workflows. Studio offers a suite of integrated development environments (IDEs). These include Code Editor, based on Code-OSS, Visual Studio Code - Open Source, a new JupyterLab application, RStudio, and Amazon SageMaker Studio Classic.

The new web-based UI in Studio is faster and provides access to all SageMaker resources, including jobs and endpoints, in one interface. ML practitioners can also choose their preferred IDE to accelerate ML development. A data scientist can use JupyterLab to explore data and tune models. In addition, a machine learning operations (MLOps) engineer can use Code Editor with the pipelines tool in Studio to deploy and monitor models in production.

### Launch from the Amazon SageMaker console
1. Open the Amazon [SageMaker console](https://console.aws.amazon.com/sagemaker/).
2. From the left navigation pane, choose Studio.
3. From the Studio landing page, select the domain and user profile for launching Studio.
4. Choose Open Studio.
5. To launch Studio, choose Launch personal Studio.

<img width="664" alt="Screenshot 2024-02-21 at 11 51 21 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/d1a5ceb4-ccc6-493e-96c3-31d4d1648c2a">

## Amazon SageMaker Studio Classic
Amazon SageMaker Studio Classic is a web-based, integrated development environment (IDE) for machine learning that lets you build, train, debug, deploy, and monitor your machine learning models.

* Write and execute code in Jupyter notebooks.
* Prepare data for machine learning.
* Build and train machine learning models.
* Deploy the models and monitor the performance of their predictions.
* Track and debug the machine learning experiments.

Studio Classic includes the following features:

### SageMaker Autopilot
Autopilot performs the following key tasks that you can use on autopilot or with various degrees of human guidance:

* Data analysis and preprocessing: Autopilot identifies your specific problem type, handles missing values, normalizes your data, selects features, and overall prepares the data for model training.
* Model selection: Autopilot explores a variety of algorithms and uses a cross-validation resampling technique to generate metrics that evaluate the predictive quality of the algorithms based on predefined objective metrics.
* Hyperparameter optimization: Autopilot automates the search for optimal hyperparameter configurations.
* Model training and evaluation: Autopilot automates the process of training and evaluating various model candidates. It splits the data into training and validation sets, trains the selected model candidates using the training data, and evaluates their performance on the unseen data of the validation set. Lastly, it ranks the optimized model candidates based on their performance and identifies the best performing model.
* Model deployment: Once Autopilot has identified the best performing model, it provides the option to deploy the model automatically by generating the model artifacts and the endpoint exposing an API. External applications can send data to the endpoint and receive the corresponding predictions or inferences.

<img width="725" alt="Screenshot 2024-02-21 at 12 10 33 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/b8d062f4-a11d-40a6-b8f2-c5735ad047fb">

Autopilot currently supports the following problem types:

* Regression, binary, and multiclass classification with tabular data formatted as CSV or Parquet files in which each column contains a feature with a specific data type and each row contains an observation. The column data types accepted include numerical, categorical, text, and time series that consists of strings of comma-separated numbers.
* Text classification with data formatted as CSV or Parquet files in which a column provides the sentences to be classified, while another column should provide the corresponding class label.
* Image classification with image formats such as PNG, JPEG, or a combination of both.
* Time-series forecasting with time-series data formatted as CSV or Parquet files.
* Fine-tuning of large language models (LLMs) for text generation with data formatted as CSV or Parquet files.

Autopilot supports tabular data formatted as CSV files or as Parquet files.

##### To create an Autopilot experiment using Studio Classic UI
1. Sign in at [SageMaker](https://console.aws.amazon.com/sagemaker/), choose Studio from the left navigation pane, select your Domain and user profile, then Open Studio.
2. In Studio, choose the Studio Classic icon in the top left navigation pane. This opens a Studio Classic app.
3. Run or open a Studio Classic application from the space of your choice, or Create Studio Classic space. . On the Home tab, choose the AutoML card. This opens a new AutoML tab.
4. Choose Create an AutoML experiment. This opens a new Create experiment tab.
5. In the Experiment and data details section, enter the following information:
   * Experiment name – Must be unique to your account in the current AWS Region and contain a maximum of 63 alphanumeric characters. Can include hyphens (-) but not spaces.
   * Input data – Provide the Amazon Simple Storage Service (Amazon S3) bucket location of your input data. This S3 bucket must be in your current AWS Region. The URL must be in an s3:// format where Amazon SageMaker has write permissions. The file must be in CSV or Parquet format and contain at least 500 rows. Select Browse to scroll through available paths and Preview to see a sample of your input data.
   * Is your S3 input a manifest file? – A manifest file includes metadata with your input data. The metadata specifies the location of your data in Amazon S3. It also specifies how the data is formatted and which attributes from the dataset to use when training your model. You can use a manifest file as an alternative to preprocessing when your labeled data is being streamed in Pipe mode.
   * Auto split data? – Autopilot can split your data into an 80-20% split for training and validation data. If you prefer a custom split, you can choose the Specify split ratio. To use a custom dataset for validation, choose Provide a validation set.
   * Output data location (S3 bucket) – The name of the S3 bucket location where you want to store the output data. The URL for this bucket must be in an Amazon S3 format where Amazon SageMaker has write permissions. The S3 bucket must be in the current AWS Region. Autopilot can also create this for you in the same location as your input data.
6. Choose Next: Target and features. The Target and features tab opens.
7. In the Target and features section:
   * Select a column to set as a target for model predictions.
   * Optionally, you can pass the name of a sample weights column in the Sample weight section to request your dataset rows to be weighted during training and evaluation. For more information on the available objective metrics such as Cross-Validation, K-fold, Ensembling mode and so on.
   * You can also select features for training and change their data type. The following data types are available: Text, Numerical, Categorical, Datetime, Sequence, and Auto. All features are selected by default.
8. Choose Next: Training method. The Training method tab opens.
9. In the Training method section, select your training option: Ensembling, Hyperparameter optimization (HPO), or Auto to let Autopilot choose the training method automatically based on the dataset size. Each training mode runs a pre-defined set of algorithms on your dataset to train model candidates. By default, Autopilot pre-selects all the available algorithms for the given training mode. You can run an Autopilot training experiment with all the algorithms or choose your own subset.
10. Choose Next: Deployment and advanced settings to open the Deployment and advanced settings tab. Settings include the auto-display endpoint name, machine learning problem type, and additional choices for running your experiment.
    * Deployment settings – Autopilot can automatically create an endpoint and deploy your model for you.
      To auto-deploy to an automatically generated endpoint, or to provide an endpoint name for custom deployment, set the toggle to Yes under Auto deploy? If you are importing data from Amazon SageMaker Data Wrangler, you have additional options to auto-deploy the best model with or without the transforms from Data Wrangler.
    * Advanced settings (optional) – Autopilot provides additional controls to manually set experimental parameters such as defining your problem type, time constraints on your Autopilot job and trials, security, and encryption settings.
      1. Machine learning problem type – Autopilot can automatically infer the type of supervised learning problem from your dataset. If you prefer to choose it manually, you can use the Select the machine learning problem type dropdown menu. Note that it defaults to Auto. In some cases, SageMaker is unable to infer accurately. When that happens, you must provide the value for the job to succeed. In particular, you can choose from the following types as Binary, Regression, or Multiclass.
      2. Runtime – You can define a maximum time limit. Upon reaching the time limit, trials and jobs that exceed the time constraint automatically stop.
      3. Access – You can choose the role that Amazon SageMaker Studio Classic assumes to gain temporary access to AWS services (in particular, SageMaker and Amazon S3) on your behalf. If no role is explicitly defined, Studio Classic automatically uses the default SageMaker execution role attached to your user profile.
      4. Encryption – To enhance the security of your data at rest and protect it against unauthorized access, you can specify encryption keys to encrypt data in your Amazon S3 buckets and in the Amazon Elastic Block Store (Amazon EBS) volume attached to your Studio Classic Domain.
      5. Project – Specify the name of the SageMaker project to associate with this Autopilot experiment and model outputs. When you specify a project, Autopilot tags the project to an experiment. This lets you know which model outputs are associated with this project.
      6. Tags – Tags are an array of key-value pairs. Use tags to categorize your resources from AWS services, such as their purpose, owner, or environment.
11. Select Create experiment.The creation of the experiment starts an Autopilot job in SageMaker. Autopilot provides the status of the experiment, information on the data exploration process and model candidates in notebooks, a list of generated models and their reports, and the job profile used to create them.

Please find the [SageMaker Autopilot](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/sam-app/lambda-python3.11) demo here.

### SageMaker JumpStart
SageMaker JumpStart provides pretrained, open-source models for a wide range of problem types to help you get started with machine learning. You can incrementally train and tune these models before deployment. JumpStart also provides solution templates that set up infrastructure for common use cases, and executable example notebooks for machine learning with SageMaker.

You can also access pretrained models, solution templates, and examples through the JumpStart landing page in Amazon SageMaker Studio Classic.

<img width="573" alt="Screenshot 2024-02-21 at 6 05 03 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/9fa859d2-3f05-4b70-bd7b-e01d4ccd86c7">

From the SageMaker JumpStart landing page in Studio, you can explore model hubs from providers of both proprietary and publicly available models.

<img width="574" alt="Screenshot 2024-02-21 at 6 06 11 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/73775543-cab1-44a1-85de-5f954e25b4fa">

If you have already onboarded to a SageMaker domain, you can update your domain to generate the default roles using the following procedure.
1. Open the Amazon [SageMaker console](https://console.aws.amazon.com/sagemaker/).
2. Choose Control Panel at the top left of the page.
3. From the Domain page, choose the Settings icon to edit the domain settings.
4. On General Settings choose Next.
5. Under SageMaker Projects and JumpStart, select Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for this account and Enable Amazon SageMaker project templates and Amazon SageMaker JumpStart for Studio Classic users, choose Next.
6. Select Submit.

Amazon SageMaker JumpStart provides access to hundreds of publicly available and proprietary foundation models from third-party sources and partners. You can explore the JumpStart foundation model selection directly in the SageMaker console, Studio, or Studio Classic.

For fine-tuning models that require the acceptance of an end-user license agreement, you must explicitly declare EULA acceptance when defining your JumpStart estimator. Note that after fine-tuning a pretrained model, the weights of the original model are changed, so you do not need to later accept a EULA when deploying the fine-tuned model.

#### Customize a foundation model
The recommended way to first customize a foundation model to a specific use case is through prompt engineering. Providing your foundation model with well-engineered, context-rich prompts can help achieve desired results without any fine-tuning or changing of model weights. For more information.

#### Prompt engineering for foundation models
Prompt engineering is the process of designing and refining the prompts or input stimuli for a language model to generate specific types of output. Prompt engineering involves selecting appropriate keywords, providing context, and shaping the input in a way that encourages the model to produce the desired response and is a vital technique to actively shape the behavior and output of foundation models.

##### Zero-shot learning
Zero-shot learning involves training a model to generalize and make predictions on unseen classes or tasks. To perform prompt engineering in zero-shot learning environments, we recommend constructing prompts that explicitly provide information about the target task and the desired output format. For example, if you want to use a foundation model for zero-shot text classification on a set of classes that the model did not see during training, a well-engineered prompt could be: "Classify the following text as either sports, politics, or entertainment: (input text)." By explicitly specifying the target classes and the expected output format, you can guide the model to make accurate predictions even on unseen classes.

##### Few-shot learning
Few-shot learning involves training a model with a limited amount of data for new classes or tasks. Prompt engineering in few-shot learning environments focuses on designing prompts that effectively use the limited available training data. For example, if you use a foundation model for an image classification task and only have a few examples of a new image class, you can engineer a prompt that includes the available labeled examples with a placeholder for the target class. For example, the prompt could be: "[image 1], [image 2], and [image 3] are examples of (target class). Classify the following image as (target class)".

#### Fine-tune a foundation model
Foundation models are computationally expensive and trained on a large, unlabeled corpus. Fine-tuning a pre-trained foundation model is an affordable way to take advantage of their broad capabilities while customizing a model on your own small, corpus. Fine-tuning is a customization method that involved further training and does change the weights of your model.

#### Evaluate a text generation foundation model in Studio
Amazon SageMaker JumpStart has integrations with SageMaker Clarify Foundation Model Evaluations (FMEval) in Studio. If a JumpStart model has built-in evaluation capabilities available, you can choose Evaluate in the upper right corner of the model detail page in the JumpStart Studio UI. For more information on navigating the JumpStart Studio UI.

Use Amazon SageMaker JumpStart to evaluate text-based foundation models with FMEval. You can use these model evaluations to compare model quality and responsibility metrics for one model, between two models, or between different versions of the same model, to help you quantify model risks. FMEval can evaluate text-based models that perform the following tasks:

### Prepare ML Data with Amazon SageMaker Data Wrangler
You can use Amazon SageMaker Data Wrangler to import, prepare, transform, visualize and analyze data. You can integrate Data Wrangler into your machine learning workflows to simplify and streamline data pre-processing and feature engineering using little to no coding. You can also add your own Python scripts and transformations to customize your data prep workflow.

Import data from Amazon S3, Amazon Redshift, Amazon Athena, and use Data Wrangler to create sophisticated machine learning data prep workflows with built-in and custom data transformations and analysis including feature target leakage and quick modeling.

After you have defined a data prep workflow, or data flow, you can integrate it with SageMaker Processing, SageMaker Pipelines, and SageMaker Feature Store, simplify the task of processing, sharing and storing ML training data. You can also export your data flow to a python script and create a custom ML data prep pipeline.

To access Data Wrangler in Studio Classic, do the following.
1. Sign in to Studio Classic and Choose Studio.
2. Choose Launch app.
3. From the dropdown list, select Studio.
4. Choose the Home icon.
5. Choose Data.
6. Choose Data Wrangler.
7. You can also create a Data Wrangler flow by doing the following.
   * In the top navigation bar, select File.
   * Select New.
   * Select Data Wrangler Flow.
   <img width="501" alt="Screenshot 2024-02-22 at 8 03 26 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/bb763aaa-a27c-4c4b-af33-b518a9532907">

8. (Optional) Rename the new directory and the .flow file.
9. When you create a new .flow file in Studio Classic, you might see a carousel that introduces you to Data Wrangler.
10. This messaging persists as long as the KernelGateway app on your User Details page is Pending. To see the status of this app, in the SageMaker console on the Amazon SageMaker Studio Classic page, select the name of the user you are using to access Studio Classic. On the User Details page, you see a KernelGateway app under Apps. Wait until this app status is Ready to start using Data Wrangler. This can take around 5 minutes the first time you launch Data Wrangler.

<img width="503" alt="Screenshot 2024-02-22 at 8 06 09 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/f5c5d6fa-f145-41ad-b81a-c1ed448731e9">

11. To get started, choose a data source and use it to import a dataset. When you import a dataset, it appears in your data flow.
12. After you import a dataset, Data Wrangler automatically infers the type of data in each column. Choose + next to the Data types step and select Edit data types.
13. Use the data flow to add transforms and analyses.
14. To export a complete data flow, choose Export and choose an export option.
15. Finally, choose the Components and registries icon, and select Data Wrangler from the dropdown list to see all the .flow files that you've created. You can use this menu to find and move between data flows.

#### Import
You can use Amazon SageMaker Data Wrangler to import data from the following data sources: Amazon Simple Storage Service (Amazon S3), Amazon Athena, Amazon Redshift, and Snowflake. The dataset that you import can include up to 1000 columns.

#### Create and Use a Data Wrangler Flow
Use an Amazon SageMaker Data Wrangler flow, or a data flow, to create and modify a data preparation pipeline. The data flow connects the datasets, transformations, and analyses, or steps, you create and can be used to define your pipeline.

When you create a Data Wrangler flow in Amazon SageMaker Studio Classic, Data Wrangler uses an Amazon EC2 instance to run the analyses and transformations in your flow. By default, Data Wrangler uses the m5.4xlarge instance. m5 instances are general purpose instances that provide a balance between compute and memory. You can use m5 instances for a variety of compute workloads.

Data Wrangler also gives you the option of using r5 instances. r5 instances are designed to deliver fast performance that processes large datasets in memory.

##### The Data Flow UI
When you import a dataset, the original dataset appears on the data flow and is named Source. Each time you add a transform step, you create a new dataframe. When multiple transform steps (other than Join or Concatenate) are added to the same dataset, they are stacked.

<img width="594" alt="Screenshot 2024-02-22 at 8 21 21 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/c732e506-cfe5-4d9d-9517-9229bfe30d60">

##### Add a Step to Your Data Flow
Select + next to any dataset or previously added step and then select one of the following options:

##### Delete a Step from Your Data Flow
Choose the group of steps that has the step that you're deleting. Choose the icon next to the step. Choose Delete step.

<img width="595" alt="Screenshot 2024-02-22 at 8 26 27 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/b3352f2c-6a36-4fc0-89c0-6766f03cd821">

##### The following image shows an example of editing a step.

<img width="667" alt="Screenshot 2024-02-22 at 8 27 40 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/f4aa41c8-eefc-4e3f-9b17-7710211a2e94">

#### Get Insights On Data and Data Quality
Use the Data Quality and Insights Report to perform an analysis of the data that you've imported into Data Wrangler. We recommend that you create the report after you import your dataset. You can use the report to help you clean and process your data. It gives you information such as the number of missing values and the number of outliers. If you have issues with your data, such as target leakage or imbalance, the insights report can bring those issues to your attention.

1. Choose a + next to a node in your Data Wrangler flow.
2. Select Get data insights.
3. For Analysis name, specify a name for the insights report.
4. (Optional) For Target column, specify the target column.
5. For Problem type, specify Regression or Classification.
6. For Data size, specify one of the following:
   * 50 K – Uses the first 50000 rows of the dataset that you've imported to create the report.
   * Entire dataset – Uses the entire dataset that you've imported to create the report.
7. Choose Create.

Data Quality report will give insight on the following topics:
* Summary
* Target column
* Quick model
* Feature summary
* Samples
* Definitions

#### Automatically Train Models on Your Data Flow
You can use Amazon SageMaker Autopilot to automatically train, tune, and deploy models on the data that you've transformed in your data flow. Amazon SageMaker Autopilot can go through several algorithms and use the one that works best with your data. For more information about Amazon SageMaker Autopilot, see SageMaker Autopilot.

You can prepare and deploy a model by choosing a node in your Data Wrangler flow and choosing Export and Train in the data preview. You can use this method to view your dataset before you choose to train a model on it.

1. Choose the + next to the node containing the training data.
2. Choose Train model.
3. (Optional) Specify a AWS KMS key or ID. For more information about creating and controlling cryptographic keys to protect your data.
4. Choose Export and train.
5. After Amazon SageMaker Autopilot trains the model on the data that Data Wrangler exported, specify a name for Experiment name.
6. Under Input data, choose Preview to verify that Data Wrangler properly exported your data to Amazon SageMaker Autopilot.
7. For Target, choose the target column.
8. (Optional) For S3 location under Output data, specify an Amazon S3 location other than the default location.
9. Choose Next: Training method.
10. (Optional) For Auto deploy endpoint, specify a name for the endpoint.
11. For Deployment option, choose a deployment method. You can choose to deploy with or without the transformations that you've made to your data.
12. Choose Next: Review and create.
13. Choose Create experiment.

#### Transform Data
Amazon SageMaker Data Wrangler provides numerous ML data transforms to streamline cleaning, transforming, and featurizing your data. When you add a transform, it adds a step to the data flow. Each transform you add modifies your dataset and produces a new dataframe. All subsequent transforms apply to the resulting dataframe.

1. Choose the + next to the step in the data flow.
2. Choose Add transform.
3. Choose Add step.
4. Choose a transform.
5. (Optional) You can search for the transform that you want to use. Data Wrangler highlights the query in the results.

<img width="683" alt="Screenshot 2024-02-22 at 8 39 25 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/b2bcc432-3d84-43ef-bf1b-53e15b398989">

#### Analyze and Visualize
Amazon SageMaker Data Wrangler includes built-in analyses that help you generate visualizations and data analyses in a few clicks. You can also create custom analyses using your own code.

You add an analysis to a dataframe by selecting a step in your data flow, and then choosing Add analysis. To access an analysis you've created, select the step that contains the analysis, and select the analysis.

#### Reusing Data Flows for Different Datasets
For Amazon Simple Storage Service (Amazon S3) data sources, you can create and use parameters. A parameter is a variable that you've saved in your Data Wrangler flow. Its value can be any portion of the data source's Amazon S3 path. Use parameters to quickly change the data that you're importing into a Data Wrangler flow or exporting to a processing job. You can also use parameters to select and import a specific subset of your data.

#### Export
You can export your data to S3 bucket, Python Code, SageMaker pipeline, and Feature Store.

<img width="596" alt="Screenshot 2024-02-22 at 8 49 21 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/e6547bff-71e9-42b9-8d1c-4eabf68a14de">

### Scale data preparation using Apache Spark, Hive, or Presto on Amazon EMR or AWS Glue from Amazon SageMaker Studio Classic notebooks
Amazon SageMaker Studio Classic provides data scientists, machine learning (ML) engineers, and general practitioners with tools to perform data analytics and data preparation at scale. Analyzing, transforming, and preparing large amounts of data is a foundational step of any data science and ML workflow. SageMaker Studio Classic comes with built-in integration of Amazon EMR and AWS Glue Interactive Sessions to handle your large-scale interactive data preparation and machine learning workflows, all within your Studio Classic notebook.

### Process data
To analyze data and evaluate machine learning models on Amazon SageMaker, use Amazon SageMaker Processing. With Processing, you can use a simplified, managed experience on SageMaker to run your data processing workloads, such as feature engineering, data validation, model evaluation, and model interpretation. You can also use the Amazon SageMaker Processing APIs during the experimentation phase and after the code is deployed in production to evaluate performance.

<img width="670" alt="Screenshot 2024-02-22 at 11 32 08 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/38d9ebdd-3b61-42f4-a041-8c5883bfaea6">

The preceding diagram shows how Amazon SageMaker spins up a Processing job. Amazon SageMaker takes your script, copies your data from Amazon Simple Storage Service (Amazon S3), and then pulls a processing container. The processing container image can either be an Amazon SageMaker built-in image or a custom image that you provide. The underlying infrastructure for a Processing job is fully managed by Amazon SageMaker. Cluster resources are provisioned for the duration of your job, and cleaned up when a job completes. The output of the Processing job is stored in the Amazon S3 bucket you specified.

#### Data Processing with Apache Spark
Apache Spark is a unified analytics engine for large-scale data processing. Amazon SageMaker provides prebuilt Docker images that include Apache Spark and other dependencies needed to run distributed data processing jobs.

You can use the sagemaker.spark.PySparkProcessor or sagemaker.spark.SparkJarProcessor class to run your Spark application inside of a processing job. Note you can set MaxRuntimeInSeconds to a maximum runtime limit of 5 days. With respect to execution time, and number of instances used, simple spark workloads see a near linear relationship between the number of instances vs. time to completion.

The following code example shows how to run a processing job that invokes your PySpark script preprocess.py.

```
from sagemaker.spark.processing import PySparkProcessor

spark_processor = PySparkProcessor(
    base_job_name="spark-preprocessor",
    framework_version="2.4",
    role=role,
    instance_count=2,
    instance_type="ml.m5.xlarge",
    max_runtime_in_seconds=1200,
)

spark_processor.run(
    submit_app="preprocess.py",
    arguments=['s3_input_bucket', bucket,
               's3_input_key_prefix', input_prefix,
               's3_output_bucket', bucket,
               's3_output_key_prefix', output_prefix]
)
```
The following code example shows how the notebook uses SKLearnProcessor to run your own scikit-learn script using a Docker image provided and maintained by SageMaker, instead of your own Docker image.

```
from sagemaker.sklearn.processing import SKLearnProcessor
from sagemaker.processing import ProcessingInput, ProcessingOutput

sklearn_processor = SKLearnProcessor(framework_version='0.20.0',
                                     role=role,
                                     instance_type='ml.m5.xlarge',
                                     instance_count=1)

sklearn_processor.run(code='preprocessing.py',
                      inputs=[ProcessingInput(
                        source='s3://path/to/my/input-data.csv',
                        destination='/opt/ml/processing/input')],
                      outputs=[ProcessingOutput(source='/opt/ml/processing/output/train'),
                               ProcessingOutput(source='/opt/ml/processing/output/validation'),
                               ProcessingOutput(source='/opt/ml/processing/output/test')]
                     )

```

A FrameworkProcessor can run Processing jobs with a specified machine learning framework, providing you with an Amazon SageMaker–managed container for whichever machine learning framework you choose. FrameworkProcessor provides premade containers for the following machine learning frameworks: Hugging Face, MXNet, PyTorch, TensorFlow, and XGBoost.

#### Run Scripts with Your Own Processing Container
1. Create a Docker directory and add the Dockerfile used to create the processing container. Install pandas and scikit-learn into it. (You could also install your own dependencies with a similar RUN command.)
   ```
    mkdir docker

    %%writefile docker/Dockerfile
    
    FROM python:3.7-slim-buster
    
    RUN pip3 install pandas==0.25.3 scikit-learn==0.21.3
    ENV PYTHONUNBUFFERED=TRUE
    
    ENTRYPOINT ["python3"]
   ```
2. Build the container using the docker command, create an Amazon Elastic Container Registry (Amazon ECR) repository, and push the image to Amazon ECR.
   ```
    import boto3

    account_id = boto3.client('sts').get_caller_identity().get('Account')
    region = boto3.Session().region_name
    ecr_repository = 'sagemaker-processing-container'
    tag = ':latest'
    processing_repository_uri = '{}.dkr.ecr.{}.amazonaws.com/{}'.format(account_id, region, ecr_repository + tag)
    
    # Create ECR repository and push docker image
    !docker build -t $ecr_repository docker
    !aws ecr get-login-password --region {region} | docker login --username AWS --password-stdin {account_id}.dkr.ecr.{region}.amazonaws.com
    !aws ecr create-repository --repository-name $ecr_repository
    !docker tag {ecr_repository + tag} $processing_repository_uri
    !docker push $processing_repository_uri
   ```
3. Set up the ScriptProcessor from the SageMaker Python SDK to run the script. Replace image_uri with the URI for the image you created, and replace role_arn with the ARN for an AWS Identity and Access Management role that has access to your target Amazon S3 bucket.
   ```
    from sagemaker.processing import ScriptProcessor, ProcessingInput, ProcessingOutput

    script_processor = ScriptProcessor(command=['python3'],
                    image_uri='image_uri',
                    role='role_arn',
                    instance_count=1,
                    instance_type='ml.m5.xlarge')
   ```
4. Run the script. Replace preprocessing.py with the name of your own Python processing script, and replace s3://path/to/my/input-data.csv with the Amazon S3 path to your input data.
   ```
   script_processor.run(code='preprocessing.py',
                     inputs=[ProcessingInput(
                        source='s3://path/to/my/input-data.csv',
                        destination='/opt/ml/processing/input')],
                     outputs=[ProcessingOutput(source='/opt/ml/processing/output/train'),
                               ProcessingOutput(source='/opt/ml/processing/output/validation'),
                               ProcessingOutput(source='/opt/ml/processing/output/test')])
   ```

### Create, store, and share features with Amazon SageMaker Feature Store
The machine learning (ML) development process often begins with extracting data signals also known as features from data to train ML models. Amazon SageMaker Feature Store makes it easy for data scientists, machine learning engineers, and general practitioners to create, share, and manage features for ML development. Feature Store accelerates this process by reducing repetitive data processing and curation work required to convert raw data into features for training an ML algorithm.

Further, the processing logic for your data is authored only once, and features generated are used for both training and inference, reducing the training-serving skew. Feature Store is a centralized store for features and associated metadata so features can be easily discovered and reused. You can create an online or an offline store. The online store is used for low latency real-time inference use cases, and the offline store is used for training and batch inference.  

<img width="662" alt="Screenshot 2024-02-22 at 12 13 25 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/f86e4d10-3a66-457f-861d-8174efce842e">

#### How Feature Store works
In Feature Store, features are stored in a collection called a feature group. You can visualize a feature group as a table in which each column is a feature, with a unique identifier for each row. In principle, a feature group is composed of features and values specific to each feature. A Record is a collection of values for features that correspond to a unique RecordIdentifier. Altogether, a FeatureGroup is a group of features defined in your FeatureStore to describe a Record. 

* Online – In online mode, features are read with low latency (milliseconds) reads and used for high throughput predictions. This mode requires a feature group to be stored in an online store.
* Offline – In offline mode, large streams of data are fed to an offline store, which can be used for training and batch inference. This mode requires a feature group to be stored in an offline store. The offline store uses your S3 bucket for storage and can also fetch data using Athena queries.
* Online and Offline – This includes both online and offline modes.

The following example diagram conceptualizes a few Feature Store concepts:
<img width="631" alt="Screenshot 2024-02-22 at 12 16 54 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/806f955f-9f38-435e-afbe-2262afa646a1">

#### Stream ingestion
You can use streaming sources such as Kafka or Kinesis as a data source, where records are extracted from, and directly feed records to the online store for training, inference or feature creation. Records can be ingested into your feature group by using the synchronous PutRecord API call.

#### Data Wrangler with Feature Store
Data Wrangler is a feature of Studio Classic that provides an end-to-end solution to import, prepare, transform, featurize, and analyze data. Data Wrangler enables you to engineer your features and ingest them into your online or offline store feature groups.

#### Time to live (TTL) duration for records
Amazon SageMaker Feature Store provides the option for records to be hard deleted from the online store after a time duration is reached, with time to live (TTL) duration (TtlDuration). The record will expire after the record’s EventTime plus the TtlDuration is reached, or ExpiresAt = EventTime + TtlDuration. The TtlDuration can be applied at a feature group level, where all records within the feature group will have the TtlDuration by default, or at an individual record level. If TtlDuration is unspecified, the default value is null and the record will remain in the online store until it is overwritten.

For offline feature store it preserves the history of each timestamp. It cannot be overwritten. Amazon SageMaker Feature Store supports the AWS Glue and Apache Iceberg table formats for the offline store. You can choose the table format when you’re creating a new feature group. AWS Glue is the default format.

1. Open the Studio Classic console by following the instructions in Launch Amazon SageMaker Studio Classic.
2. Choose the Home icon on the left navigation pane.
3. Choose Data.
4. From the dropdown list, choose Feature Store.
5. Choose Create feature group.
6. Under Feature group details, enter a feature group name.
7. (Optional) Enter a description of the feature group.
8. Under Feature group storage configuration, choose a storage configuration from the dropdown list. For information about storage configurations.
9. If you have chosen to enable the online storage:
   * If you only enable the online storage, you may choose a Storage type from the dropdown list.
   * (Optional) Apply Time to Live (TTL) by toggling the switch to On and specifying the Time to Live duration value and unit. This will update the default TTL duration for all records added to the feature group after the feature group is created.
10. If you have chosen to enable the offline storage:
    * Under the Amazon S3 bucket name, enter a new bucket name or enter an existing bucket URL manually.
    * From the Table format dropdown list, choose the table format. In most use cases, you should use the Apache Iceberg table format.
    * Under IAM role ARN, choose the IAM role ARN you want to attach to this feature group.
    * If you have chosen to enable the offline storage Table format and AWS Glue (default) Table format, under Data catalog Use default values for your AWS Glue Data Catalog or provide your existing Data Catalog name, table name, and database name to extend your existing AWS Glue Data Catalog.
11. Under the Online store encryption key or Offline store encryption key dropdown list.
12. After you specify all of the required information, the Continue button appears available. Choose Continue.
13. Under Specify feature definitions, you have two options for providing a schema for your features: a JSON editor, or a table editor.
    * JSON editor: In the JSON tab, enter or copy and paste your feature definitions in the JSON format.
    * Table editor: In the Table tab, enter the feature feature name and choose the corresponding data type for each feature in your feature group. Choose + Add feature definitions to include more features. Be aware that you cannot remove feature definitions from your feature groups. However, you can add and update feature definitions after the feature group is created.
14. After all of the features are included, choose Continue.
15. Under Select required features, you must specify the record identifier and event time features. Do this by choosing the feature name under Record identifier feature name and Event time feature name dropdown lists, respectively.
16. After you choose the record identifier and event time features, choose Continue.
17. (Optional) To add tags for the feature group, choose Add new tag. Then enter a tag key and the corresponding value under Key and Value, respectively.
18. Choose Continue.
19. Under Review feature group, review the feature group information. To edit any step, choose the Edit button that corresponds to that step. This brings you to the corresponding step for editing. To return to step 5, choose Continue until you return to step 5.
20. After you finalize the setup for your feature group, choose Create feature group.
    If an issue occurs during setup, a pop-up alert message appears at the bottom of the page with tips for solving the issue. You can return to previous steps to fix the issues by choosing Edit for the step with conflicts.

On the Features tab, you can find a list of all of the features. Use the filter to refine your list. Choose a feature to view its details.

### SageMaker Ground Truth
To train a machine learning model, you need a large, high-quality, labeled dataset. You can label your data using Amazon SageMaker Ground Truth. It can also be utilized to generate labels for the production model and assist in determining the baseline.

To train a machine learning model, you need a large, high-quality, labeled dataset. Ground Truth helps you build high-quality training datasets for your machine learning models. With Ground Truth, you can use workers from either Amazon Mechanical Turk, a vendor company that you choose, or an internal, private workforce along with machine learning to enable you to create a labeled dataset. You can use the labeled dataset output from Ground Truth to train your own models. You can also use the output as a training dataset for an Amazon SageMaker model.

1. Create an Amazon S3 bucket to hold the input and output files. The bucket must be in the same Region where you are running Ground Truth.
2. From the left navigation, choose Labeling jobs.
3. Choose Create labeling job to start the job creation process.
4. In the Job overview section, provide the following information:
   * Job name – Give the labeling job a name that describes the job. This name is shown in your job list. The name must be unique in your account in an AWS Region.
   * Label attribute name – Leave this unchecked as the default value is the best option for this introductory job.
   * Input data setup – Select Automated data setup. This option allows you to automatically connect to your input data in S3.
   * S3 location for input datasets – Enter the S3 location where you added the images in step 1.
   * S3 location for output datasets – The location where your output data is written in S3.
   * Data type – Use the drop down menu to select Image. Ground Truth will use all images found in the S3 location for input datasets as input for your labeling job.
   * IAM role – Create or choose an IAM role with the AmazonSageMakerFullAccess IAM policy attached.
5. In the Task type section, for the Task category field, choose Image.
6. In the Task selection choose Bounding box.
7. Choose Next to move on to configuring your labeling job.
8. Select Workers
9. In the Workers section, choose Private.
10. If this is your first time using a private workforce, in the Email addresses field, enter up to 100 email addresses. The addresses must be separated by a comma. You should include your own email address so that you are part of the workforce and can see data object labeling tasks.
11. In the Organization name field, enter the name of your organization. This information is used to customize the email sent to invite a person to your private workforce. You can change the organization name after the user pool is created through the console.
12. In the Contact email field enter an email address that members of the workforce use to report problems with the task.
13. Configure the Bounding Box Tool.
14. In the Task description field type in brief instructions for the task. For example: Draw a box around any objects in the image.
15. In the Labels field, type a category name for the objects that the worker should draw a bounding box around. For example, if you are asking the worker to draw boxes around football players, you could use "Football Player" in this field.
16. The Short instructions section enables you to create instructions that are displayed on the page with the image that your workers are labeling. We suggest that you include an example of a correctly drawn bounding box and an example of an incorrectly drawn box. To create your own instructions, use these steps:
    * Select the text between GOOD EXAMPLE and the image placeholder. Replace it with the following text: "Draw the box around the object with a small border."
    * Choose the image button and then enter the HTTPS URL of one of the images that you created in step 1. It is also possible to embed images directly in the short instructions section, however this section has a quota of 100 kilobytes (including text). If your images and text exceed 100 kilobytes, you receive an error.
    * Select the text between BAD EXAMPLE and the image placeholder. Replace it with the following text:"Don't make the bounding box too large or cut into the object."
    * Choose the image button and then enter the HTTPS URL of the other image that you created in step 1.
17. Select Preview to preview the worker UI. The preview opens in a new tab, and so if your browser blocks pop ups you may need to manually enable the tab to open. When you add one or more annotations to the preview and then select Submit you can see a preview of the output data your annotation would created.
18. After you have configured and verified your instructions, select Create to create the labeling job.

After you have configured and verified your instructions, select Create to create the labeling job.

### Automate Data Labeling
If you choose, Amazon SageMaker Ground Truth can use active learning to automate the labeling of your input data for certain built-in task types. Active learning is a machine learning technique that identifies data that should be labeled by your workers. In Ground Truth, this functionality is called automated data labeling. Automated data labeling helps to reduce the cost and time that it takes to label your dataset compared to using only humans. When you use automated labeling, you incur SageMaker training and inference costs.

#### How it Works
1. When Ground Truth starts an automated data labeling job, it selects a random sample of input data objects and sends them to human workers. If more than 10% of these data objects fail, the labeling job will fail. If the labeling job fails, in addition to reviewing any error message Ground Truth returns, check that your input data is displaying correctly in the worker UI, instructions are clear, and that you have given workers enough time to complete tasks.
2. When the labeled data is returned, it is used to create a training set and a validation set. Ground Truth uses these datasets to train and validate the model used for auto-labeling.
3. Ground Truth runs a batch transform job, using the validated model for inference on the validation data. Batch inference produces a confidence score and quality metric for each object in the validation data.
4. The auto labeling component will use these quality metrics and confidence scores to create a confidence score threshold that ensures quality labels.
5. Ground Truth runs a batch transform job on the unlabeled data in the dataset, using the same validated model for inference. This produces a confidence score for each object.
6. The Ground Truth auto labeling component determines if the confidence score produced in step 5 for each object meets the required threshold determined in step 4. If the confidence score meets the threshold, the expected quality of automatically labeling exceeds the requested level of accuracy and that object is considered auto-labeled.
7. Step 6 produces a dataset of unlabeled data with confidence scores. Ground Truth selects data points with low confidence scores from this dataset and sends them to human workers.
8. Ground Truth uses the existing human-labeled data and this additional labeled data from human workers to update the model.
9. The process is repeated until the dataset is fully labeled or until another stopping condition is met. For example, auto-labeling stops if your human annotation budget is reached.

<img width="608" alt="Screenshot 2024-02-22 at 6 13 49 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/9df37ae0-756d-47b6-b198-8d4b6da5d660">

##### To create an automated data labeling job (console)
1. Open the Ground Truth Labeling jobs section of the [SageMaker console](https://console.aws.amazon.com/sagemaker/groundtruth).
2. Using Create a Labeling Job (Console) as a guide, complete the Job overview and Task type sections. Note that auto labeling is not supported for custom task types.
3. Under Workers, choose your workforce type.
4. In the same section, choose Enable automated data labeling.
5. Using Step 4: Configure the Bounding Box Tool as a guide, create worker instructions in the section Task Type labeling tool. For example, if you chose Semantic segmentation as your labeling job type, this section is called Semantic segmentation labeling tool.
6. To preview your worker instructions and dashboard, choose Preview.
7. Choose Create. This creates and starts your labeling job and the auto labeling process.

## Train machine learning models
The training stage of the full machine learning (ML) lifecycle spans from accessing your training dataset to generating a final model and selecting the best performing model for deployment. The following sections provide an overview of available SageMaker training features and resources with in-depth technical information for each.

The following architecture diagram shows how SageMaker manages ML training jobs and provisions Amazon EC2 instances on behalf of SageMaker users. You as a SageMaker user can bring your own training dataset, saving it to Amazon S3. You can choose an ML model training from available SageMaker built-in algorithms, or bring your own training script with a model built with popular machine learning frameworks.

<img width="663" alt="Screenshot 2024-02-23 at 7 55 25 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/339677c5-8142-46e6-8a97-44b73087fefa">

### Full view of the SageMaker Training workflow and features
<img width="652" alt="Screenshot 2024-02-23 at 7 57 24 AM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/5a6daa5d-9ff3-4510-9c2c-e8b38acd54cd">

#### Before training
There are a number of scenarios of setting up data resources and access you need to consider before training. Refer to the following diagram and details of each before-training stage to get a sense of what decisions you need to make.

![training-before](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/32d5a67f-925b-4f88-89d2-79d1d4e56c47)

#### During training
During training, you need to continuously improve training stability, training speed, training efficiency while scaling compute resources, cost optimization, and, most importantly, model performance. Read on for more information about during-training stages and relevant SageMaker Training features.

![training-during](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/12b25c1e-782b-407b-abc5-8bb67c905459)

#### After Training
After training, you obtain a final model artifact to use for model deployment and inference. There are additional actions involved in the after-training phase as shown in the following diagram.

![training-after](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/8bc43d9f-fc35-4acb-81b4-cd31363159ed)

### Run your local code as a SageMaker training job
You can run your local machine learning (ML) Python code as a large single-node Amazon SageMaker training job or as multiple parallel jobs. You can do this by annotating your code with an @remote decorator, as shown in the following code example. Distributed training (across multiple instances) are not supported with remote functions.

```
@remote(**settings)
def divide(x, y):
    return x / y
```

#### Run Your Code from within SageMake Notebook or Studio UI
If you working on Studio then Select Base Python 3.0 from the down arrow next to Image in the Change environment dialog box.

The @remote decorator automatically detects the image attached to the SageMaker Studio Classic notebook and uses it to run the SageMaker training job. If image_uri is specified either as an argument in the decorator or in the configuration file, then the value specified in image_uri will be used instead of the detected image.

For SageMaker notebook follow the example code [here](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-Remote-Function).

## Deploy models for inference
With Amazon SageMaker, you can deploy your machine learning (ML) models to make predictions, also known as inference. SageMaker provides a broad selection of ML infrastructure and model deployment options to help meet all your ML inference needs. It is a fully managed service and integrates with MLOps tools, so you can scale your model deployment, reduce inference costs, manage models more effectively in production, and reduce operational burden.

![inference-workflow-flowchart](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/38b5a583-2744-48a1-9368-a57aa84bec5e)

You can perform these actions using the AWS console, the AWS SDKs, the SageMaker Python SDK, AWS CloudFormation or the AWS CLI.

### Create a model in Amazon SageMaker with ModelBuilder
Preparing your model for deployment on a SageMaker endpoint requires multiple steps, including choosing a model image, setting up the endpoint configuration, coding your serialization and deserialization functions to transfer data to and from server and client, identifying model dependencies, and uploading them to Amazon S3. ModelBuilder can reduce the complexity of initial setup and deployment to help you create a deployable model in a single step.

Please refer the example link of [ModelBuilder](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-Model-Builder).

### Real-time inference
Real-time inference is ideal for inference workloads where you have real-time, interactive, low latency requirements. You can deploy your model to SageMaker hosting services and get an endpoint that can be used for inference. These endpoints are fully managed and support autoscaling.

#### Create a deployable model
1. Open the SageMaker Studio application.
2. In the left navigation pane, choose Models.
3. Choose the Deployable models tab.
4. On the Deployable models page, choose Create.
5. On the Create deployable model page, for the Model name field, enter a name for the model.
The Container definition section looks like the following screenshot:
<img width="500" alt="studio-container-definition" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/b6aef138-3d39-4414-bfb3-f3898c2bc86f">

For the Container definition section, do the following:
1. For Container type, select Pre-built container if you'd like to use a SageMaker managed container, or select Bring your own container if you have your own container.
2. If you selected Pre-built container, select the Container framework, Framework version, and Hardware type that you'd like to use.
3. If you selected Bring your own container, enter an Amazon ECR path for ECR path to container image.

Then, fill out the Artifacts section, which looks like the following screenshot:
<img width="400" alt="studio-artifacts-section" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/78a928e0-9efe-40e3-8b6c-a33b6d2cc5dd">

1. If you're using one of the frameworks that SageMaker supports for packaging model artifacts (PyTorch or XGBoost), then for Artifacts, you can choose the Upload artifacts option. With this option, you can simply specify your raw model artifacts, any custom inference code you have, and your requirements.txt file, and SageMaker handles packaging the archive for you.
2. If you're not using a framework that SageMaker supports for packaging model artifacts, then Studio shows you the Pre-packaged artifacts option, and you must provide all of your artifacts already packaged as a tar.gz archive. Do the following:
   * For Pre-packaged artifacts, select Input S3 URI for pre-packaged model artifacts if you have your tar.gz archive already uploaded to Amazon S3. Select Upload pre-packaged model artifacts if you want to directly upload your archive to SageMaker.
   * If you selected Input S3 URI for pre-packaged model artifacts, enter the Amazon S3 path to your archive for S3 URI. Otherwise, select and upload the archive from your local machine.

The next section is Security, which looks like the following screenshot:
<img width="500" alt="studio-security-section" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/b2368642-2105-4b31-aaf9-35f7d2820e97">

1. For IAM role, enter the ARN for an IAM role.
2. (Optional) For Virtual Private Cloud (VPC), you can select an Amazon VPC for storing your model configuration and artifacts.
3. (Optional) Turn on the Network isolation toggle if you want to restrict your container's internet access.

Finally, you can optionally fill out the Advanced options section, which looks like the following screenshot:
<img width="500" alt="studio-advanced-options" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/c682fcd8-29bd-4967-bc32-f53a66f38486">

1. Turn on the Customized instance recommendations toggle if you want to run an Amazon SageMaker Inference Recommender job on your model after its creation. Inference Recommender is a feature that provides you with recommended instance types for optimizing inference performance and cost. You can view these instance recommendations when preparing to deploy your model.
2. For Add environment variables, enter an environment variables for your container as key-value pairs.
3. For Tags, enter any tags as key-value pairs.
4. After finishing your model and container configuration, choose Create deployable model.

Follow the [Real Time Inference](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-RealTime-Inference) Example.

#### Hosting Single Model
You can create, update, and delete real-time inference endpoints that host a single model with Amazon SageMaker Studio, the AWS SDK for Python (Boto3), the SageMaker Python SDK, or the AWS CLI. For procedures and code examples.

#### Host multiple models in one container behind one endpoint
Multi-model endpoints provide a scalable and cost-effective solution to deploying large numbers of models. They use the same fleet of resources and a shared serving container to host all of your models. This reduces hosting costs by improving endpoint utilization compared with using single-model endpoints. It also reduces deployment overhead because Amazon SageMaker manages loading models in memory and scaling them based on the traffic patterns to your endpoint.

Multi-model endpoints also enable time-sharing of memory resources across your models. This works best when the models are fairly similar in size and invocation latency. When this is the case, multi-model endpoints can effectively use instances across all models. If you have models that have significantly higher transactions per second (TPS) or latency requirements, we recommend hosting them on dedicated endpoints.

##### Create a multi-model endpoint (console)
1. Open the Amazon [SageMaker console](https://console.aws.amazon.com/sagemaker/).
2. Choose Model, and then from the Inference group, choose Create model.
3. For Model name, enter a name.
4. For IAM role, choose or create an IAM role that has the AmazonSageMakerFullAccess IAM policy attached.
5. In the Container definition section, for Provide model artifacts and inference image options, choose Use multiple models.
  ![mme-create-model-ux-2](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/73889aae-5f5d-4f2f-a012-0362eaa856f5)

6. For the Inference container image, enter the Amazon ECR path for your desired container image.
7. Choose Create model.
8. Deploy your multi-model endpoint as you would a single model endpoint.

#### Host multiple models which use different containers behind one endpoint
SageMaker multi-container endpoints enable customers to deploy multiple containers, that use different models or frameworks, on a single SageMaker endpoint. The containers can be run in a sequence as an inference pipeline, or each container can be accessed individually by using direct invocation to improve endpoint utilization and optimize costs.

1. Create container elements and InferenceExecutionConfig with direct invocation.
```
container1 = {
                 'Image': '123456789012.dkr.ecr.us-east-1.amazonaws.com/myimage1:mytag',
                 'ContainerHostname': 'firstContainer'
             }

container2 = {
                 'Image': '123456789012.dkr.ecr.us-east-1.amazonaws.com/myimage2:mytag',
                 'ContainerHostname': 'secondContainer'
             }
inferenceExecutionConfig = {'Mode': 'Direct'}
                
```
2. Create the model with the container elements and set the InferenceExecutionConfig field.
```
import boto3
sm_client = boto3.Session().client('sagemaker')

response = sm_client.create_model(
               ModelName = 'my-direct-mode-model-name',
               InferenceExecutionConfig = inferenceExecutionConfig,
               ExecutionRoleArn = role,
               Containers = [container1, container2]
           )
                
```
The following example directly invokes the secondContainer of a multi-container endpoint to get a prediction.
```
import boto3
runtime_sm_client = boto3.Session().client('sagemaker-runtime')

response = runtime_sm_client.invoke_endpoint(
   EndpointName ='my-endpoint',
   ContentType = 'text/csv',
   TargetContainerHostname='secondContainer', 
   Body = body)

```

#### Host models along with pre-processing logic as serial inference pipeline behind one endpoint
An inference pipeline is a Amazon SageMaker model that is composed of a linear sequence of two to fifteen containers that process requests for inferences on data. You use an inference pipeline to define and deploy any combination of pretrained SageMaker built-in algorithms and your own custom algorithms packaged in Docker containers. You can use an inference pipeline to combine preprocessing, predictions, and post-processing data science tasks. Inference pipelines are fully managed.

##### Create a Pipeline Model
1. Open the Amazon [SageMaker console](https://console.aws.amazon.com/sagemaker/).
2. Choose Models, and then choose Create models from the Inference group.
3. On the Create model page, provide a model name, choose an IAM role, and, if you want to use a private VPC, specify VPC values.
![create-pipeline-model](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/70c02c53-644b-48fd-9cc0-7dbc8a5de4e4)

4. To add information about the containers in the inference pipeline, choose Add container, then choose Next.
5. Complete the fields for each container in the order that you want to execute them, up to the maximum of fifteen. Complete the Container input options, , Location of inference code image, and, optionally, Location of model artifacts, Container host name, and Environmental variables fields. 

![create-pipeline-model-containers](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/64b72795-6c8b-4a8d-a422-a9ba81686f69)

The MyInferencePipelineModel page summarizes the settings for the containers that provide input for the model. If you provided the environment variables in a corresponding container definition, SageMaker shows them in the Environment variables field.

![pipeline-MyInferencePipelinesModel-recap](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/6a8f0596-8166-42fe-85df-80711306798d)

### Serverless Inference
Amazon SageMaker Serverless Inference is a purpose-built inference option that enables you to deploy and scale ML models without configuring or managing any of the underlying infrastructure. On-demand Serverless Inference is ideal for workloads which have idle periods between traffic spurts and can tolerate cold starts. Serverless endpoints automatically launch compute resources and scale them in and out depending on traffic, eliminating the need to choose instance types or manage scaling policies. 

1. Sign in to the Amazon SageMaker console.
2. In the navigation tab, choose Inference.
3. Choose Create model.
4. For Model name, enter a name for the model that is unique to your account and AWS Region.
5. For IAM role, either select an IAM role you have already created (see Prerequisites) or allow SageMaker to create one for you.
6. In Container definition 1, for Container input options, select Provide model artifacts and input location.
7. For Provide model artifacts and inference image options, select Use a single model.
8. For Location of inference code image, enter an Amazon ECR path to a container. The image must either be a SageMaker-provided first party image (e.g. TensorFlow, XGBoost) or an image that resides in an Amazon ECR repository within the same account in which you are creating the endpoint.
9. For Location of model artifacts, enter the Amazon S3 URI to your ML model. For example, s3://DOC-EXAMPLE-BUCKET/models/model.tar.gz.
10. (Optional) For Tags, add key-value pairs to create metadata for your model.
11. Choose Create model.

To create an endpoint configuration (using the console)
1. Sign in to the Amazon SageMaker console.
2. In the navigation tab, choose Inference.
3. Next, choose Endpoint configurations.
4. Choose Create endpoint configuration.
5. For Endpoint configuration name, enter a name that is unique within your account in a Region.
6. For Type of endpoint, select Serverless.
<img width="556" alt="serverless-endpoints-endpoint-config" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/2aa715f3-ad58-426d-80ad-4c7bdd037192">

7. For Production variants, choose Add model.
8. Under Add model, select the model you want to use from the list of models and then choose Save.
9. After adding your model, under Actions, choose Edit.
10. For Memory size, choose the memory size you want in GB.
![serverless-endpoints-endpoint-config-2](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/85efa002-1810-4ebd-9282-d8e98f5445a5)

11. For Max Concurrency, enter your desired maximum concurrent invocations for the endpoint. The maximum value you can enter is 200 and the minimum is 1.
12. (Optional) To use Provisioned Concurrency, enter the desired number of concurrent invocations in the Provisioned Concurrency setting field. The number of provisioned concurrent invocations must be less than or equal to the number of maximum concurrent invocations.
13. Choose Save.
14. (Optional) For Tags, enter key-value pairs if you want to create metadata for your endpoint configuration.
15. Choose Create endpoint configuration.

Create an endpoint
1. Sign in to the Amazon SageMaker console.
2. In the navigation tab, choose Inference.
3. Next, choose Endpoints.
4. Choose Create endpoint.
5. For Endpoint name, enter a name than is unique within a Region in your account.
6. For Attach endpoint configuration, select Use an existing endpoint configuration.
7. For Endpoint configuration, select the name of the endpoint configuration you created in the previous section and then choose Select endpoint configuration.
8. (Optional) For Tags, enter key-value pairs if you want to create metadata for your endpoint.
9. Choose Create endpoint.

<img width="461" alt="serverless-endpoints-create" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/67476fd2-631f-45d1-93e4-e411bbd6cbdd">

Find the example [here](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-Serverless-Inference).

To invoke an endpoint
```
runtime = boto3.client("sagemaker-runtime")

endpoint_name = "<your-endpoint-name>"
content_type = "<request-mime-type>"
payload = <your-request-body>

response = runtime.invoke_endpoint(
    EndpointName=endpoint_name,
    ContentType=content_type,
    Body=payload
)
```

### Asynchronous inference
Amazon SageMaker Asynchronous Inference is a capability in SageMaker that queues incoming requests and processes them asynchronously. This option is ideal for requests with large payload sizes (up to 1GB), long processing times (up to one hour), and near real-time latency requirements. Asynchronous Inference enables you to save on costs by autoscaling the instance count to zero when there are no requests to process, so you only pay when your endpoint is processing requests.

Creating an asynchronous inference endpoint is similar to creating real-time inference endpoints. You can use your existing SageMaker models and only need to specify the AsyncInferenceConfig object while creating your endpoint configuration with the EndpointConfig field in the CreateEndpointConfig API. The following diagram shows the architecture and workflow of Asynchronous Inference.

Please follow the example [link](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-Async-Inference).

### Batch Transform
Use batch transform when you need to do the following:

* Preprocess datasets to remove noise or bias that interferes with training or inference from your dataset.
* Get inferences from large datasets.
* Run inference when you don't need a persistent endpoint.
* Associate input records with inferences to assist the interpretation of results.

When a batch transform job starts, SageMaker initializes compute instances and distributes the inference or preprocessing workload between them. Batch Transform partitions the Amazon S3 objects in the input by key and maps Amazon S3 objects to instances. When you have multiple files, one instance might process input1.csv, and another instance might process the file named input2.csv. If you have one input file but initialize multiple compute instances, only one instance processes the input file and the rest of the instances are idle.

Follow the example [here](https://github.com/ankitakotadiya/Data-Engineering-ML/tree/main/SageMaker-Batch-Transform-Inference).

### Update models in production
Deployment guardrails are a set of model deployment options in Amazon SageMaker Inference to update your machine learning models in production. Using the fully managed deployment options, you can control the switch from the current model in production to a new one. Traffic shifting modes in blue/green deployments, such as canary and linear, give you granular control over the traffic shifting process from your current model to the new one during the course of the update. There are also built-in safeguards such as auto-rollbacks that help you catch issues early and automatically take corrective action before they significantly impact production.

#### Blue/Green Deployments
You can shift traffic from your old fleet (the blue fleet) to a new fleet (green fleet) with the updates. Blue/green deployments offer multiple traffic shifting modes. A traffic shifting mode is a configuration that specifies how SageMaker routes endpoint traffic to a new fleet containing your updates. The following traffic shifting modes provide you with different levels of control over the endpoint update process:

* All At Once Traffic Shifting shifts all of your endpoint traffic from the blue fleet to the green fleet. Once the traffic shifts to the green fleet, your pre-specified Amazon CloudWatch alarms begin monitoring the green fleet for a set amount of time (the baking period). If no alarms trip during the baking period, then SageMaker terminates the blue fleet.
* Canary Traffic Shifting shifts one small portion of your traffic (a canary) to the green fleet and monitor it for a baking period. If the canary succeeds on the green fleet, then SageMaker shifts the rest of the traffic from the blue fleet to the green fleet before terminating the blue fleet.
* Linear Traffic Shifting provides even more customization over the number of traffic-shifting steps and the percentage of traffic to shift for each step. While canary shifting lets you shift traffic in two steps, linear shifting extends this to n linearly spaced steps.

#### Rolling Deployments
You can update your endpoint as SageMaker incrementally provisions capacity and shifts traffic to a new fleet in steps of a batch size that you specify. Instances on the new fleet are updated with the new deployment configuration, and if no CloudWatch alarms trip during the baking period, then SageMaker cleans up instances on the old fleet. This option gives you granular control over the instance count or capacity percentage shifted during each step.

### Shadow tests
With Amazon SageMaker you can evaluate any changes to your model serving infrastructure by comparing its performance against the currently deployed infrastructure. This practice is known as shadow testing. Shadow testing can help you catch potential configuration errors and performance issues before they impact end users. With SageMaker, you don't need to invest in building your shadow testing infrastructure, so you can focus on model development.

You can schedule the test to start at any time and continue for a specified duration. The default duration is 7 days and the maximum is 30 days. After the test is complete, the endpoint reverts to the state it was in prior to starting the test. This ensures that you do not have to manually clean up resources upon the completion of the test.

1. Open the SageMaker console.
2. In the left navigation panel, choose Inference, and then choose Shadow tests.
3. Choose Create shadow test.
4. Under Name, enter a name for the test.
5. (Optional) Under Description, enter a description for the test.
6. (Optional) Specify Tags using Key and Value pairs.
7. Choose Next.

If you want to use an existing endpoint for your test, fill out the Enter shadow test settings page by doing the following:
1. Choose a role that has the AmazonSageMakerFullAccess IAM policy attached.
2. Choose Use an existing endpoint, and then choose one of the available endpoints.
3. (Optional) To encrypt the storage volume on your endpoint, either choose an existing KMS key or choose Enter a KMS key ARN from the dropdown list under Encryption key. If you choose the second option, a field to enter the KMS key ARN appears. Enter the KMS key ARN in that field.
4. If you have multiple production variants behind that endpoint, remove the ones you don't want to use for the test. You can remove a model variant by selecting it and then choosing Remove.
5. If you do not already have a shadow variant, add a shadow variant. In the Add model dialog box, choose the model you want to use for your shadow variant.
6. (Optional) In the preceding step, the shadow variant is added with the default settings. To modify these settings, select the shadow variant and choose Edit. The Edit shadow variant dialog box appears. For more information on filling out this dialog box, see Edit a shadow test.
7. In the Schedule section, choose Duration, Start time, End time, and Date and choose Apply.
8. (Optional) Turn on Enable data capture to save inference request and response information from your endpoint to an Amazon S3 bucket, and then enter the location of the Amazon S3 bucket.
9. Choose Create shadow test.

You can view the statuses of your shadow tests, monitor their progress from a dashboard, and perform actions, such as starting or stopping an test early or deleting an test. The following sections show how you can view and modify your shadow tests using the SageMaker console.






































































 






































   



































   















