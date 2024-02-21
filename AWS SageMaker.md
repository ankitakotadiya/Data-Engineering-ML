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

























   















