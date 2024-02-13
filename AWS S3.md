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









