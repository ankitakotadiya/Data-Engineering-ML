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

## Deploying Lambda functions
You can deploy code to your Lambda function by uploading a zip file archive, or by creating and uploading a container image.

### .zip file archives
A .zip file archive includes your application code and its dependencies. When you author functions using the Lambda console or a toolkit, Lambda automatically creates a .zip file archive of your code.

When you create functions with the Lambda API, command line tools, or the AWS SDKs, you must create a deployment package. You also must create a deployment package if your function uses a compiled language, or to add dependencies to your function. To deploy your function's code, you upload the deployment package from Amazon Simple Storage Service (Amazon S3) or your local machine.

You can upload a .zip file as your deployment package using the Lambda console, AWS Command Line Interface (AWS CLI), or to an Amazon Simple Storage Service (Amazon S3) bucket.

### Container images
You can package your code and dependencies as a container image using tools such as the Docker command line interface (CLI). You can then upload the image to your container registry hosted on Amazon Elastic Container Registry (Amazon ECR).

Additionally, AWS provides a runtime interface emulator for you to test your functions locally using tools such as the Docker CLI.

I have created a demo Lambda [SAM](https://github.com/ankitakotadiya/Data-Engineering/tree/main/sam-app/lambda-python3.11) application.

## CloudFormation
AWS CloudFormation is a service that helps you model and set up your AWS resources so that you can spend less time managing those resources and more time focusing on your applications that run in AWS. You create a template that describes all the AWS resources that you want (like Amazon EC2 instances or Amazon RDS DB instances), and CloudFormation takes care of provisioning and configuring those resources for you. You don't need to individually create and configure AWS resources and figure out what's dependent on what; CloudFormation handles that. 

### Quickly replicate your infrastructure
If your application requires additional availability, you might replicate it in multiple regions so that if one region becomes unavailable, your users can still use your application in other regions. The challenge in replicating your application is that it also requires you to replicate your resources. Not only do you need to record all the resources that your application requires, but you must also provision and configure those resources in each region.

Reuse your CloudFormation template to create your resources in a consistent and repeatable manner. To reuse your template, describe your resources once and then provision the same resources over and over in multiple regions.

### Easily control and track changes to your infrastructure
In some cases, you might have underlying resources that you want to upgrade incrementally. For example, you might change to a higher performing instance type in your Auto Scaling launch configuration so that you can reduce the maximum number of instances in your Auto Scaling group. If problems occur after you complete the update, you might need to roll back your infrastructure to the original settings. To do this manually, you not only have to remember which resources were changed, you also have to know what the original settings were.

When you provision your infrastructure with CloudFormation, the CloudFormation template describes exactly what resources are provisioned and their settings. Because these templates are text files, you simply track differences in your templates to track changes to your infrastructure, similar to the way developers control revisions to source code. For example, you can use a version control system with your templates so that you know exactly what changes were made, who made them, and when. If at any point you need to reverse changes to your infrastructure, you can use a previous version of your template.

### AWS CloudFormation concepts
When you use AWS CloudFormation, you work with templates and stacks. You create templates to describe your AWS resources and their properties. Whenever you create a stack, CloudFormation provisions the resources that are described in your template.

#### Templates
A CloudFormation template is a JSON or YAML formatted text file. You can save these files with any extension, such as .json, .yaml, .template, or .txt. CloudFormation uses these templates as blueprints for building your AWS resources. For example, in a template, you can describe an Amazon EC2 instance, such as the instance type, the AMI ID, block device mappings, and its Amazon EC2 key pair name. Whenever you create a stack, you also specify a template that CloudFormation uses to create whatever you described in the template.

For example, if you created a stack with the following template, CloudFormation provisions an instance with an ami-0ff8a91507f77f867 AMI ID, t2.micro instance type, testkey key pair name, and an Amazon EBS volume.

##### JSON
```
{
    "AWSTemplateFormatVersion": "2010-09-09",
    "Description": "A sample template",
    "Resources": {
        "MyEC2Instance": {
            "Type": "AWS::EC2::Instance",
            "Properties": {
                "ImageId": "ami-0ff8a91507f77f867",
                "InstanceType": "t2.micro",
                "KeyName": "testkey",
                "BlockDeviceMappings": [
                    {
                        "DeviceName": "/dev/sdm",
                        "Ebs": {
                            "VolumeType": "io1",
                            "Iops": 200,
                            "DeleteOnTermination": false,
                            "VolumeSize": 20
                        }
                    }
                ]
            }
        }
    }
}
```

##### YML
```
AWSTemplateFormatVersion: 2010-09-09
Description: A sample template
Resources:
  MyEC2Instance:
    Type: 'AWS::EC2::Instance'
    Properties:
      ImageId: ami-0ff8a91507f77f867
      InstanceType: t2.micro
      KeyName: testkey
      BlockDeviceMappings:
        - DeviceName: /dev/sdm
          Ebs:
            VolumeType: io1
            Iops: 200
            DeleteOnTermination: false
            VolumeSize: 20

```

### Stacks
When you use CloudFormation, you manage related resources as a single unit called a stack. You create, update, and delete a collection of resources by creating, updating, and deleting stacks. All the resources in a stack are defined by the stack's CloudFormation template. Suppose you created a template that includes an Auto Scaling group, Elastic Load Balancing load balancer, and an Amazon Relational Database Service (Amazon RDS) database instance. To create those resources, you create a stack by submitting the template that you created, and CloudFormation provisions all those resources for you.

### Change sets
If you need to make changes to the running resources in a stack, you update the stack. Before making changes to your resources, you can generate a change set, which is a summary of your proposed changes. Change sets allow you to see how your changes might impact your running resources, especially for critical resources, before implementing them.

For example, if you change the name of an Amazon RDS database instance, CloudFormation will create a new database and delete the old one. You will lose the data in the old database unless you've already backed it up. If you generate a change set, you will see that your change will cause your database to be replaced, and you will be able to plan accordingly before you update your stack.




























 













