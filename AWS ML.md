# AWS ML

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










