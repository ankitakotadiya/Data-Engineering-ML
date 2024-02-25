# SageMaker-MLOps
Amazon SageMaker supports features to implement machine learning models in production environments with continuous integration and deployment.

## Amazon SageMaker ML Lineage Tracking
Amazon SageMaker ML Lineage Tracking creates and stores information about the steps of a machine learning (ML) workflow from data preparation to model deployment. With the tracking information, you can reproduce the workflow steps, track model and dataset lineage, and establish model governance and audit standards.

Tracking entities maintain a representation of all the elements of your end-to-end machine learning workflow. You can use this representation to establish model governance, reproduce your workflow, and maintain a record of your work history.

### Lineage entities
* Trial Component – Represents processing, training, and transform jobs in the lineage. Also part of experiment management.
* Context – Provides a logical grouping of other tracking or experiment entities. Conceptually, experiments and trials are contexts. Some examples are an endpoint and a model package.
* Action – Represents an action or activity. Generally, an action involves at least one input artifact or output artifact. Some examples are a workflow step and a model deployment.
* Artifact – Represents a URI addressable object or data. An artifact is generally either an input or an output to a trial component or action. Some examples include a dataset (S3 bucket URI), or an image (Amazon ECR registry path).
* Association – Links other tracking or experiment entities, such as an association between the location of training data and a training job.

## Register and Deploy Models with Model Registry
Catalog models by creating SageMaker Model Registry Model (Package) Groups that contain different versions of a model. You can create a Model Group that tracks all of the models that you train to solve a particular problem. You can then register each model you train and the Model Registry adds it to the Model Group as a new model version. Lastly, you can create categories of Model Groups by further organizing them into SageMaker Model Registry Collections. A typical workflow might look like the following:

* Create a Model Group.
* Create an ML pipeline that trains a model. For information about SageMaker pipelines.
* For each run of the ML pipeline, create a model version that you register in the Model Group you created in the first step.
* Add your Model Group into one or more Model Registry Collections.

### Set Up Your Environment
Create a new SageMaker session using the following code block. This returns the role ARN for the session. This role ARN should be the execution role ARN that you set up as a prerequisite.

```
import boto3
import sagemaker
import sagemaker.session
from sagemaker.workflow.pipeline_context import PipelineSession

region = boto3.Session().region_name
sagemaker_session = sagemaker.session.Session()
role = sagemaker.get_execution_role()
default_bucket = sagemaker_session.default_bucket()

pipeline_session = PipelineSession()

model_package_group_name = f"AbaloneModelPackageGroupName"
```

### Create a Pipeline
Run the following steps from your SageMaker notebook instance to create a pipeline including steps for preprocessing, training, evaluation, conditional evaluation, and model registration.

1. Open the SageMaker Studio console by following the instructions in Launch Amazon SageMaker Studio.
2. In the left navigation pane, select Pipelines.
3. (Optional) To filter the list of pipelines by name, enter a full or partial pipeline name in the search field.
4. Select a pipeline name to view details about the pipeline. The pipeline's Executions page opens and displays a list of pipeline executions. Use the Column icon (  ) to choose which columns to display.
5. From the pipeline's Executions page, choose one of the following pages in the Overview, Settings, or Details dropdown menus (to the left of the pipeline executions table) to view pipeline details:
   * Executions – Details about the executions.
   * Graph – The DAG for the pipeline.
   * Parameters – Includes the model approval status.
   * Information – The metadata associated with the pipeline, such as the pipeline Amazon Resource Name (ARN) and role ARN. You can also edit the pipeline description from this page.

## Monitor data quality
Data quality monitoring automatically monitors machine learning (ML) models in production and notifies you when data quality issues arise. ML models in production have to make predictions on real-life data that is not carefully curated like most training datasets. If the statistical nature of the data that your model receives while in production drifts away from the nature of the baseline data it was trained on, the model begins to lose accuracy in its predictions. Amazon SageMaker Model Monitor uses rules to detect data drift and alerts you when it happens. To monitor data quality, follow these steps:

* Enable data capture. This captures inference input and output from a real-time inference endpoint or batch transform job and stores the data in Amazon S3. For more information.
* Create a baseline. In this step, you run a baseline job that analyzes an input dataset that you provide. The baseline computes baseline schema constraints and statistics for each feature using Deequ, an open source library built on Apache Spark, which is used to measure data quality in large datasets. For more information.
* Define and schedule data quality monitoring jobs. For specific information and code samples of data quality monitoring jobs, see Schedule data quality monitoring jobs. For general information about monitoring jobs, see Schedule monitoring jobs.
* View data quality metrics. For more information.
* Integrate data quality monitoring with Amazon CloudWatch.
* Interpret the results of a monitoring job.
* Use SageMaker Studio to enable data quality monitoring and visualize results if you are using a real-time endpoint.

