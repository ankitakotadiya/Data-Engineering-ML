# AWS ML Services

## AWS Polly
Amazon Polly is a cloud service that converts text into lifelike speech. You can use Amazon Polly to develop applications that increase engagement and accessibility. Amazon Polly supports multiple languages and includes a variety of lifelike voices, so you can build speech-enabled applications that work in multiple locations and use the ideal voice for your customers. With Amazon Polly, you only pay for the text you synthesize. You can also cache and replay Amazon Polly’s generated speech at no additional cost.

Amazon Polly offers many voice options, including: Long-form voices, which produce human-like, highly expressive, and emotionally adept voices, and Neural Text-to-Speech (NTTS) voices. These voices deliver ground-breaking improvements in speech quality through new machine learning technology, and offer the most natural and human-like text-to-speech voices possible. Neural TTS technology also supports a Newscaster speaking style that is tailored to news narration use cases.

1. Sign in to the AWS Management Console and open the [Amazon Polly console](https://console.aws.amazon.com/polly/).
2. Choose the Text-to-Speech tab. The text field will load with example text so you can quickly try out Amazon Polly.
3. Turn off SSML.
4. Under Engine, choose Standard, Neural, or Long Form.
5. Choose a language and AWS Region, then choose a voice. If you choose Neural for Engine, only the languages and voices that support NTTS are available. All Standard and Long Form voices are disabled.
6. Choose Listen.
7. Type or paste this text into the input box.
   ```
   He was caught up in the game. 
   In the middle of the 10/3/2014 W3C meeting
   he shouted, "Score!" quite loudly.
   ```
8. Choose a language and AWS Region, then choose a voice. If you choose Neural for Engine, only the languages and voices that support NTTS are available. All Standard and Long Form voices are disabled.
9. To listen to the speech immediately, choose Listen.
10. To save the speech to a file, do one of the following:
    * Choose Download.
    * To change to a different file format, expand Additional settings, turn on Speech file format settings, choose the file format that you want, and then choose Download.

## Amazon Lex
Amazon Lex is an AWS service for building conversational interfaces for applications using voice and text. With Amazon Lex, the same conversational engine that powers Amazon Alexa is now available to any developer, enabling you to build sophisticated, natural language chatbots into your new and existing applications. Amazon Lex provides the deep functionality and flexibility of natural language understanding (NLU) and automatic speech recognition (ASR) so you can build highly engaging user experiences with lifelike, conversational interactions, and create new categories of products.

Amazon Lex enables any developer to build conversational chatbots quickly. With Amazon Lex, no deep learning expertise is necessary—to create a bot, you just specify the basic conversation flow in the Amazon Lex console. Amazon Lex manages the dialogue and dynamically adjusts the responses in the conversation. Using the console, you can build, test, and publish your text or voice chatbot. You can then add the conversational interfaces to bots on mobile devices, web applications, and chat platforms (for example, Facebook Messenger).

### How It Works
1. Create a bot and configure it with one or more intents that you want to support. Configure the bot so it understands the user's goal (intent), engages in conversation with the user to elicit information, and fulfills the user's intent.
2. Test the bot. You can use the test window client provided by the Amazon Lex console.
3. Publish a version and create an alias.
4. Deploy the bot. You can deploy the bot on platforms such as mobile applications or messaging platforms such as Facebook Messenger.

* Bot – A bot performs automated tasks such as ordering a pizza, booking a hotel, ordering flowers, and so on. An Amazon Lex bot is powered by Automatic Speech Recognition (ASR) and Natural Language Understanding (NLU) capabilities. Each bot must have a unique name within your account.
* Intent – An intent represents an action that the user wants to perform. You create a bot to support one or more related intents. For example, you might create a bot that orders pizza and drinks. For each intent, you provide the following required information:
  * Intent name– A descriptive name for the intent. For example, OrderPizza. Intent names must be unique within your account.
  * Sample utterances – How a user might convey the intent. For example, a user might say "Can I order a pizza please" or "I want to order a pizza".
  * How to fulfill the intent – How you want to fulfill the intent after the user provides the necessary information (for example, place order with a local pizza shop). We recommend that you create a Lambda function to fulfill the intent.
* Slot – An intent can require zero or more slots or parameters. You add slots as part of the intent configuration. At runtime, Amazon Lex prompts the user for specific slot values. The user must provide values for all required slots before Amazon Lex can fulfill the intent.

For example, the OrderPizza intent requires slots such as pizza size, crust type, and number of pizzas. In the intent configuration, you add these slots. For each slot, you provide slot type and a prompt for Amazon Lex to send to the client to elicit data from the user. A user can reply with a slot value that includes additional words, such as "large pizza please" or "let's stick with small." Amazon Lex can still understand the intended slot value.

* Slot type – Each slot has a type. You can create your custom slot types or use built-in slot types. Each slot type must have a unique name within your account. For example, you might create and use the following slot types for the OrderPizza intent:


## Amazon Comprehend
Amazon Comprehend uses natural language processing (NLP) to extract insights about the content of documents. It develops insights by recognizing the entities, key phrases, language, sentiments, and other common elements in a document. Use Amazon Comprehend to create new products based on understanding the structure of documents. For example, using Amazon Comprehend you can search social networking feeds for mentions of products or scan an entire document repository for key phrases.

### Amazon Comprehend insights
Amazon Comprehend analyzes the following types of insights:

* Entities – References to the names of people, places, items, and locations contained in a document.
* Key phrases – Phrases that appear in a document. For example, a document about a basketball game might return the names of the teams, the name of the venue, and the final score.
* Personally Identifiable Information (PII) – Personal data that can identify an individual, such as an address, bank account number, or phone number.
* Language – The dominant language of a document.
* Sentiment – The dominant sentiment of a document, which can be positive, neutral, negative, or mixed.
* Targeted sentiment – The sentiments associated with specific entities in a document. The sentiment for each entity occurrence can be positive, negative, neutral or mixed.
* Syntax – The parts of speech for each word in the document.

* Sign in to the AWS Management Console and open the [Amazon Comprehend console](https://console.aws.amazon.com/comprehend/).
* From the left menu, choose Analysis Jobs and then choose Create job.
* Under Job settings, give the job a name. The name must be unique within the Region and account.
* For Analysis Type, choose Entities.
* For Language, choose the language of the input documents.
* Under Input data, for Data source, choose Example documents. The console sets S3 location to be the folder containing the public samples.
* Under Output data, in S3 location, paste the URL or folder location in Amazon S3 for the output files.
* Under Access permissions section, select Create an IAM role. The console creates a new IAM role with the proper permissions for Amazon Comprehend to access the input and output buckets.
* When you have finished filling out the form, choose Create job to create and start the topic detection job.
* The new job appears in the job list with the status field showing the status of the job. The field can be IN_PROGRESS for a job that is processing, COMPLETED for a job that has finished successfully, and FAILED for a job that has an error.
* Choose the job to open the Job details panel.
* Under Output, in Output data location choose the link to open the Amazon S3 console.
* In the Amazon S3 console, choose Download and save the output.tar.gz file.
* Decompress the file and save it as a Json file.
* See Entities for a description of the entity types and the fields for each detected entity.

### Flywheel overview
A flywheel is an Amazon Comprehend resource that orchestrates the training and evaluation of new versions of a custom model. You can create a flywheel to use an existing trained model, or Amazon Comprehend can create and train a new model for the flywheel. Use flywheels with plain-text custom models for custom classification or custom entity recognition.

### Code Example
```
class ComprehendClassifier:
    """Encapsulates an Amazon Comprehend custom classifier."""

    def __init__(self, comprehend_client):
        """
        :param comprehend_client: A Boto3 Comprehend client.
        """
        self.comprehend_client = comprehend_client
        self.classifier_arn = None


    def create(
        self,
        name,
        language_code,
        training_bucket,
        training_key,
        data_access_role_arn,
        mode,
    ):
        """
        Creates a custom classifier. After the classifier is created, it immediately
        starts training on the data found in the specified Amazon S3 bucket. Training
        can take 30 minutes or longer. The `describe_document_classifier` function
        can be used to get training status and returns a status of TRAINED when the
        classifier is ready to use.

        :param name: The name of the classifier.
        :param language_code: The language the classifier can operate on.
        :param training_bucket: The Amazon S3 bucket that contains the training data.
        :param training_key: The prefix used to find training data in the training
                             bucket. If multiple objects have the same prefix, all
                             of them are used.
        :param data_access_role_arn: The Amazon Resource Name (ARN) of a role that
                                     grants Comprehend permission to read from the
                                     training bucket.
        :return: The ARN of the newly created classifier.
        """
        try:
            response = self.comprehend_client.create_document_classifier(
                DocumentClassifierName=name,
                LanguageCode=language_code,
                InputDataConfig={"S3Uri": f"s3://{training_bucket}/{training_key}"},
                DataAccessRoleArn=data_access_role_arn,
                Mode=mode.value,
            )
            self.classifier_arn = response["DocumentClassifierArn"]
            logger.info("Started classifier creation. Arn is: %s.", self.classifier_arn)
        except ClientError:
            logger.exception("Couldn't create classifier %s.", name)
            raise
        else:
            return self.classifier_arn

```

## Amazon Translate
Amazon Translate is a text translation service that uses advanced machine learning technologies to provide high-quality translation on demand. You can use Amazon Translate to translate unstructured text documents or to build applications that work in multiple languages. See Supported languages and language codes.

In Real-time translation, choose the target language. Amazon Translate autodetects the source language, or you can choose a source language. Enter the text that you want to translate in the left-hand text box. The translated text appears in the right-hand text box.

![translate](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/f57ef1f8-4c47-404c-a7a9-2a61aeda164c)

In the Application integration section you can see the JSON input and output for the TranslateText operation.

![json](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/8620ff65-1be0-4fea-9fc5-f1ace6c42e54)

* Extract named entities, sentiment, and key phrases from unstructured text, such as social media streams with Amazon Comprehend.
* Make subtitles and live captioning available in many languages with Amazon Transcribe.
* Speak translated content with Amazon Polly.
* Translate document repositories stored in Amazon S3 .
* Translate text stored in the following databases: Amazon DynamoDB, Amazon Aurora, and Amazon Redshift.
* Seamlessly integrate workflows with AWS Lambda or AWS Glue.

## Amazon Transcribe
Amazon Transcribe is an automatic speech recognition service that uses machine learning models to convert audio to text. You can use Amazon Transcribe as a standalone transcription service or to add speech-to-text capabilities to any application.

With Amazon Transcribe, you can improve accuracy for your specific use case with language customization, filter content to ensure customer privacy or audience-appropriate language, analyze content in multi-channel audio, partition the speech of individual speakers, and more.

Amazon Transcribe is covered under AWS’s HIPAA eligibility and BAA which requires BAA customers to encrypt all PHI at rest and in transit when in use. Automatic PHI identification is available at no additional charge and in all regions where Amazon Transcribe operates. For more information, refer to HIPAA eligibility and BAA.

Amazon Transcribe is a pay-as-you-go service; pricing is based on seconds of transcribed audio, billed on a monthly basis.

Transcription methods can be separated into two main categories:

* Batch transcriptions: Transcribe media files that have been uploaded into an Amazon S3 bucket. You can use the AWS CLI, AWS Management Console, and various AWS SDKs for batch transcriptions.
* Streaming transcriptions: Transcribe media streams in real time. You can use the AWS Management Console, HTTP/2, WebSockets, and various AWS SDKs for streaming transcriptions.

## Amazon Recokgnition
Amazon Rekognition makes it easy to add image and video analysis to your applications. You just have to provide an image or video to the Amazon Rekognition API, and the service can:

* Identify labels (objects, concepts, people, scenes, and activities) and text
* Detect inappropriate content
* Provide highly accurate facial analysis, face comparison, and face search capabilities

With Amazon Rekognition's face recognition APIs, you can detect, analyze, and compare faces for a wide variety of use cases, including user verification, cataloging, people counting, and public safety.

Amazon Rekognition is based on the same proven, highly scalable, deep learning technology developed by Amazon’s computer vision scientists to analyze billions of images and videos daily. It requires no machine learning expertise to use. Amazon Rekognition includes a simple, easy-to-use API that can quickly analyze any image or video file that’s stored in Amazon S3. It's always learning from new data, and we’re continually adding new labels and facial comparison features to the service.

## Amazon Forecast
Amazon Forecast is a fully managed service that uses statistical and machine learning algorithms to deliver highly accurate time-series forecasts. Based on the same technology used for time-series forecasting at Amazon.com, Forecast provides state-of-the-art algorithms to predict future time-series data based on historical data, and requires no machine learning experience.

When creating forecasting projects in Amazon Forecast, you work with the following resources:
* Importing Datasets – Datasets are collections of your input data. Dataset groups are collections of datasets that contain complimentary information. Forecast algorithms use your dataset groups to train custom forecasting models, called predictors.
* Training Predictors – Predictors are custom models trained on your data. You can train a predictor by choosing a prebuilt algorithm,or by choosing the AutoML option to have Amazon Forecast pick the best algorithm for you.
* Generating Forecasts – You can generate forecasts for your time-series data, query them using the QueryForecast API, or visualize them in the console.

### Forecast Algorithms
#### Autoregressive Integrated Moving Average (ARIMA) Algorithm
Autoregressive Integrated Moving Average (ARIMA) is a commonly-used local statistical algorithm for time-series forecasting. ARIMA captures standard temporal structures (patterned organizations of time) in the input dataset. The Amazon Forecast ARIMA algorithm calls the Arima function in the Package 'forecast' of the Comprehensive R Archive Network (CRAN).

The ARIMA algorithm is especially useful for datasets that can be mapped to stationary time series. The statistical properties of stationary time series, such as autocorrelations, are independent of time. Datasets with stationary time series usually contain a combination of signal and noise. The signal may exhibit a pattern of sinusoidal oscillation or have a seasonal component. ARIMA acts like a filter to separate the signal from the noise, and then extrapolates the signal in the future to make predictions.

#### CNN-QR Algorithm
Amazon Forecast CNN-QR, Convolutional Neural Network - Quantile Regression, is a proprietary machine learning algorithm for forecasting scalar (one-dimensional) time series using causal convolutional neural networks (CNNs). This supervised learning algorithm trains one global model from a large collection of time series and uses a quantile decoder to make probabilistic predictions.

To facilitate learning time-dependent patterns, such as spikes during weekends, CNN-QR automatically creates feature time series based on time-series granularity. For example, CNN-QR creates two feature time series (day-of-month and day-of-year) at a weekly time-series frequency. The algorithm uses these derived feature time series along with the custom feature time series provided during training and inference. The following example shows a target time series, zi,t, and two derived time-series features: ui,1,t represents the hour of the day, and ui,2,t represents the day of the week.

#### DeepAR+ Algorithm
Amazon Forecast DeepAR+ is a supervised learning algorithm for forecasting scalar (one-dimensional) time series using recurrent neural networks (RNNs). Classical forecasting methods, such as autoregressive integrated moving average (ARIMA) or exponential smoothing (ETS), fit a single model to each individual time series, and then use that model to extrapolate the time series into the future. In many applications, however, you have many similar time series across a set of cross-sectional units. These time-series groupings demand different products, server loads, and requests for web pages. In this case, it can be beneficial to train a single model jointly over all of the time series. DeepAR+ takes this approach. When your dataset contains hundreds of feature time series, the DeepAR+ algorithm outperforms the standard ARIMA and ETS methods. You can also use the trained model for generating forecasts for new time series that are similar to the ones it has been trained on.

#### Exponential Smoothing (ETS) Algorithm
The ETS algorithm is especially useful for datasets with seasonality and other prior assumptions about the data. ETS computes a weighted average over all observations in the input time series dataset as its prediction. The weights are exponentially decreasing over time, rather than the constant weights in simple moving average methods. The weights are dependent on a constant parameter, which is known as the smoothing parameter.

#### Non-Parametric Time Series (NPTS) Algorithm
The Amazon Forecast Non-Parametric Time Series (NPTS) algorithm is a scalable, probabilistic baseline forecaster. It predicts the future value distribution of a given time series by sampling from past observations. The predictions are bounded by the observed values. NPTS is especially useful when the time series is intermittent (or sparse, containing many 0s) and bursty. For example, forecasting demand for individual items where the time series has many low counts. Amazon Forecast provides variants of NPTS that differ in which of the past observations are sampled and how they are sampled. To use an NPTS variant, you choose a hyperparameter setting.

#### Prophet Algorithm
Prophet is a popular local Bayesian structural time series model. The Amazon Forecast Prophet algorithm uses the Prophet class of the Python implementation of Prophet.

Prophet is especially useful for datasets that:
* Contain an extended time period (months or years) of detailed historical observations (hourly, daily, or weekly)
* Have multiple strong seasonalities
* Include previously known important, but irregular, events
* Have missing data points or large outliers
* Have non-linear growth trends that are approaching a limit

Prophet is an additive regression model with a piecewise linear or logistic growth curve trend. It includes a yearly seasonal component modeled using Fourier series and a weekly seasonal component modeled using dummy variables.


## Amazon Personalise
Amazon Personalize is a fully managed machine learning service that uses your data to generate item recommendations for your users. It can also generate user segments based on the users' affinity for certain items or item metadata.

Amazon Personalize uses your data to train domain-based or customizable recommendation models. You use a private recommendation API in your application to request real-time recommendations. Amazon Personalize also supports batch workflows get item recommendations and user segments.

## Amazon Textract
Amazon Textract helps you add document text detection and analysis to your applications. Using Amazon Textract, you can do the following:

* Detect typed and handwritten text in a variety of documents, including financial reports, medical records, and tax forms.
* Extract text, forms, and tables from documents with structured data, using the Amazon Textract Document Analysis API.
* Specify and extract information from documents using the Queries feature within the Amazon Textract Analyze Document API.
* Process invoices and receipts with the AnalyzeExpense API.
* Process ID documents such as drivers licenses and passports issued by U.S. government, using the AnalyzeID API.
* Upload and process mortgage loan packages, through automatic routing of the the document pages to the appropriate Amazon Textract analysis operations using the Analyze Lending workflow. You can retrieve analysis results for each document page or you can retrieve summarized results for a set of document pages.
* Use Custom Queries to customize the pretrained Queries feature using your data to support your down stream processing needs.

### Custom Queries
With Amazon Textract document analysis, you can customize the model output through adapters trained on your own documents. Adapters are components that plug in to the Amazon Textract pre-trained deep learning model, customizing its output for your business specific documents. You create an adapter for your specific use case by annotating/labeling your sample documents and training the adapter on the annotated samples.

After you create an adapter, Amazon Textract provides you with an AdapterId. You can have multiple adapter versions within a single adapter. You can provide the AdapterId, along with an AdapterVersion, to an operation to specify that you want to use the adapter that you created. For example, you provide the two parameters to the AnalyzeDocument API for synchronous document analysis, or the StartDocumentAnalysis operation for asynchronous analysis. Providing the AdapterId as part of the request will automatically integrate the adapter into the analysis process and use it to enhance predictions for your documents. This way, you can leverage the capabilities of AnalyzeDocument while customizing the model to fit your own use case.


## DeepRacer
AWS DeepRacer is a fully autonomous 1/18th scale race car driven by reinforcement learning. It consists of the following components:

* AWS DeepRacer console: An AWS Machine Learning service for training and evaluating reinforcement learning models in a three-dimensional simulated autonomous-driving environment.
* AWS DeepRacer vehicle: A 1/18th scale RC car capable of running inference on a trained AWS DeepRacer model for autonomous driving.
* AWS DeepRacer League: The world’s first global, autonomous racing league. Race for prizes, glory, and an opportunity to advance to the World Championship Cup. For more information, see the terms and conditions.

The AWS DeepRacer console is a graphical user interface for interacting with the AWS DeepRacer service. You can use the console to train a reinforcement learning model and to evaluate the model performance in the AWS DeepRacer simulator. In the console, you can also download a trained model for deployment to your AWS DeepRacer vehicle for autonomous driving in a physical environment.


## AWS Lookout
You can use Amazon Lookout for Vision to find visual defects in industrial products, accurately and at scale. It uses computer vision to identify missing components in an industrial product, damage to vehicles or structures, irregularities in production lines, and even minuscule defects in silicon wafers—or any other physical item where quality is important such as a missing capacitor on printed circuit boards.

Amazon Lookout for Vision provides the following benefits:
* Quickly and efficiently improve processes – You can use Amazon Lookout for Vision to implement computer vision-based inspection in industrial processes quickly and efficiently, at scale. You can provide as few as 30 baseline good images and Lookout for Vision can automatically build a custom ML model for defect detection. You can then process images from IP cameras, in batch or in real time, to quickly and accurately identify anomalies like dents, cracks, and scratches.
* Increase production quality, fast – With Amazon Lookout for Vision you can reduce defects in production processes, in real time. It identifies and reports visual anomalies in a dashboard so you can take action quickly to stop more defects from occurring—increasing production quality and reducing costs.
* Reduce operational costs – Amazon Lookout for Vision reports trends in your visual inspection data, such as identifying processes with the highest defect rate or flagging recent variations in defects. Using this information, you can determine whether to schedule maintenance on the process line or reroute production to another machine before costly, unplanned downtime occurs.


## Amazon Monitron
Amazon Monitron is an end-to-end system that automatically detects abnormal behavior in industrial machinery, enabling you to take proactive action on potential failures and reduce unplanned downtime. It includes sensor devices to capture vibration and temperature data, a gateway device to securely transfer data to the AWS Cloud, the Amazon Monitron service that analyzes the data for abnormal machine patterns using machine learning, and a companion mobile app to set up the devices and track potential failures in your machinery. Reliability managers can quickly deploy Amazon Monitron to easily track machine health of industrial equipment such as such as bearings, motors, gearboxes, and pumps without any development work or specialized training.


## AWS Panorama
AWS Panorama is a service that brings computer vision to your on-premises camera network. You install the AWS Panorama Appliance or another compatible device in your datacenter, register it with AWS Panorama, and deploy computer vision applications from the cloud. AWS Panorama works with your existing real time streaming protocol (RTSP) network cameras. The appliance runs secure computer vision applications from AWS Partners, or applications that you build yourself with the AWS Panorama Application SDK.

The AWS Panorama Appliance enables you to run self-contained computer vision applications at the edge, without sending images to the AWS Cloud. By using the AWS SDK, you can integrate with other AWS services and use them to track data from the application over time. By integrating with other AWS services, you can use AWS Panorama to do the following:

* Analyze traffic patterns – Use the AWS SDK to record data for retail analytics in Amazon DynamoDB. Use a serverless application to analyze the collected data over time, detect anomalies in the data, and predict future behavior.
* Receive site safety alerts – Monitor off-limits areas at an industrial site. When your application detects a potentially unsafe situation, upload an image to Amazon Simple Storage Service (Amazon S3) and send a notification to an Amazon Simple Notification Service (Amazon SNS) topic so recipients can take corrective action.
* Improve quality control – Monitor an assembly line's output to identify parts that don't conform to requirements. Highlight images of nonconformant parts with text and a bounding box and display them on a monitor for review by your quality control team.
* Collect training and test data – Upload images of objects that your computer vision model couldn't identify, or where the model's confidence in its guess was borderline. Use a serverless application to create a queue of images that need to be tagged. Tag the images and use them to retrain the model in Amazon SageMaker.

In AWS Panorama, you create computer vision applications and deploy them to the AWS Panorama Appliance or a compatible device to analyze video streams from network cameras. You write application code in Python and build application containers with Docker. You use the AWS Panorama Application CLI to import machine learning models locally or from Amazon Simple Storage Service (Amazon S3). Applications use the AWS Panorama Application SDK to receive video input from a camera and interact with a model.


## Deep Composer
Also known as the AWS DeepComposer physical keyboard or hardware keyboard. You can connect, or link, the keyboard to a computer that has access to the AWS DeepComposer console. You can use the linked keyboard to play and record short melodies that are fewer than eight bars. Then use a recorded melody with a supported AWS DeepComposer generative AI technique.

Sequences of notes, melodies, harmonies, and rhythms that make up a musical work.

AWS DeepComposer provides a creative, hands-on experience for learning generative AI and machine learning. With generative AI, one of the biggest recent advancements in artificial intelligence, you can create a new dataset based on a training dataset. With AWS DeepComposer, you can experiment with different generative AI architectures and models in a musical setting by creating and transforming musical inputs and accompaniments.


## Fraud Detection
Amazon Fraud Detector is a fully managed fraud detection service that automates the detection of potentially fraudulent activities online. These activities include unauthorized transactions and the creation of fake accounts. Amazon Fraud Detector works by using machine learning to analyze your data. It does this in a way that builds off of the seasoned expertise of more than 20 years of fraud detection at Amazon.

You can use Amazon Fraud Detector to build customized fraud-detection models, add decision logic to interpret the model’s fraud evaluations, and assign outcomes such as pass or send for review for each possible fraud evaluation. With Amazon Fraud Detector, you don't need machine learning expertise to detect fraudulent activities.

To get started, collect and prepare fraud data that you collected at your organization. Amazon Fraud Detector then uses this data to train, test, and deploy a custom fraud detection model on your behalf. As a part of this process, Amazon Fraud Detector uses machine learning models that have learned patterns of fraud from AWS and Amazon’s own fraud expertise to evaluate your fraud data and generate model scores and model performance data. You configure decision logic to interpret the model’s score and assign outcomes for how to deal with each fraud evaluation.


## AWS CodeGuru
Amazon CodeGuru Reviewer uses program analysis combined with machine learning models trained on millions of lines of Java and Python code from the Amazon code base and other sources. When you associate CodeGuru Reviewer with a repository, CodeGuru Reviewer can find and flag code defects and suggest recommendations to improve your code. CodeGuru Reviewer provides actionable recommendations with a low rate of false positives and might improve its ability to analyze code over time based on user feedback.

You can associate CodeGuru Reviewer with a repository to allow CodeGuru Reviewer to provide recommendations. After you associate a repository with CodeGuru Reviewer, CodeGuru Reviewer automatically analyzes pull requests that you make, and you can choose to run repository analyses on the code in your branch to analyze all the code at any time.

If you want to suppress recommendations from CodeGuru Reviewer, you can create and add to the root directory of your repository an aws-codeguru-reviewer.yml file that lists files and directories to exclude from analysis. For more information, see Suppress recommendations.


## Amazon Kendra













