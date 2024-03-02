# Amazon Container Service

## Amazon Elastic Container Service
Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service that helps you easily deploy, manage, and scale containerized applications. As a fully managed service, Amazon ECS comes with AWS configuration and operational best practices built-in. It's integrated with both AWS and third-party tools, such as Amazon Elastic Container Registry and Docker. This integration makes it easier for teams to focus on building the applications, not the environment. You can run and scale your container workloads across AWS Regions in the cloud, and on-premises, without the complexity of managing a control plane.

### Amazon ECS capacity
Amazon ECS capacity is the infrastructure where your containers run. The following is an overview of the capacity options:
* Amazon EC2 instances in the AWS cloud. You choose the instance type, the number of instances, and manage the capacity.
* Serverless (AWS Fargate (Fargate)) in the AWS cloud. Fargate is a serverless, pay-as-you-go compute engine. With Fargate you don't need to manage servers, handle capacity planning, or isolate container workloads for security.
* On-premises virtual machines (VM) or servers. Amazon ECS Anywhere provides support for registering an external instance such as an on-premises server or virtual machine (VM), to your Amazon ECS cluster.

### The capacity can be located in any of the following AWS resources:
* Availability Zones
* Local Zones
* Wavelength Zones
* AWS Regions
* AWS Outposts

You must architect your applications so that they can run on containers. A container is a standardized unit of software development that holds everything that your software application requires to run. This includes relevant code, runtime, system tools, and system libraries. Containers are created from a read-only template that's called an image. Images are typically built from a Dockerfile. A Dockerfile is a plaintext file that specifies all of the components that are included in the container. After they're built, these images are stored in a registry such as Amazon ECR where they can be downloaded from.

After you create and store your image, you create an Amazon ECS task definition. A task definition is a blueprint for your application. It is a text file in JSON format that describes the parameters and one or more containers that form your application. For example, you can use it to specify the image and parameters for the operating system, which containers to use, which ports to open for your application, and what data volumes to use with the containers in the task. The specific parameters available for your task definition depend on the needs of your specific application.

![ecs-lifecycle](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/ebeb7800-6b99-44bf-950d-6bd448f97c5d)

### Create a Docker image
Amazon ECS task definitions use Docker images to launch containers on the container instances in your clusters. In this section, you create a Docker image of a simple web application, and test it on your local system or Amazon EC2 instance, and then push the image to the Amazon ECR container registry so you can use it in an Amazon ECS task definition.

1. Create a file called Dockerfile. A Dockerfile is a manifest that describes the base image to use for your Docker image and what you want installed and running on it.
   ```
   touch Dockerfile
   ```
2. Edit the Dockerfile you just created and add the following content.
   ```
    FROM public.ecr.aws/amazonlinux/amazonlinux:latest

    # Install dependencies
    RUN yum update -y && \
     yum install -y httpd
    
    # Install apache and write hello world message
    RUN echo 'Hello World!' > /var/www/html/index.html
    
    # Configure apache
    RUN echo 'mkdir -p /var/run/httpd' >> /root/run_apache.sh && \
     echo 'mkdir -p /var/lock/httpd' >> /root/run_apache.sh && \
     echo '/usr/sbin/httpd -D FOREGROUND' >> /root/run_apache.sh && \
     chmod 755 /root/run_apache.sh
    
    EXPOSE 80
    
    CMD /root/run_apache.sh
   ```
3. Build the Docker image from your Dockerfile.
   ```
   docker build -t hello-world .
   ```
4. List your container image.
   ```
   docker images --filter reference=hello-world
   ```

   ```
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   hello-world         latest              e9ffedc8c286        4 minutes ago       194MB
   ```

### Push your image to Amazon Elastic Container Registry
1. Create an Amazon ECR repository to store your hello-world image. Note the repositoryUri in the output.

  ```
  aws ecr create-repository --repository-name hello-repository --region region
  ```
2. Tag the hello-world image with the repositoryUri value from the previous step.
   ```
   docker tag hello-world aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```
3. Run the aws ecr get-login-password command. Specify the registry URI you want to authenticate to. For more information, see Registry Authentication in the Amazon Elastic Container Registry User Guide.
   ```
   aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```
4. Push the image to Amazon ECR with the repositoryUri value from the earlier step.
   ```
   docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```










