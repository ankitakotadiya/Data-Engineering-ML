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
3. Tag the hello-world image with the repositoryUri value from the previous step.
   ```
   docker tag hello-world aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```
4. Run the aws ecr get-login-password command. Specify the registry URI you want to authenticate to. For more information, see Registry Authentication in the Amazon Elastic Container Registry User Guide.
   ```
   aws ecr get-login-password --region region | docker login --username AWS --password-stdin aws_account_id.dkr.ecr.region.amazonaws.com
   ```
5. Push the image to Amazon ECR with the repositoryUri value from the earlier step.
   ```
   docker push aws_account_id.dkr.ecr.region.amazonaws.com/hello-repository
   ```

### Step 1: Create the cluster
1. Open the [console](https://console.aws.amazon.com/ecs/v2).
2. From the navigation bar, select the Region to use.
3. In the navigation pane, choose Clusters.
4. On the Clusters page, choose Create cluster.
5. Under Cluster configuration, for Cluster name, enter a unique name.
6. (Optional) To turn on Container Insights, expand Monitoring, and then turn on Use Container Insights.
7. (Optional) To help identify your cluster, expand Tags, and then configure your tags.
8. Choose Create.

### Step 2: Create a task definition
A task definition is like a blueprint for your application. Each time you launch a task in Amazon ECS, you specify a task definition. The service then knows which Docker image to use for containers, how many containers to use in the task, and the resource allocation for each container.

1. In the navigation pane, choose Task Definitions.
2. Choose Create new Task Definition, Create new revision with JSON.
3. Copy and paste the following example task definition into the box and then choose Save.
   ```
   {
    "family": "sample-fargate", 
    "networkMode": "awsvpc", 
    "containerDefinitions": [
        {
            "name": "fargate-app", 
            "image": "public.ecr.aws/docker/library/httpd:latest", 
            "portMappings": [
                {
                    "containerPort": 80, 
                    "hostPort": 80, 
                    "protocol": "tcp"
                }
            ], 
            "essential": true, 
            "entryPoint": [
                "sh",
		"-c"
            ], 
            "command": [
                "/bin/sh -c \"echo '<html> <head> <title>Amazon ECS Sample App</title> <style>body {margin-top: 40px; background-color: #333;} </style> </head><body> <div style=color:white;text-align:center> <h1>Amazon ECS Sample App</h1> <h2>Congratulations!</h2> <p>Your application is now running on a container in Amazon ECS.</p> </div></body></html>' >  /usr/local/apache2/htdocs/index.html && httpd-foreground\""
            ]
        }
    ], 
    "requiresCompatibilities": [
        "FARGATE"
    ], 
    "cpu": "256", 
    "memory": "512"
   }
   ```
4. Choose Create.

### Step 3: Create the service

1. In the navigation pane, choose Clusters, and then select the cluster you created in Step 1: Create the cluster.
2. From the Services tab, choose Create.
3. Under Deployment configuration, specify how your application is deployed.
   * For Task definition, choose the task definition you created in Step 2: Create a task definition.
   * For Service name, enter a name for your service.
   * For Desired tasks, enter 1.
4. Under Networking, you can create a new security group or choose an existing security group for your task. Ensure that the security group you use has the inbound rule listed under Prerequisites.
5. Choose Create.

### Step 4: View your service
1. Open the [console](https://console.aws.amazon.com/ecs/v2).
2. In the navigation pane, choose Clusters.
3. Choose the cluster where you ran the service.
4. In the Services tab, under Service name, choose the service you created in Step 3: Create the service.
5. Choose the Tasks tab, and then choose the task in your service.
6. On the task page, in the Configuration section, under Public IP, choose Open address.

### Create Cluster using EC2
1. To add Amazon EC2 instances to your cluster, expand Infrastructure, and then select Amazon EC2 instances. Next, configure the Auto Scaling group which acts as the capacity provider:
   * To using an existing Auto Scaling group, from Auto Scaling group (ASG), select the group.
   * To create a Auto Scaling group, from Auto Scaling group (ASG), select Create new group.
   * For Operating system/Architecture, choose the Amazon ECS-optimized AMI for the Auto Scaling group instances.
   * For EC2 instance type, choose the instance type for your workloads. For more information about the different instance types, see Amazon EC2 Instances.
   * Managed scaling works best if your Auto Scaling group uses the same or similar instance types.
   * For SSH key pair, choose the pair that proves your identity when you connect to the instance.
   * For Capacity, enter the minimum number and the maximum number of instances to launch in the Auto Scaling group. Amazon EC2 instances incur costs while they exist in your AWS resources.
   * Apply additional settings based on your requirements and choose create cluster.

## Amazon EKS
Amazon Elastic Kubernetes Service (Amazon EKS) is a managed service that eliminates the need to install, operate, and maintain your own Kubernetes control plane on Amazon Web Services (AWS). Kubernetes is an open-source system that automates the management, scaling, and deployment of containerized applications.

### Features of Amazon EKS
* Secure networking and authentication
* Easy cluster scaling
* Managed Kubernetes experience
* High availability
* Integration with AWS services

### Get started with Amazon EKS
* Create a cluster – Start by creating your cluster using eksctl, AWS Management Console, AWS CLI, or one of the AWS SDKs.
* Choose your approach to compute resources – Decide between AWS Fargate, Karpenter, managed node groups, and self-managed nodes.
* Setup – Set up the necessary controllers, drivers, and services.
* Deploy workloads – Tailor your Kubernetes workloads to best utilize the resources and capabilities of your chosen node type.
* Management – Oversee your workloads, integrating AWS services to streamline operations and enhance workload performance. You can view information about your workloads using the AWS Management Console.

<img width="839" alt="what-is-eks" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/68df3dff-1a6f-4922-a66b-fcd7b3da4cab">

### Setting up to use Amazon EKS
1. Generate Security credentials for IAM user.
2. Configure AWS CLI.
3. Installing or updating kubectl

```
curl -O https://s3.us-west-2.amazonaws.com/amazon-eks/1.29.0/2024-01-04/bin/darwin/amd64/kubectl

#Apply execute permissions to the binary.
chmod +x ./kubectl

#Copy the binary to a folder in your PATH. If you have already installed a version of kubectl, then we recommend creating a $HOME/bin/kubectl and ensuring that $HOME/bin comes first in your $PATH.
mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH

#(Optional) Add the $HOME/bin path to your shell initialization file so that it is configured when you open a shell.
echo 'export PATH=$HOME/bin:$PATH' >> ~/.bash_profile
```

When first installing kubectl, it isn't yet configured to communicate with any server. We will cover this configuration as needed in other procedures. If you ever need to update the configuration to communicate with a particular cluster, you can run the following command. Replace region-code with the AWS Region that your cluster is in. Replace my-cluster with the name of your cluster.

```
aws eks update-kubeconfig --region region-code --name my-cluster

```

kubectl – A command line tool for working with Kubernetes clusters.
eksctl – A command line tool for working with EKS clusters that automates many individual tasks. 

### Create your Amazon EKS cluster and nodes
```
eksctl create cluster --name my-cluster --region region-code --fargate

```

### To create your cluster from AWS Console
1. Open the [Amazon EKS console](https://console.aws.amazon.com/eks/home#/clusters).
2. Choose Add cluster, and then choose Create. If you don't see this option, then choose Clusters in the left navigation pane first.
3. On the Configure cluster page, do the following:
   * Enter a Name for your cluster, such as my-cluster.
   * For Cluster Service Role, choose myAmazonEKSClusterRole.
   * Leave the remaining settings at their default values and choose Next.
4. On the Specify networking page, do the following:
   * Choose the ID of the VPC that you created in a previous step from the VPC dropdown list. It is something like vpc-00x0000x000x0x000 | my-eks-vpc-stack-VPC.
   * Leave the remaining settings at their default values and choose Next.
5. On the Configure observability page, choose Next.
6. On the Select add-ons page, choose Next.
7. On the Configure selected add-ons settings page, choose Next.
8. On the Review and create page, choose Create.

### To create Nodes
1. Open the [Amazon EKS console](https://console.aws.amazon.com/eks/home#/clusters).
2. On the Clusters page, choose the my-cluster cluster.
3. On the my-cluster page, do the following:
   * Choose the Compute tab.
   * Under Fargate Profiles, choose Add Fargate Profile.
4. On the Configure Fargate Profile page, do the following:
   * For Name, enter a unique name for your Fargate profile, such as my-profile.
   * For Pod execution role, choose the AmazonEKSFargatePodExecutionRole that you created in a previous step.
   * Choose the Subnets dropdown and deselect any subnet with Public in its name. Only private subnets are supported for Pods that are running on Fargate.
   * Choose Next.
5. On the Configure Pod selection page, do the following:
   * For Namespace, enter default.
   * Choose Next.
6. On the Review and create page, review the information for your Fargate profile and choose Create.
7. After a few minutes, the Status in the Fargate Profile configuration section will change from Creating to Active. Don't continue to the next step until the status is Active.



































