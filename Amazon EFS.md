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


## Amazon FSx
The Amazon FSx family of services makes it easy to launch, run, and scale shared storage powered by popular commercial and open-source file systems. You can use the new launch instance wizard to automatically attach the following types of Amazon FSx file systems to your Amazon EC2 instances at launch:

* Amazon FSx for NetApp ONTAP provides fully managed shared storage in the AWS Cloud with the popular data access and management capabilities of NetApp ONTAP.
* Amazon FSx for OpenZFS provides fully managed cost-effective shared storage powered by the popular OpenZFS file system.

### Step 1: Create your file system
1. Open the Amazon FSx console at https://console.aws.amazon.com/fsx/.
2. On the dashboard, choose Create file system to start the file system creation wizard.
3. On the Select file system type page, choose FSx for Windows File Server, and then choose Next. The Create file system page appears.
4. In the File system details section, provide a name for your file system. It's easier to find and manage your file systems when you name them. You can use a maximum of 256 Unicode letters, white space, and numbers, plus the special characters + - = . _ : /

<img width="606" alt="CreateFSxWindow-details" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/3d0517c0-6303-4c7c-bd73-4ecee7c34efe">

5. For Deployment type choose Multi-AZ or Single-AZ.
   * Choose Multi-AZ to deploy a file system that is tolerant to Availability Zone unavailability. This option supports SSD and HDD storage.
   * Choose Single-AZ to deploy a file system that is deployed in a single Availability Zone. Single-AZ 2 is the latest generation of single Availability Zone file systems, and it supports SSD and HDD storage.
6. For Storage type, you can choose either SSD or HDD.
7. For Provisioned SSD IOPS, you can choose either Automatic or User-provisioned mode.
8. For Storage capacity, enter the storage capacity of your file system, in GiB. If you're using SSD storage, enter any whole number in the range of 32–65,536. If you're using HDD storage, enter any whole number in the range of 2,000–65,536. You can increase the amount of storage capacity as needed at any time after you create the file system. For more information, see Managing storage capacity.
9. Keep Throughput capacity at its default setting. Throughput capacity is the sustained speed at which the file server that hosts your file system can serve data. The Recommended throughput capacity setting is based on the amount of storage capacity you choose. If you need more than the recommended throughput capacity, choose Specify throughput capacity, and then choose a value. For more information, see FSx for Windows File Server performance.
10. In the Network & security section, choose the Amazon VPC that you want to associate with your file system. For this getting started exercise, choose the same Amazon VPC that you chose for your AWS Directory Service directory and your Amazon EC2 instance.
11. For VPC Security Groups, the default security group for your default Amazon VPC is already added to your file system in the console. If you're not using the default security group, make sure that the security group you choose is in the same AWS Region as your file system.
12. If you have a Multi-AZ deployment (see step 5), choose a Preferred subnet value for the primary file server and a Standby subnet value for the standby file server. A Multi-AZ deployment has a primary and a standby file server, each in its own Availability Zone and subnet.
13. For Windows authentication, you have the following options:
    If you want to join your file system to a Microsoft Active Directory domain that is managed by AWS, choose AWS Managed Microsoft Active Directory, and then choose your AWS Directory Service directory from the list.
14. For Encryption, keep the default Encryption key setting of aws/fsx (default).
15. For Auditing - optional, file access auditing is disabled by default. For information about enabling and configuring file access auditing.
16. For Access - optional, enter any DNS aliases that you want to associate with the file system. Each alias name must be formatted as a fully qualified domain name (FQDN).
17. For Backup and maintenance - optional, keep the default settings.
18. For Tags - optional, enter a key and value to add tags to your file system. A tag is a case-sensitive key-value pair that helps you manage, filter, and search for your file system.
19. Review the file system configuration shown on the Create file system page. For your reference, note which file system settings you can modify after file system is created. Choose Create file system.
20. After Amazon FSx creates the file system, choose the file system ID in the File Systems dashboard. Choose Attach, and note the fully qualified domain name for your file system. You will need it in a later step.

### Step 2: Map your file share to an EC2 instance running Windows Server
1. Before you can mount a file share on a Windows instance, you must launch the EC2 instance and join it to an AWS Directory Service for Microsoft Active Directory. To perform this action, choose one of the following procedures from the AWS Directory Service Administration Guide:
   * [Seamlessly Join a Windows EC2 Instance](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/launching_instance.html)
   * [Manually Join a Windows Instance](https://docs.aws.amazon.com/directoryservice/latest/admin-guide/join_windows_instance.html)
2. Connect to your instance.
3. When you're connected, open File Explorer.
4. From the navigation pane, open the context (right-click) menu for Network and choose Map Network Drive.
5. Choose a drive letter of your choice for Drive.
6. You can map your file system using either its default DNS name assigned by Amazon FSx, or using a DNS alias of your choosing. This procedure describes mapping a file share using the default DNS name.
7. Choose whether the file share should Reconnect at sign-in, and then choose Finish.

### Step 3: Write data to your file share
1. Open the Notepad text editor.
2. Write some content in the text editor. For example: Hello, World!
3. Save the file to your file share's drive letter.
4. Using File Explorer, navigate to your file share and find the text file that you just saved.

### Step 4: Back up your file system
1. Open the Amazon FSx console at https://console.aws.amazon.com/fsx/.
2. From the console dashboard, choose the name of the file system you created for this exercise.
3. From the Overview tab for your file system, choose Create backup.
4. In the Create backup dialog box that opens, provide a name for your backup. This name can contain a maximum of 256 Unicode letters and include white space, numbers, and the following special characters: + - = . _ : /
5. Choose Create backup.
6. To view all your backups in a list, so you can restore your file system or delete the backup, choose Backups.





      
    






















