# Amazon EBS
Amazon Elastic Block Store (Amazon EBS) provides block level storage volumes for use with EC2 instances. EBS volumes behave like raw, unformatted block devices. You can mount these volumes as devices on your instances. EBS volumes that are attached to an instance are exposed as storage volumes that persist independently from the life of the instance. You can create a file system on top of these volumes, or use them in any way you would use a block device (such as a hard drive). You can dynamically change the configuration of a volume attached to an instance.

We recommend Amazon EBS for data that must be quickly accessible and requires long-term persistence. EBS volumes are particularly well-suited for use as the primary storage for file systems, databases, or for any applications that require fine granular updates and access to raw, unformatted, block-level storage. Amazon EBS is well suited to both database-style applications that rely on random reads and writes, and to throughput-intensive applications that perform long, continuous reads and writes.

## Amazon EBS volumes
An Amazon EBS volume is a durable, block-level storage device that you can attach to your instances. After you attach a volume to an instance, you can use it as you would use a physical hard drive. EBS volumes are flexible. For current-generation volumes attached to current-generation instance types, you can dynamically increase size, modify the provisioned IOPS capacity, and change volume type on live production volumes.

Amazon EBS provides the following volume types: General Purpose SSD (gp2 and gp3), Provisioned IOPS SSD (io1 and io2), Throughput Optimized HDD (st1), Cold HDD (sc1), and Magnetic (standard). They differ in performance characteristics and price, allowing you to tailor your storage performance and cost to the needs of your applications. 

Volume types

* Solid state drive (SSD) volumes
* Hard disk drive (HDD) volumes
* Previous generation volumes

### General Purpose SSD volumes
General Purpose SSD (gp3) volumes are the latest generation of General Purpose SSD volumes, and the lowest cost SSD volume offered by Amazon EBS. This volume type helps to provide the right balance of price and performance for most applications. It also helps you to scale volume performance independently of volume size. This means that you can provision the required performance without needing to provision additional block storage capacity. Additionally, gp3 volumes offer a 20 percent lower price per GiB than General Purpose SSD (gp2) volumes.

### Provisioned IOPS SSD volumes
io2 Block Express volumes are built on the next generation of Amazon EBS storage server architecture. It has been built for the purpose of meeting the performance requirements of the most demanding I/O intensive applications that run on instances built on the Nitro System. With the highest durability and lowest latency, Block Express is ideal for running performance-intensive, mission-critical workloads, such as Oracle, SAP HANA, Microsoft SQL Server, and SAS Analytics.

### Throughput Optimized HDD and Cold HDD volumes
* Throughput Optimized HDD — A low-cost HDD designed for frequently accessed, throughput-intensive workloads.
* Cold HDD — The lowest-cost HDD design for less frequently accessed workloads.

### Attach an Amazon EBS volume to an instance
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. In the navigation pane, choose Volumes.
3. Select the volume to attach and choose Actions, Attach volume.
4. For Instance, enter the ID of the instance or select the instance from the list of options.
5. For Device name, enter a supported device name for the volume. This device name is used by Amazon EC2. The block device driver for the instance might assign a different device name when mounting the volume.
6. Choose Attach volume.

Amazon EBS Multi-Attach enables you to attach a single Provisioned IOPS SSD (io1 or io2) volume to multiple instances that are in the same Availability Zone. You can attach multiple Multi-Attach enabled volumes to an instance or set of instances. Each instance to which the volume is attached has full read and write permission to the shared volume. Multi-Attach makes it easier for you to achieve higher application availability in applications that manage concurrent write operations.


## Amazon EBS snapshots
You can back up the data on your Amazon EBS volumes by making point-in-time copies, known as Amazon EBS snapshots. A snapshot is an incremental backup, which means that we save only the blocks on the device that have changed since your most recent snapshot. This minimizes the time required to create the snapshot and saves on storage costs by not duplicating data.

EBS snapshots are stored in Amazon S3, in S3 buckets that you can't access directly. You can create and manage your snapshots using the Amazon EC2 console or the Amazon EC2 API. You can't access your snapshots using the Amazon S3 console or the Amazon S3 API.

To create a snapshot using the console
1. Open the [Amazon EC2 console](https://console.aws.amazon.com/ec2/).
2. In the navigation pane, choose Snapshots, Create snapshot.
3. For Resource type, choose Volume.
4. For Volume ID, select the volume from which to create the snapshot.
5. The Encryption field indicates the selected volume's encryption status. If the selected volume is encrypted, the snapshot is automatically encrypted using the same KMS key. If the selected volume is unencrypted, the snapshot is not encrypted.
6. (Optional) For Description, enter a brief description for the snapshot.
7. (Optional) To assign custom tags to the snapshot, in the Tags section, choose Add tag, and then enter the key-value pair. You can add up to 50 tags.
8. Choose Create snapshot.

To create multi-volume snapshots using the console follow the below options:

<img width="839" alt="Screenshot 2024-03-03 at 4 57 48 PM" src="https://github.com/ankitakotadiya/Data-Engineering-ML/assets/27961132/3673c1e8-2602-4993-a2ac-463c8afa66f9">

You can lock your Amazon EBS snapshots to protect them against accidental or malicious deletions, or to store them in WORM (write-once-read-many) format for a specific duration. While a snapshot is locked, it can't be deleted by any user, regardless of their IAM permissions. You can continue to use a locked snapshot in the same way that you would use any other snapshot.

### Amazon Data Lifecycle Manager
You can use Amazon Data Lifecycle Manager to automate the creation, retention, and deletion of EBS snapshots and EBS-backed AMIs. When you automate snapshot and AMI management, it helps you to:

* Protect valuable data by enforcing a regular backup schedule.
* Create standardized AMIs that can be refreshed at regular intervals.
* Retain backups as required by auditors or internal compliance.
* Reduce storage costs by deleting outdated backups.
* Create disaster recovery backup policies that back up data to isolated Regions or accounts.





