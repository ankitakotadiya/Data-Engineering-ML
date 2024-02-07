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


 













