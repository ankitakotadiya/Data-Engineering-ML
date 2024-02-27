# Amazon Batch
AWS Batch helps you to run batch computing workloads on the AWS Cloud. Batch computing is a common way for developers, scientists, and engineers to access large amounts of compute resources. AWS Batch removes the undifferentiated heavy lifting of configuring and managing the required infrastructure, similar to traditional batch computing software. This service can efficiently provision resources in response to jobs submitted in order to eliminate capacity constraints, reduce compute costs, and deliver results quickly.

As a fully managed service, AWS Batch helps you to run batch computing workloads of any scale. AWS Batch automatically provisions compute resources and optimizes the workload distribution based on the quantity and scale of the workloads. With AWS Batch, there's no need to install or manage batch computing software, so you can focus your time on analyzing results and solving problems.

## Getting Started - Amazon EC2
Amazon Elastic Compute Cloud (Amazon EC2) provides scalable computing capacity in the AWS Cloud. Using Amazon EC2 eliminates your need to invest in hardware up front, so you can develop and deploy applications faster.

You can use Amazon EC2 to launch as many or as few virtual servers as you need, configure security and networking, and manage storage. Amazon EC2 enables you to scale up or down to handle changes in requirements or spikes in popularity, reducing your need to forecast traffic.

### Create a compute environment
1. Open the [AWS Batch console first-run wizard](https://console.aws.amazon.com/batch/home#wizard).
2. For Select orchestration type, choose Amazon Elastic Compute Cloud(Amazon EC2).
3. Choose Next.
4. In the Compute environment configuration section for Name, specify a unique name for your compute environment. The name can be up to 128 characters in length. It can contain uppercase and lowercase letters, numbers, hyphens (-), and underscores (_).
5. For Instance role, choose an existing instance profile that has the required IAM permissions attached. This instance profile allows the Amazon ECS container instances in your compute environment to make calls to the required AWS API operations. For more information.
6. (Optional) A tag is a label that's assigned to a resource. To add a tag or an Amazon EC2 tag, expand Tags, then choose Add tag. Enter a key-value pair, and then choose Add tag again.
7. (Optional) In the Instance configuration section for Use Amazon EC2 Spot instances, turn on Enable using Spotinstances.
8. (Spot only) For Maximum % on-demand price, enter the maximum percentage of On-demand pricing that you want to pay for Spot resources.
9. (Optional) (Spot only) For Spot fleet role, choose an existing Amazon EC2 Spot Fleet IAM role to apply to your Spot compute environment. If you don't already have an existing Amazon EC2 Spot Fleet IAM role, you must create one first. For more information, see Amazon EC2 spot fleet role.
10. For Minimum vCPUs, choose the minimum number of EC2 vCPUs that your compute environment maintains, regardless of job queue demand.
11. For Desired vCPUs, choose the number of EC2 vCPUs that your compute environment launches with. As job queue demand increases, AWS Batch increases the desired number of vCPUs and add EC2 instances. The number of vCPUs can increase up to the maximum number of vCPUs. As demand decreases, AWS Batch decreases the desired number of vCPUs and remove instances. The number of decrease all the way to the minimum number of vCPUs.
12. For Maximum vCPUs, choose the maximum number of EC2 vCPUs that your compute environment can scale out to, regardless of job queue demand.
13. For Allowed instance types, choose the Amazon EC2 instance types that can be launched. You can specify instance families to launch any instance type within those families (for example, c5, c5n, or p3). Or, you can specify specific sizes within a family (such as c5.8xlarge). Metal instance types aren't in the instance families. For example, c5 doesn't include c5.metal. You can also choose optimal to select instance types (from the C4, M4, and R4 instance families) that match the demand of your job queues.
14. Expand Additional configuration.
15. (Optional) For Placement group, enter a placement group name to group resources in the compute environment.
16. (Optional) For EC2 key pair, choose a public and private key pair as security credentials when you connect to the instance. For more information about Amazon EC2 key pairs.
17. For Allocation strategy, choose the allocation strategy to use when selecting instance types from the list of allowed instance types. BEST_FIT_PROGRESSIVE is usually the better choice for EC2 On-Demand compute environments, and SPOT_CAPACITY_OPTIMIZED for EC2 Spot compute environments. For more information.
18. (Optional) For EC2 configuration, choose Add EC2 configuration. Choose Image type and Image ID override values to provide information for AWS Batch to select Amazon Machine Images (AMIs) for instances in the compute environment. If the Image ID override isn't specified for each Image type, AWS Batch selects a recent Amazon ECS optimized AMI. If no Image type is specified, the default is a Amazon Linux 2 for non-GPU, non AWS Graviton instance.
19. (Optional) For Launch template, select an existing Amazon EC2 launch template to configure your compute resources. The default version of the template is automatically populated.
20. (Optional) For Launch template version, enter $Default, $Latest, or a specific version number to use.
21. In the Network configuration section:
    * For Virtual Private Cloud (VPC) ID, choose an Amazon VPC.
    * For Subnets, the subnets for your AWS account are listed. If you want to create a custom set of subnets, choose Clear subnets, and then choose the subnets that you want.
    * For Security groups, choose the Amazon EC2 security groups that you want to associate with the instance. If you want to create a custom set of security groups, choose Clear security groups. Then, choose the security groups that you want.
22. Choose Next.

### Create a job queue
A job queue stores your submitted jobs until the AWS Batch Scheduler runs the job on a resource in your compute environment.
1. In the Job queue configuration section for Name, specify a unique name for your compute environment. The name can be up to 128 characters in length. It can contain uppercase and lowercase letters, numbers, hyphens (-), and underscores (_).
2. For Priority, enter an integer between 0 and 100 for the job queue.
3. Choose Next.

### Create a job definition
AWS Batch job definitions specify how jobs are to be run. Even though each job must reference a job definition, many of the parameters that are specified in the job definition can be overridden at runtime.

1. In the General configuration section:
   * In the General configuration section for Name, specify a unique name for your compute environment. The name can be up to 128 characters in length. The name can contain uppercase and lowercase letters, numbers, hyphens (-), and underscores (_).
   * (Optional) For Execution timeout, enter the amount of time (in seconds) that an unfinished job terminates after.
   * (Optional) A tag is a label that's assigned to a resource. To add a tag, expand Tags, then choose Add tag. Enter a key-value pair, and then choose Add tag again.
   * (Optional) Turn on Propagate tags to propagate tags to the Amazon Elastic Container Service task.
2. In the Container configuration section:
   * For Image, enter the name of the image that's used to launch the container. By default, all the images in the Docker Hub registry are available. You can also specify other repositories in repository-url/image:tag format. The parameter can be up to 255 characters in length. The parameter can contain uppercase and lowercase letters, numbers, hyphens (-), underscores (_), colons (:), periods (.), forward slashes (/), and number signs (#). The parameter maps to Image in the Create a container section of the Docker Remote API and the IMAGE parameter of docker run.
   * For Command, specify the command to pass to the container. This parameter maps to Cmd in the Create a container section of the Docker Remote API and the COMMAND parameter to docker run. For more information about the Docker CMD parameter, see https://docs.docker.com/engine/reference/builder/#cmd.
   * (Optional) For Execution role, specify an IAM role that grants the Amazon ECS container agents permission to make AWS API calls on your behalf. This feature uses Amazon ECS IAM roles for tasks. For more information, see Amazon ECS task execution IAM roles in the Amazon Elastic Container Service Developer Guide.
   * For Parameters, choose Add parameter. Enter a key-value pair and then choose Add parameter again.
   * In the Environment configuration section for vCPUs, specify the number of vCPUs to reserve for the container.
   * For Memory, specify the hard limit (in MiB) of memory to present to the job container. If your container attempts to exceed the memory specified here, the container is stopped.
   * For Number of GPUs, choose the number of GPUs to reserve for the container.
   * (Optional) For Environment variables configuration, choose Add environment variables to add environment variables to pass to the container.
   * (Optional) For Secrets, choose Add secret to add secrets as a name-value pairs.
3. Choose Next.

### Create a job
1. In the Job configuration section for Name, specify a unique name for the job. The name can be up to 128 characters in length. It can contain uppercase and lowercase letters, numbers, hyphens (-), and underscores (_).
2. Choose Next.
3. On the Review and create page, review the configuration steps. If you need to make changes, choose Edit. When you're finished, choose Create resources.

Follow the same steps for EKS and Fargate just select EKS or Fargate orchestration.

## Jobs
Jobs are the unit of work that's started by AWS Batch. Jobs can be invoked as containerized applications that run on Amazon ECS container instances in an ECS cluster.

### Submitting a job
After you register a job definition, you can submit it as a job to an AWS Batch job queue. You can override many of the parameters that are specified in the job definition at runtime.

1. Open the [AWS Batch console](https://console.aws.amazon.com/batch/).
2. From the navigation bar, select the AWS Region to use.
3. In the navigation pane, choose Jobs.
4. Choose Submit new job.
5. For Name, enter a unique name for your job definition. The name can be up to 128 characters in length. It can contain uppercase and lowercase letters, numbers, hyphens (-), and underscores (_).
6. For Job definition, choose an existing job definition for your job. For more information, see Job defination we have created in previous step.
7. For Job queue, choose an existing job queue. For more information, see Creating a job queue in previous steps.
8. For Job dependencies, choose Add Job dependencies.
   * For Job id, enter the job ID for any dependencies. Then choose Add job dependencies. A job can have up to 20 dependencies.
9. (Array jobs only) For Array size, specify an array size between 2 and 10,000.
10. (Optional) Expand Tags and then choose Add tag to add tags to the resource. Enter a key and optional value, then choose Add tag.
11. Choose Next page.
12. In the Job overrides section:
    * (Optional) For Scheduling priority, enter a scheduling priority value between 0 and 100. Higher values are given higher priority.
    * (Optional) For Job attempts, enter the maximum number of times that AWS Batch attempts to move the job to a RUNNABLE status. You can enter a number between 1 and 10. For more information, see [Automated job retries](https://docs.aws.amazon.com/batch/latest/userguide/job_retries.html).
    * (Optional) For Execution timeout, enter the timeout value (in seconds). The execution timeout is the length of time before an unfinished job is terminated. If an attempt exceeds the timeout duration, it's stopped and moves to a FAILED status. For more information, see [Job timeouts](https://docs.aws.amazon.com/batch/latest/userguide/job_timeouts.html). The minimum value is 60 seconds.
    * (Optional) Turn on Propagate tags to propagate tags from the job and job definition to the Amazon ECS task.
13. Expand Additional configuration.
14. (Optional) For Retry strategy conditions, choose Add evaluate on exit. Enter at least one parameter value and then choose an Action. For each set of conditions, Action must be set to either Retry or Exit. These actions mean the following:
    * Retry – AWS Batch retries until the number of job attempts that you specified is reached.
    * Exit – AWS Batch stops retrying the job.
15. For Parameters, choose Add parameters to add parameter substitution placeholders. Then, enter a Key and an optional Value.
16. In the Container overrides section:
    * For Command, specify the command to pass to the container.
    * For vCPUs, enter the number of vCPUs to reserve for the container.
    * For Memory, enter the memory limit that's available to the container.
    * (Optional) For Number of GPUs, choose the number of GPUs to reserve for the container.
    * Optional) For Environment variables, choose Add environment variable to add environment variables as name-value pairs.
    * Choose Next page.
17. For Job review, review the configuration steps. If you need to make changes, choose Edit. When you're finished, choose Create job definition.






