### Create a Baseline
The baseline calculations of statistics and constraints are needed as a standard against which data drift and other data quality issues can be detected. Model Monitor provides a built-in container that provides the ability to suggest the constraints automatically for CSV and flat JSON input.

The training dataset that you used to train the model is usually a good baseline dataset. The training dataset data schema and the inference dataset schema should exactly match (the number and order of the features). Note that the prediction/output columns are assumed to be the first columns in the training dataset. From the training dataset, you can ask SageMaker to suggest a set of baseline constraints and generate descriptive statistics to explore the data. For this example, upload the training dataset that was used to train the pretrained model included in this example. If you already stored the training dataset in Amazon S3, you can point to it directly.

```
from sagemaker.model_monitor import DefaultModelMonitor
from sagemaker.model_monitor.dataset_format import DatasetFormat

my_default_monitor = DefaultModelMonitor(
    role=role,
    instance_count=1,
    instance_type='ml.m5.xlarge',
    volume_size_in_gb=20,
    max_runtime_in_seconds=3600,
)

my_default_monitor.suggest_baseline(
    baseline_dataset=baseline_data_uri+'/training-dataset-with-header.csv',
    dataset_format=DatasetFormat.csv(header=True),
    output_s3_uri=baseline_results_uri,
    wait=True
)
```

### Schedule data quality monitoring jobs
After you create your baseline, you can call the create_monitoring_schedule() method of your DefaultModelMonitor class instance to schedule an hourly data quality monitor. The following sections show you how to create a data quality monitor for a model deployed to a real-time endpoint as well as for a batch transform job.

```
from sagemaker.model_monitor import CronExpressionGenerator
                
data_quality_model_monitor = DefaultModelMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = data_quality_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   statistics=data_quality_model_monitor.baseline_statistics(),
   constraints=data_quality_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
   )
)
```

### Schema for Statistics (statistics.json file)
Amazon SageMaker Model Monitor prebuilt container computes per column/feature statistics. The statistics are calculated for the baseline dataset and also for the current dataset that is being analyzed.

```
{
    "version": 0,
    # dataset level stats
    "dataset": {
        "item_count": number
    },
    # feature level stats
    "features": [
        {
            "name": "feature-name",
            "inferred_type": "Fractional" | "Integral",
            "numerical_statistics": {
                "common": {
                    "num_present": number,
                    "num_missing": number
                },
                "mean": number,
                "sum": number,
                "std_dev": number,
                "min": number,
                "max": number,
                "distribution": {
                    "kll": {
                        "buckets": [
                            {
                                "lower_bound": number,
                                "upper_bound": number,
                                "count": number
                            }
                        ],
                        "sketch": {
                            "parameters": {
                                "c": number,
                                "k": number
                            },
                            "data": [
                                [
                                    num,
                                    num,
                                    num,
                                    num
                                ],
                                [
                                    num,
                                    num
                                ][
                                    num,
                                    num
                                ]
                            ]
                        }#sketch
                    }#KLL
                }#distribution
            }#num_stats
        },
        {
            "name": "feature-name",
            "inferred_type": "String",
            "string_statistics": {
                "common": {
                    "num_present": number,
                    "num_missing": number
                },
                "distinct_count": number,
                "distribution": {
                    "categorical": {
                         "buckets": [
                                {
                                    "value": "string",
                                    "count": number
                                }
                          ]
                     }
                }
            },
            #provision for custom stats
        }
    ]
}
```

### CloudWatch Metrics
You can use the built-in Amazon SageMaker Model Monitor container for CloudWatch metrics. When the emit_metrics option is Enabled in the baseline constraints file, SageMaker emits these metrics for each feature/column observed in the dataset in the following namespace:

* For real-time endpoints: /aws/sagemaker/Endpoints/data-metric namespace with EndpointName and ScheduleName dimensions.
* For batch transform jobs: /aws/sagemaker/ModelMonitoring/data-metric namespace with MonitoringSchedule dimension.

