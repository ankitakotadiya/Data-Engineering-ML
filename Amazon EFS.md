# Amazon EFS
Amazon Elastic File System (Amazon EFS) provides serverless, fully elastic file storage so that you can share file data without provisioning or managing storage capacity and performance. Amazon EFS is built to scale on demand to petabytes without disrupting applications, growing and shrinking automatically as you add and remove files. Because Amazon EFS has a simple web services interface, you can create and configure file systems quickly and easily. The service manages all the file storage infrastructure for you, meaning that you can avoid the complexity of deploying, patching, and maintaining complex file system configurations.

Amazon EFS supports the Network File System version 4 (NFSv4.1 and NFSv4.0) protocol, so the applications and tools that you use today work seamlessly with Amazon EFS. Amazon EFS is accessible across most types of Amazon Web Services compute instances, including Amazon EC2, Amazon ECS, Amazon EKS, AWS Lambda, and AWS Fargate.

The service is designed to be highly scalable, highly available, and highly durable. Amazon EFS offers the following file system types to meet your availability and durability needs:
* Regional (Recommended) – Regional file systems provide continuous availability to data, even when one or more Availability Zones in an AWS Region are unavailable. Regional file systems offer the highest levels of availability and durability by storing file system data and metadata redundantly across multiple geographically separated Availability Zones within an AWS Region.
* One Zone – One Zone file systems provide continuous availability to data within a single Availability Zone in an AWS Region.

To access your Amazon EFS file system in a VPC, you create one or more mount targets in the VPC.
* For Regional file systems, you can create a mount target in each Availability Zone in the AWS Region.
* For One Zone file systems, you create only a single mount target that is in the same Availability Zone as the file system.

The following illustration shows multiple EC2 instances accessing an Amazon EFS file system that is configured for multiple Availability Zones in an AWS Region.

![efs-ec2-how-it-works-Regional_china-world](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/c7478e95-aad6-491c-a1fd-306b259e3293)

The following illustration shows multiple EC2 instances accessing a One Zone file system from different Availability Zones in a single AWS Region.

![efs-ec2-how-it-works-OneZone](https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/ba2114cc-ee0f-4872-b50e-bc33b5013b6c)

For a comprehensive backup implementation for your file systems, you can use Amazon EFS with AWS Backup. AWS Backup is a fully managed backup service that makes it easy to centralize and automate data backup across AWS services in the cloud and on-premises. Using AWS Backup, you can centrally configure backup policies and monitor backup activity for your AWS resources. Amazon EFS always prioritizes file system operations over backup operations. To learn more about backing up EFS file systems using AWS Backup, see Backing up your Amazon EFS file systems.

### Step 1: Create your Amazon EFS file system
1. Sign in to the AWS Management Console and open the [Amazon EFS console](https://console.aws.amazon.com/efs/).
2. Choose Create file system to open the Create file system dialog box.
3. (Optional) Enter a Name for your file system.
4. For Virtual Private Cloud (VPC), choose your VPC, or keep it set to your default VPC.
5. Choose Create to create a file system that uses the following service recommended settings:
   * Automatic backups enabled.
   * Mount targets configured with the following settings:
     * Created in each Availability Zone in the AWS Region in which the file system is created.
     * Located in the default subnets of the VPC you selected.
     * Using the VPC's default security group – You can manage security groups after the file system is the created.
   * lifecycle management – Amazon EFS creates the file system with the following lifecycle policies:
     * Transition into IA set to 30 days since last access.
     * TransitionToArchive set to 90 days since last access.
     * Transition into Standard set to None.

After you create the file system, you can customize the file system's settings with the exception of availability and durability, encryption, and performance mode.

### Step 2: Create your EC2 resources and launch your EC2 instance
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. Choose Launch Instance.
3. In Step 1: Choose an Amazon Machine Image (AMI), find an Amazon Linux 2 AMI at the top of the list and choose Select.
4. In Step 2: Choose an Instance Type, choose Next: Configure Instance Details.
5. In Step 3: Configure Instance Details, provide the following information:
   * Leave Number of instances at one.
   * Leave Purchasing option at the default setting.
   * For Network, choose the entry for the same VPC that you noted when you created your EFS file system in Step 1: Create your Amazon EFS file system.
   * For Subnet, choose a default subnet in any Availability Zone.
   * For File systems, make sure that the EFS file system that you created in Step 1: Create your Amazon EFS file system is selected. The path shown next to the file system ID is the mount point that the EC2 instance will use, which you can change.
   * The User data automatically includes the commands for mounting your Amazon EFS file system.
6. Choose Next: Add Storage.
7. Choose Next: Add Tags.
8. Name your instance and choose Next: Configure Security Group.
9. In Step 6: Configure Security Group, set Assign a security group to Select an existing security group. Choose the default security group to make sure that it can access your EFS file system.
10. Choose Review and Launch.
11. Choose Launch.
















