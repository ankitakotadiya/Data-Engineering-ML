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
