For numerical fields, the built-in container emits the following CloudWatch metrics:
* Metric: Max → query for MetricName: feature_data_{feature_name}, Stat: Max
* Metric: Min → query for MetricName: feature_data_{feature_name}, Stat: Min
* Metric: Sum → query for MetricName: feature_data_{feature_name}, Stat: Sum
* Metric: SampleCount → query for MetricName: feature_data_{feature_name}, Stat: SampleCount
* Metric: Average → query for MetricName: feature_data_{feature_name}, Stat: Average

## Monitor model quality
Model quality monitoring jobs monitor the performance of a model by comparing the predictions that the model makes with the actual Ground Truth labels that the model attempts to predict. To do this, model quality monitoring merges data that is captured from real-time or batch inference with actual labels that you store in an Amazon S3 bucket, and then compares the predictions with the actual labels.

Create a baseline job that compares your model predictions with ground truth labels in a baseline dataset that you have stored in Amazon S3. Typically, you use a training dataset as the baseline dataset. The baseline job calculates metrics for the model and suggests constraints to use to monitor model quality drift.

To create a baseline job, you need to have a dataset that contains predictions from your model along with labels that represent the Ground Truth for your data.

1. First, create an instance of the ModelQualityMonitor class. The following code snippet shows how to do this.
```
from sagemaker import get_execution_role, session, Session
from sagemaker.model_monitor import ModelQualityMonitor
                
role = get_execution_role()
session = Session()

model_quality_monitor = ModelQualityMonitor(
    role=role,
    instance_count=1,
    instance_type='ml.m5.xlarge',
    volume_size_in_gb=20,
    max_runtime_in_seconds=1800,
    sagemaker_session=session
)
```
2. Now call the suggest_baseline method of the ModelQualityMonitor object to run a baseline job. The following code snippet assumes that you have a baseline dataset that contains both predictions and labels stored in Amazon S3.
```
baseline_job_name = "MyBaseLineJob"
job = model_quality_monitor.suggest_baseline(
    job_name=baseline_job_name,
    baseline_dataset=baseline_dataset_uri, # The S3 location of the validation dataset.
    dataset_format=DatasetFormat.csv(header=True),
    output_s3_uri = baseline_results_uri, # The S3 location to store the results.
    problem_type='BinaryClassification',
    inference_attribute= "prediction", # The column in the dataset that contains predictions.
    probability_attribute= "probability", # The column in the dataset that contains probabilities.
    ground_truth_attribute= "label" # The column in the dataset that contains ground truth labels.
)
job.wait(logs=False)
```
3. After the baseline job finishes, you can see the constraints that the job generated. First, get the results of the baseline job by calling the latest_baselining_job method of the ModelQualityMonitor object.
```
baseline_job = model_quality_monitor.latest_baselining_job
```
4. The baseline job suggests constraints, which are thresholds for metrics that model monitor measures. If a metric goes beyond the suggested threshold, Model Monitor reports a violation. To view the constraints that the baseline job generated, call the suggested_constraints method of the baseline job. The following code snippet loads the constraints for a binary classification model into a Pandas dataframe.
```
import pandas as pd
pd.DataFrame(baseline_job.suggested_constraints().body_dict["binary_classification_constraints"]).T
```

### Schedule Model Quality Monitoring Jobs
After you create your baseline, you can call the create_monitoring_schedule() method of your ModelQualityMonitor class instance to schedule an hourly model quality monitor. The following sections show you how to create a model quality monitor for a model deployed to a real-time endpoint as well as for a batch transform job.

To schedule a model quality monitor for a real-time endpoint, pass your EndpointInput instance to the endpoint_input argument of your ModelQualityMonitor instance, as shown in the following code sample:
```
from sagemaker.model_monitor import CronExpressionGenerator
                    
model_quality_model_monitor = ModelQualityMonitor(
   role=sagemaker.get_execution_role(),
   ...
)

schedule = model_quality_model_monitor.create_monitoring_schedule(
   monitor_schedule_name=schedule_name,
   post_analytics_processor_script=s3_code_postprocessor_uri,
   output_s3_uri=s3_report_path,
   schedule_cron_expression=CronExpressionGenerator.hourly(),    
   statistics=model_quality_model_monitor.baseline_statistics(),
   constraints=model_quality_model_monitor.suggested_constraints(),
   schedule_cron_expression=CronExpressionGenerator.hourly(),
   enable_cloudwatch_metrics=True,
   endpoint_input=EndpointInput(
        endpoint_name=endpoint_name,
        destination="/opt/ml/processing/input/endpoint",
        start_time_offset="-PT2D",
        end_time_offset="-PT1D",
    )
)
```

