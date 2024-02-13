# AWS S3
Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can use Amazon S3 to store and protect any amount of data for a range of use cases, such as data lakes, websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics. Amazon S3 provides management features so that you can optimize, organize, and configure access to your data to meet your specific business, organizational, and compliance requirements.

Amazon S3 offers a range of storage classes designed for different use cases. For example, you can store mission-critical production data in S3 Standard or S3 Express One Zone for frequent access, save costs by storing infrequently accessed data in S3 Standard-IA or S3 One Zone-IA, and archive data at the lowest costs in S3 Glacier Instant Retrieval, S3 Glacier Flexible Retrieval, and S3 Glacier Deep Archive.

## S3 Storage Class
Amazon S3 (Simple Storage Service) provides several storage classes, each designed to optimize cost, performance, and durability based on different data access patterns and retention requirements. Here's an overview of the commonly used Amazon S3 storage classes:

| Storage class                                 | Designed for                                                                                                          | Availability Zones | Min storage duration | Min billable object size | Monitoring and auto-tiering fees            | Retrieval fees    |   |
|-----------------------------------------------|-----------------------------------------------------------------------------------------------------------------------|--------------------|----------------------|--------------------------|---------------------------------------------|-------------------|---|
| Standard                                      | Frequently accessed data (more than once a month) with milliseconds access                                            | ≥ 3                | -                    | -                        | -                                           | -                 |   |
| Intelligent-Tiering                           | Data with changing or unknown access patterns                                                                         | ≥ 3                | -                    | -                        | Per-object fees apply for objects >= 128 KB | -                 |   |
| Standard-IA                                   | Infrequently accessed data (once a month) with milliseconds access                                                    | ≥ 3                | 30 days              | 128 KB                   | -                                           | Per-GB fees apply |   |
| One Zone-IA                                   | Recreatable infrequently accessed data (once a month) stored in a single Availability Zone with milliseconds access   | 1                  | 30 days              | 128 KB                   | -                                           | Per-GB fees apply |   |
| Glacier Instant Retrieval                     | Long-lived archive data accessed once a quarter with instant retrieval in milliseconds                                | ≥ 3                | 90 days              | 128 KB                   | -                                           | Per-GB fees apply |   |
| Glacier Flexible Retrieval (formerly Glacier) | Long-lived archive data accessed once a year with retrieval of minutes to hours                                       | ≥ 3                | 90 days              | -                        | -                                           | Per-GB fees apply |   |
| Glacier Deep Archive                          | Long-lived archive data accessed less than once a year with retrieval of hours                                        | ≥ 3                | 180 days             | -                        | -                                           | Per-GB fees apply |   |
| Reduced redundancy                            | Noncritical frequently accessed data with milliseconds access (not recommended as S3 Standard is more cost effective) | ≥ 3                | -                    | -                        | -                                           | Per-GB fees apply |   |


### Transition object to the S3-Intelligent-Tiering Storage Class Using Amazon S3 Lifecycle
When data is programmatically uploaded to Amazon S3, some clients might not be compatible with the S3 Intelligent-Tiering storage class. As a result, those clients will upload the data in the Amazon S3 Standard storage class. In this case, you can use Amazon S3 Lifecycle to immediately transition objects from the S3 Standard storage class to the S3 Intelligent-Tiering storage class.

1. If you have logged out of your AWS Management Console session, log back in. Navigate to the S3 console and select the Buckets menu option. From the list of available buckets, select the bucket name of the bucket you created.
   <img width="1069" alt="Screenshot 2024-02-13 at 9 48 39 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/05623d9b-4452-4a18-a719-77e7d094203b">

2. Select the Management tab and then select Create lifecycle rule in the Lifecycle rules section.
   <img width="1085" alt="Screenshot 2024-02-13 at 9 49 27 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/896f0feb-5edc-4d47-8304-d094b767f473">

3. Create lifecycle rule.
   
   <img width="816" alt="Screenshot 2024-02-13 at 9 50 56 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/5fbd3787-4a5d-4fc8-be15-a83fea4f6918">
   
   <img width="809" alt="Screenshot 2024-02-13 at 9 51 09 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/0db3bc9d-8ca6-4621-8f3b-ccf9a6727d13">

   <img width="808" alt="Screenshot 2024-02-13 at 9 51 20 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/2cebd571-f8a6-4b0e-a609-5cdb3413d9ed">

   <img width="825" alt="Screenshot 2024-02-13 at 9 51 31 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/2528658a-b22e-4ce5-aeef-17c119b2c73b">

### Activate the Amazon S3 Intelligent-Tiering Optional Asynchronous Archive Tiers
To save even more on data that doesn’t require immediate retrieval, you can activate the optional asynchronous Archive Access and Deep Archive Access tiers. When these tiers are activated, objects not accessed for 90 consecutive days are automatically moved directly to the Archive Access tier with up to 71% in storage cost savings. Objects are then moved to the Deep Archive Access tier after 180 consecutive days of no access with up to 95% in storage cost savings.

For this workload, we will activate only the Deep Archive Access tier as depicted below:
<img width="1092" alt="Screenshot 2024-02-13 at 9 56 55 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/1ee766c4-9289-4671-86cd-5215ad4352f2">

1. Select the Properties tab.
   <img width="1060" alt="Screenshot 2024-02-13 at 9 58 55 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/d768cb30-96aa-4943-a168-ba9d523f25fd">

