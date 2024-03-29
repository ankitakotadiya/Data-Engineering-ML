# Amazon API Gateway
Amazon API Gateway is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at any scale. API developers can create APIs that access AWS or other web services, as well as data stored in the AWS Cloud. As an API Gateway API developer, you can create APIs for use in your own client applications. Or you can make your APIs available to third-party app developers.
API Gateway is an AWS service that supports the following:
* Creating, deploying, and managing a RESTful application programming interface (API) to expose backend HTTP endpoints, AWS Lambda functions, or other AWS services.
* Creating, deploying, and managing a WebSocket API to expose AWS Lambda functions or other AWS services.
* Invoking exposed API methods through the frontend HTTP and WebSocket endpoints.

First, you create a Lambda function using the AWS Lambda console. Next, you create an HTTP API using the API Gateway console. Then, you invoke your API.
## Step 1: Create a Lambda function
You use a Lambda function for the backend of your API. Lambda runs your code only when needed and scales automatically, from a few requests per day to thousands per second.

### To create a Lambda function
1. Sign in to the [Lambda console](https://console.aws.amazon.com/lambda).
2. Choose Create function.
3. For Function name, enter my-function.
4. Choose Create function.

The example function returns a 200 response to clients, and the text Hello from Lambda!.
<img width="726" alt="Screenshot 2024-02-10 at 1 19 48 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/0e1277bf-7869-4b83-ae18-ab3d4c372293">

## Step 2: Create an HTTP API
Next, you create an HTTP API. API Gateway also supports REST APIs and WebSocket APIs, but an HTTP API is the best choice for this exercise. REST APIs support more features than HTTP APIs, but we don't need those features for this exercise. HTTP APIs are designed with minimal features so that they can be offered at a lower price. WebSocket APIs maintain persistent connections with clients for full-duplex communication, which isn't required for this example.

The HTTP API provides an HTTP endpoint for your Lambda function. API Gateway routes requests to your Lambda function, and then returns the function's response to clients.
### To create an HTTP API
1. Sign in to the [API Gateway console](https://console.aws.amazon.com/apigateway).
2. To create your first API, for HTTP API, choose Build.
   <img width="824" alt="Screenshot 2024-02-10 at 1 02 28 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/908fe68f-d2c6-4d0c-9745-6b546912f5f7">
3. For Integrations, choose Add integration. Choose Lambda. For Lambda function, enter my-function.
   <img width="1126" alt="Screenshot 2024-02-10 at 1 03 29 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/346a67ab-8bc5-4ce8-ac80-391d27981601">
4. For API name, enter my-http-api.
5. Choose Next.
6. Review the route that API Gateway creates for you, and then choose Next.
   <img width="1135" alt="Screenshot 2024-02-10 at 1 05 10 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/648db7d5-22e3-4b56-851c-6c382d2404e5">
7. Review the stage that API Gateway creates for you, and then choose Next.
    <img width="1128" alt="Screenshot 2024-02-10 at 1 06 57 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/69cb9572-b64f-470f-84be-10385e6bae3f">
8. Choose Create.
   <img width="1067" alt="Screenshot 2024-02-10 at 1 07 43 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/70c066f8-7a8f-47e8-bf6f-c9641d29103a">

## Step 3: Test your API
Next, you test your API to make sure that it's working. For simplicity, use a web browser to invoke your API.

### To test your API
1. Sign in to the [API Gateway console](https://console.aws.amazon.com/apigateway).
2. Choose your API.
   <img width="1080" alt="Screenshot 2024-02-10 at 1 10 11 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/e780e077-e5aa-45ae-b7c3-bae0404e5a63">
3. Copy your API's invoke URL, and enter it in a web browser. Append the name of your Lambda function to your invoke URL to call your Lambda function. By default, the API Gateway console creates a route with the same name as your Lambda function, my-function.
   The full URL should look like https://abcdef123.execute-api.us-east-2.amazonaws.com/my-function.
   <img width="623" alt="Screenshot 2024-02-10 at 1 10 47 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/9bf8b3d3-4a31-472b-b613-99d0bfac3e3e">

## (Optional) Step 4: Clean up
To prevent unnecessary costs, delete the resources that you created as part of this getting started exercise. The following steps delete your HTTP API, your Lambda function, and associated resources.

### To delete an HTTP API
1. Sign in to the [API Gateway console](https://console.aws.amazon.com/apigateway).
2. On the APIs page, select an API. Choose Actions, and then choose Delete.
3. Choose Delete.

### To delete a Lambda function
1. Sign in to the [Lambda console](https://console.aws.amazon.com/lambda).
2. On the Functions page, select a function. Choose Actions, and then choose Delete.
3. Choose Delete.

### To delete a Lambda function's log group
1. In the Amazon CloudWatch console, open the [Log groups page](https://console.aws.amazon.com/cloudwatch/home#logs:).
2. On the Log groups page, select the function's log group (/aws/lambda/my-function). Choose Actions, and then choose Delete log group.
3. Choose Delete.

### To delete a Lambda function's execution role
1. In the AWS Identity and Access Management console, open the [Roles page](https://console.aws.amazon.com/iam/home?#/roles).
2. Select the function's role, for example, my-function-31exxmpl.
3. Choose Delete role.

## Expose GET on the API's root resource to list all of the Amazon S3 buckets of a caller.

### Set up IAM permissions for the API to invoke Amazon S3 actions
To allow the API to invoke Amazon S3 actions, you must have the appropriate IAM policies attached to an IAM role.
1. Sign in to the AWS Management Console and open the [IAM console](https://console.aws.amazon.com/iam/).
2. Choose Roles.
3. Choose Create role.
4. Choose AWS service under Select type of trusted entity, and then select API Gateway and select Allows API Gateway to push logs to CloudWatch Logs.
5. Choose Next, and then choose Next.
6. For Role name, enter APIGatewayS3ProxyPolicy, and then choose Create role.
7. In the Roles list, choose the role you just created. You may need to scroll or use the search bar to find the role.
8. For the selected role, select the Add permissions tab.
9. Choose Attach policies from the dropdown list.
10. In the search bar, enter AmazonS3FullAccess and choose Add permissions.
    
<img width="1151" alt="Screenshot 2024-02-11 at 1 40 43 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/13a35853-b91b-4894-9e9c-0ead249aa905">

### Create API resources to represent Amazon S3 resources
1. In the same AWS Region you created your Amazon S3 bucket, create an API named MyS3. This API's root resource (/) represents the Amazon S3 service. In this step, you create two additional resources /{folder} and /{item}.
2. Select the API's root resource, and then choose Create resource.
3. Keep Proxy resource turned off.
4. For Resource path, select /.
5. For Resource name, enter {folder}.
6. Keep CORS (Cross Origin Resource Sharing) unchecked.
7. Choose Create resource.
8. Select the /{folder} resource, and then choose Create resource.
9. Use the previous steps to create a child resource of /{folder} named {item}.

<img width="1085" alt="Screenshot 2024-02-11 at 12 14 56 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/86f335af-860e-4de3-b4e3-1b2717d9b65f">

### Expose an API method to list the caller's Amazon S3 buckets
1. Select the / resource, and then choose Create method.
2. For method type, select GET.
3. For Integration type, select AWS service.
4. For AWS Region, select the AWS Region where you created your Amazon S3 bucket.
5. For AWS service, select Amazon Simple Storage Service.
6. Keep AWS subdomain blank.
7. For HTTP method, select GET.
8. For Action type, select Use path override.
9. For Path override, enter /.
10. For Execution role, enter the role ARN for APIGatewayS3ProxyPolicy.
11. Choose Create method.

#### To test the GET / method
1. Choose the Test tab. You might need to choose the right arrow button to show the tab.
2. Choose Test. The result should look like the following image:
   
<img width="1123" alt="Screenshot 2024-02-11 at 1 50 48 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/14a71ea9-c2ed-4dbc-a35c-4af323175830">

## Expose API methods to access an Amazon S3 bucket
1. Select the /{folder} resource, and then choose Create method.
2. For method type, select GET.
3. For Integration type, select AWS service.
4. For AWS Region, select the AWS Region where you created your Amazon S3 bucket.
5. For AWS service, select Amazon Simple Storage Service.
6. Keep AWS subdomain blank.
7. For HTTP method, select GET.
8. For Action type, select Use path override.
9. For Path override, enter {bucket}.
10. For Execution role, enter the role ARN for APIGatewayS3ProxyPolicy.
11. Choose Create method.

You set the {folder} path parameter in the Amazon S3 endpoint URL. You need to map the {folder} path parameter of the method request to the {bucket} path parameter of the integration request.

### To map {folder} to {bucket}
1. On the Integration request tab, under Integration request settings, choose Edit.
2. Choose URL path parameters, and then choose Add path parameter.
3. For Name, enter bucket.
4. For Mapped from, enter method.request.path.folder. The configuration should look similar to the following:

<img width="642" alt="Screenshot 2024-02-11 at 1 59 32 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/a7594009-dae4-4c60-b266-40cbd029c599">
5. Choose Save.

### To test the /{folder} GET method.
1. Choose the Test tab. You might need to choose the right arrow button to show the tab.
2. Under Path, for folder, enter the name of your bucket.
3. Choose Test.

<img width="1139" alt="Screenshot 2024-02-11 at 2 01 46 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/6d71125e-bad2-4735-8bf1-a282b9d32cc5">

## Expose API methods to access an Amazon S3 object in a bucket
Amazon S3 supports GET, DELETE, HEAD, OPTIONS, POST and PUT actions to access and manage objects in a given bucket. In this tutorial, you expose a GET method on the {folder}/{item} resource to get an image from a bucket. For more applications of the {folder}/{item} resource.

1. Select the /{item} resource, and then choose Create method.
2. For method type, select GET.
3. For Integration type, select AWS service.
4. For AWS Region, select the AWS Region where you created your Amazon S3 bucket.
5. For AWS service, select Amazon Simple Storage Service.
6. Keep AWS subdomain blank.
7. For HTTP method, select GET.
8. For Action type, select Use path override.
9. For Path override, enter {bucket}/{object}.
10. For Execution role, enter the role ARN for APIGatewayS3ProxyPolicy.
11. Choose Create method.

You set the {folder} and {item} path parameters in the Amazon S3 endpoint URL. You need to map the path parameter of the method request to the path parameter of the integration request.

### To map {folder} to {bucket} and {item} to {object}
1. On the Integration request tab, under Integration request settings, choose Edit.
2. Choose URL path parameters.
3. Choose Add path parameter.
4. For Name, enter bucket.
5. For Mapped from, enter method.request.path.folder
6. Choose Add path parameter.
7. For Name, enter object.
8. For Mapped from, enter method.request.path.item.
9. Choose Save.

### To test the /{folder}/{object} GET method.
1. Choose the Test tab. You might need to choose the right arrow button to show the tab.
2. Under Path, for folder, enter the name of your bucket.
3. Under Path, for item, enter the name of an item.
4. Choose Test.

The response body will contain the contents of the item.
<img width="1144" alt="Screenshot 2024-02-11 at 2 12 34 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/5fc99fdb-b223-4f58-aa3a-e12da82f62d8">
<img width="1135" alt="Screenshot 2024-02-11 at 2 13 29 PM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/0ded49d1-8007-4716-9aea-a87a273ce1fb">















































