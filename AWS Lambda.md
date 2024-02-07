# Aws Lambda
AWS Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda runs your code on a high-availability compute infrastructure and performs all of the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.

To get started with Lambda, use the Lambda console to create a function. In a few minutes, you can create and deploy a function and test it in the console. To keep things simple, you create your function using either the Python or Node.js runtime. With these interpreted languages, you can edit function code directly in the console's built-in code editor. With compiled languages like Java and C#, you need to create a deployment package on your local build machine and upload it to Lambda.

## Create a Lambda function with the console
In this example, your function takes a JSON object containing two integer values labeled "length" and "width". The function multiplies these values to calculate an area and returns this as a JSON string. Your function also prints the calculated area.

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.
2. Choose Create function.
3. Select Author from scratch.
4. In the Basic information pane, for Function name enter myLambdaFunction.
5. For Runtime, choose either Node.js 20.x or Python 3.12.
6. Leave architecture set to x86_64 and choose Create function.

Lambda creates a function that returns the message Hello from Lambda! Lambda also creates an execution role for your function. An execution role is an AWS Identity and Access Management (IAM) role that grants a Lambda function permission to access AWS services and resources. For your function, the role that Lambda creates grants basic permissions to write to CloudWatch Logs.

##### To modify the code in the console
1. Choose the Code tab.
   In the console's built-in code editor, you should see the function code that Lambda created. If you don't see the lambda_function.py tab in the code editor, select 
   lambda_function.py in the file explorer as shown on the following diagram.

   <img width="597" alt="Screenshot 2024-02-07 at 6 38 55 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/b22050eb-20be-4c22-877d-b50f4a6706e0">

2. Paste the following code into the lambda_function.py tab, replacing the code that Lambda created.

   ```
   import json
   import logging
   
   logger = logging.getLogger()
   logger.setLevel(logging.INFO)
   
   def lambda_handler(event, context):
       
       # Get the length and width parameters from the event object. The 
       # runtime converts the event object to a Python dictionary
       length=event['length']
       width=event['width']
       
       area = calculate_area(length, width)
       print(f"The area is {area}")
           
       logger.info(f"CloudWatch logs group: {context.log_group_name}")
       
       # return the calculated area as a JSON string
       data = {"area": area}
       return json.dumps(data)
       
   def calculate_area(length, width):
       return length*width
   ```

3. Select Deploy to update your function's code. When Lambda has deployed the changes, the console displays a banner letting you know that it's successfully updated your function.

##### Understanding your function code
* The Lambda handler:
  Your Lambda function contains a Python function named lambda_handler. A Lambda function in Python can contain more than one Python function, but the handler function is always the entry point to your code. When your function is invoked, Lambda runs this method.
  
* The Lambda event object:
  The function lambda_handler takes two arguments, event and context. An event in Lambda is a JSON formatted document that contains data for your function to process.

  If your function is invoked by another AWS service, the event object contains information about the event that caused the invocation. For example, if an Amazon Simple Storage Service (Amazon S3) bucket invokes your function when an object is uploaded, the event will contain the name of the Amazon S3 bucket and the object key.

* The Lambda context object:
  The second argument your function takes is context. Lambda passes the context object to your function automatically. The context object contains information about the function invocation and execution environment.

  You can use the context object to output information about your function's invocation for monitoring purposes. In this example, your function uses the log_group_name parameter to output the name of its CloudWatch log group.

* Logging in Lambda:
  With Python, you can use either a print statement or a Python logging library to send information to your function's log. To illustrate the difference in what's captured, the example code uses both methods. In a production application, I recommend that you use a logging library.

##### Invoke the Lambda function using the console
To invoke your function using the Lambda console, you first create a test event to send to your function. The event is a JSON formatted document containing two key-value pairs with the keys "length" and "width".

1. In the Code source pane, choose Test.
2. Select Create new event.
3. For Event name enter myTestEvent.
4. In the Event JSON panel, replace the default values by pasting in the following:
   ```
   {
     "length": 6,
     "width": 7
   }
   ```
5. Choose Save.

##### To test your function and view invocation records in the console
```
Test Event Name
myTestEvent

Response
"{\"area\": 42}"

Function Logs
START RequestId: 2d0b1579-46fb-4bf7-a6e1-8e08840eae5b Version: $LATEST
The area is 42
[INFO]	2023-08-31T23:43:26.428Z	2d0b1579-46fb-4bf7-a6e1-8e08840eae5b	CloudWatch logs group: /aws/lambda/myLambdaFunction
END RequestId: 2d0b1579-46fb-4bf7-a6e1-8e08840eae5b
REPORT RequestId: 2d0b1579-46fb-4bf7-a6e1-8e08840eae5b	Duration: 1.42 ms	Billed Duration: 2 ms	Memory Size: 128 MB	Max Memory Used: 39 MB	Init Duration: 123.74 ms

Request ID
2d0b1579-46fb-4bf7-a6e1-8e08840eae5b
```
##### To view your function's invocation records in CloudWatch Logs
1. Open the [Log groups](https://console.aws.amazon.com/cloudwatch/home#logs:) page of the CloudWatch console.
2. Choose the log group for your function (/aws/lambda/myLambdaFunction). This is the log group name that your function printed to the console.
3. In the Log streams tab, choose the log stream for your function's invocation.

You should see output similar to the following:
```
INIT_START Runtime Version: python:3.12.v16    Runtime Version ARN: arn:aws:lambda:us-west-2::runtime:ca202755c87b9ec2b58856efb7374b4f7b655a0ea3deb1d5acc9aee9e297b072
START RequestId: 9d4096ee-acb3-4c25-be10-8a210f0a9d8e Version: $LATEST
The area is 42
[INFO]	2023-09-01T00:05:22.464Z	9315ab6b-354a-486e-884a-2fb2972b7d84	CloudWatch logs group: /aws/lambda/myLambdaFunction
END RequestId: 9d4096ee-acb3-4c25-be10-8a210f0a9d8e 
REPORT RequestId: 9d4096ee-acb3-4c25-be10-8a210f0a9d8e    Duration: 1.15 ms    Billed Duration: 2 ms    Memory Size: 128 MB    Max Memory Used: 40 MB    
```
When you're finished working with the example function, delete it. You can also delete the log group that stores the function's logs, and the execution role that the console created.



















 













