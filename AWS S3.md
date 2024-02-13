# AWS S3
Amazon Simple Storage Service (Amazon S3) is an object storage service that offers industry-leading scalability, data availability, security, and performance. Customers of all sizes and industries can use Amazon S3 to store and protect any amount of data for a range of use cases, such as data lakes, websites, mobile applications, backup and restore, archive, enterprise applications, IoT devices, and big data analytics. Amazon S3 provides management features so that you can optimize, organize, and configure access to your data to meet your specific business, organizational, and compliance requirements.

Amazon S3 offers a range of storage classes designed for different use cases. For example, you can store mission-critical production data in S3 Standard or S3 Express One Zone for frequent access, save costs by storing infrequently accessed data in S3 Standard-IA or S3 One Zone-IA, and archive data at the lowest costs in S3 Glacier Instant Retrieval, S3 Glacier Flexible Retrieval, and S3 Glacier Deep Archive.

## S3 Storage Class
Amazon S3 (Simple Storage Service) provides several storage classes, each designed to optimize cost, performance, and durability based on different data access patterns and retention requirements. Here's an overview of the commonly used Amazon S3 storage classes:

| Storage class | Designed for | Availability Zones | Min storage duration | Min billable object size | Retrieval fees |
------------------------------------------------------------------------------------------------------------------------

	

Standard	Frequently accessed data (more than once a month) with milliseconds access	≥ 3	-	-	-	-
Intelligent-Tiering	Data with changing or unknown access patterns	≥ 3	-	-	Per-object fees apply for objects >= 128 KB	-
Standard-IA	Infrequently accessed data (once a month) with milliseconds access	≥ 3	30 days	128 KB	-	Per-GB fees apply
One Zone-IA	Recreatable, infrequently accessed data (once a month) stored in a single Availability Zone with milliseconds access	1	30 days	128 KB	-	Per-GB fees apply
Glacier Instant Retrieval	Long-lived archive data accessed once a quarter with instant retrieval in milliseconds	≥ 3	90 days	128 KB	-	Per-GB fees apply
Glacier Flexible Retrieval (formerly Glacier)	Long-lived archive data accessed once a year with retrieval of minutes to hours	≥ 3	90 days	-	-	Per-GB fees apply
Glacier Deep Archive	Long-lived archive data accessed less than once a year with retrieval of hours	≥ 3	180 days	-	-	Per-GB fees apply
Reduced redundancy	Noncritical, frequently accessed data with milliseconds access (not recommended as S3 Standard is more cost effective)	≥ 3	-	-	-	Per-GB fees apply


