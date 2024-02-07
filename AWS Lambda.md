# Aws Lambda
AWS Lambda is a compute service that lets you run code without provisioning or managing servers. Lambda runs your code on a high-availability compute infrastructure and performs all of the administration of the compute resources, including server and operating system maintenance, capacity provisioning and automatic scaling, and logging. With Lambda, all you need to do is supply your code in one of the language runtimes that Lambda supports.

To get started with Lambda, use the Lambda console to create a function. In a few minutes, you can create and deploy a function and test it in the console. To keep things simple, you create your function using either the Python or Node.js runtime. With these interpreted languages, you can edit function code directly in the console's built-in code editor. With compiled languages like Java and C#, you need to create a deployment package on your local build machine and upload it to Lambda.

## Create a Lambda function with the console
In this example, your function takes a JSON object containing two integer values labeled "length" and "width". The function multiplies these values to calculate an area and returns this as a JSON string. Your function also prints the calculated area.

1. Open the [Functions page](https://console.aws.amazon.com/lambda/home#/functions) of the Lambda console.