2. Navigate to the Intelligent-Tiering Archive configurations section and choose Create configuration.
   <img width="1077" alt="Screenshot 2024-02-13 at 9 59 45 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/a0096a46-5343-49b6-8bfd-7430270ca5fb">

3. In the Archive configuration settings section, specify a descriptive Configuration name for your S3 Intelligent-Tiering Archive configuration.
   <img width="1092" alt="Screenshot 2024-02-13 at 10 00 47 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/4a9c115e-9df6-46d7-8986-b23cfe7948c8">
   
   <img width="734" alt="Screenshot 2024-02-13 at 10 02 10 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/b85a48dd-deb4-4a38-ab51-63f781938e17">

   <img width="672" alt="Screenshot 2024-02-13 at 10 02 47 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/648ea1ae-0a89-40e1-9c69-cdf6f20940e0">

### Upload a File with the opt-in Deep Archive Tier Enabled
1. If you have logged out of your AWS Management Console session, log back in. Navigate to the S3 console and select the Buckets menu option. From the list of available buckets, select the bucket name of the bucket you created.
2. Next, select the Objects tab. Then, from within the Objects section, choose Upload.
3. Then, choose Add files. Navigate to your local file system to locate the file you would like to upload. Select the appropriate file and then choose Open. Your file will be listed in the Files and folders section.
   <img width="955" alt="Screenshot 2024-02-13 at 10 37 07 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/486e3c15-afc6-4bee-82c4-a13d4c6c8fe9">

4. In the Properties section, select Intelligent-Tiering. For more information about the Amazon S3 Intelligent-Tiering storage class.
   <img width="927" alt="Screenshot 2024-02-13 at 10 38 29 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/de5b8178-a0ce-43d1-9c4a-1158250b143a">

5. Because we want the file to be archived after 6 months of no access, in the Tags – optional section we select Add tag with Key “opt-in-archive” and Value “true”, and choose Upload.
   <img width="738" alt="Screenshot 2024-02-13 at 10 39 30 AM" src="https://github.com/ankitakotadiya/Data-Engineering/assets/27961132/7425bd6a-d989-46ff-99df-5ebc646e0f25">

## S3 Data Encryption

### Specifying server-side encryption with Amazon S3 managed keys (SSE-S3)/Default Encryption
All Amazon S3 buckets have encryption configured by default, and all new objects that are uploaded to an S3 bucket are automatically encrypted at rest. Server-side encryption with Amazon S3 managed keys (SSE-S3) is the default encryption configuration for every bucket in Amazon S3.
   
### Specifying server-side encryption with AWS KMS (SSE-KMS)
If you want to specify a different encryption type in your PUT requests, you can use server-side encryption with AWS Key Management Service (AWS KMS) keys (SSE-KMS), dual-layer server-side encryption with AWS KMS keys (DSSE-KMS), or server-side encryption with customer-provided keys (SSE-C). If you want to set a different default encryption configuration in the destination bucket, you can use SSE-KMS or DSSE-KMS.

### Using dual-layer server-side encryption with AWS KMS keys (DSSE-KMS)
Using dual-layer server-side encryption with AWS Key Management Service (AWS KMS) keys (DSSE-KMS) applies two layers of encryption to objects when they are uploaded to Amazon S3. DSSE-KMS helps you more easily fulfill compliance standards that require you to apply multilayer encryption to your data and have full control of your encryption keys.

To require dual-layer server-side encryption of all objects in a particular Amazon S3 bucket, you can use a bucket policy. For example, the following bucket policy denies the upload object (s3:PutObject) permission to everyone if the request does not include an x-amz-server-side-encryption header that requests server-side encryption with DSSE-KMS.

## IAM in Amazon S3
By default, all Amazon S3 resources—buckets, objects, and related subresources (for example, lifecycle configuration and website configuration)—are private. Only the resource owner, the AWS account that created it, can access the resource. The resource owner can optionally grant access permissions to others by writing an access policy.

Amazon S3 offers access policy options broadly categorized as resource-based policies and user policies. Access policies that you attach to your resources (buckets and objects) are referred to as resource-based policies. For example, bucket policies and access point policies are resource-based policies. You can also attach access policies to users in your account. These are called user policies. You can choose to use resource-based policies, user policies, or some combination of these to manage permissions to your Amazon S3 resources. You can also use access control lists (ACLs) to grant basic read and write permissions to other AWS accounts.

The following access point policy grants IAM user Jane in account 123456789012 permissions to GET and PUT objects with the prefix Jane/ through the access point my-access-point in account 123456789012.

```
{
    "Version":"2012-10-17",
    "Statement": [
    {
        "Effect": "Allow",
        "Principal": {
            "AWS": "arn:aws:iam::123456789012:user/Jane"
        },
        "Action": ["s3:GetObject", "s3:PutObject"],
        "Resource": "arn:aws:s3:us-west-2:123456789012:accesspoint/my-access-point/object/Jane/*"
    }]
}
```

### When to use an ACL-based access policy (bucket and object ACLs)
S3 Object Ownership is an Amazon S3 bucket-level setting that you can use to both control ownership of the objects that are uploaded to your bucket and to disable or enable ACLs. By default, Object Ownership is set to the Bucket owner enforced setting, and all ACLs are disabled. When ACLs are disabled, the bucket owner owns all the objects in the bucket and manages access to them exclusively by using access-management policies.





   
   