## Monitor Bias Drift for Models in Production
Amazon SageMaker Clarify bias monitoring helps data scientists and ML engineers monitor predictions for bias on a regular basis. As the model is monitored, customers can view exportable reports and graphs detailing bias in SageMaker Studio and configure alerts in Amazon CloudWatch to receive notifications if bias beyond a certain threshold is detected. Bias can be introduced or exacerbated in deployed ML models when the training data differs from the data that the model sees during deployment (that is, the live data). These kinds of changes in the live data distribution might be temporary (for example, due to some short-lived, real-world events) or permanent. In either case, it might be important to detect these changes. For example, the outputs of a model for predicting home prices can become biased if the mortgage rates used to train the model differ from current, real-world mortgage rates. With bias detection capabilities in Model Monitor, when SageMaker detects bias beyond a certain threshold, it automatically generates metrics that you can view in SageMaker Studio and through Amazon CloudWatch alerts.

### Create a Bias Drift Baseline
After you have configured your application to capture real-time or batch transform inference data, the first task to monitor for bias drift is to create a baseline. This involves configuring the data inputs, which groups are sensitive, how the predictions are captured, and the model and its post-training bias metrics. Then you need to start the baselining job.

```

model_bias_monitor = ModelBiasMonitor(
    role=role,
    sagemaker_session=sagemaker_session,
    max_runtime_in_seconds=1800,
)
```
DataConfig stores information about the dataset to be analyzed (for example, the dataset file), its format (that is, CSV or JSON Lines), headers (if any) and label.
```

model_bias_baselining_job_result_uri = f"{baseline_results_uri}/model_bias"
model_bias_data_config = DataConfig(
    s3_data_input_path=validation_dataset,
    s3_output_path=model_bias_baselining_job_result_uri,
    label=label_header,
    headers=all_headers,
    dataset_type=dataset_type,
)
```
BiasConfig is the configuration of the sensitive groups in the dataset. Typically, bias is measured by computing a metric and comparing it across groups. The group of interest is called the facet. For post-training bias, you should also take the positive label into account.
```
model_bias_config = BiasConfig(
    label_values_or_threshold=[1],
    facet_name="Account Length",
    facet_values_or_threshold=[100],
)
```
ModelPredictedLabelConfig specifies how to extract a predicted label from the model output. In this example, the 0.8 cutoff has been chosen in anticipation that customers will turn over frequently. For more complicated outputs, there are a few more options, like "label" is the index, name, or JMESPath to locate predicted label in endpoint response payload.
```
model_predicted_label_config = ModelPredictedLabelConfig(
    probability_threshold=0.8,
)
```
ModelConfig is the configuration related to the model to be used for inferencing. In order to compute post-training bias metrics, the computation needs to get inferences for the model name provided. To accomplish this, the processing job uses the model to create an ephemeral endpoint (also known as shadow endpoint). The processing job deletes the shadow endpoint after the computations are completed. This configuration is also used by the explainability monitor.

```
model_config = ModelConfig(
    model_name=model_name,
    instance_count=endpoint_instance_count,
    instance_type=endpoint_instance_type,
    content_type=dataset_type,
    accept_type=dataset_type,
)
```
Now you can start the baselining job.
```
model_bias_monitor.suggest_baseline(
    model_config=model_config,
    data_config=model_bias_data_config,
    bias_config=model_bias_config,
    model_predicted_label_config=model_predicted_label_config,
)
print(f"ModelBiasMonitor baselining job: {model_bias_monitor.latest_baselining_job_name}")
```
## Monitor Feature Attribution Drift for Models in Production
A drift in the distribution of live data for models in production can result in a corresponding drift in the feature attribution values, just as it could cause a drift in bias when monitoring bias metrics. Amazon SageMaker Clarify feature attribution monitoring helps data scientists and ML engineers monitor predictions for feature attribution drift on a regular basis. As the model is monitored, customers can view exportable reports and graphs detailing feature attributions in SageMaker Studio and configure alerts in Amazon CloudWatch to receive notifications if it is detected that the attribution values drift beyond a certain threshold.

To illustrate this with a specific situation, consider a hypothetical scenario for college admissions. Assume that we observe the following (aggregated) feature attribution values in the training data and in the live data:

| Feature    | Attribution in training data | Attribution in live data |
|------------|------------------------------|--------------------------|
| SAT score  | 0.70                         | 0.10                     |
| GPA        | 0.50                         | 0.20                     |
| Class rank | 0.05                         | 0.70                     |

















