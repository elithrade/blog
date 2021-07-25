# AWS solution architect associate

## High Level Services

### Difference between Regions, Availability Zones and Edge Location
* Availability zone is basically a data centre, or multiple data centres are close to each other.
* Region is a geographical area, contains two or more availability zones. Each region is a separate geographic area. * Each region has multiple, isolated locations known as Availability Zones.
* Edge locations are for AWS to cache contents, e.g. if a user in Sydney requires content from New York region, the content will be first downloaded from New York and stored in Sydney, so if the same content is requested again it will be available from the Sydney region. Currently there are more than 150 edge locations around the world.

### VPC
A Virtual Private Cloud (VPC) is a virtual network dedicated to a single AWS account. It is logically isolated from other virtual networks in the AWS cloud, providing compute resources with security and robust networking functionality. 

## IAM (Identity Management Service)
* Users, end users such as employees of an organisation.
Groups, a collection of users, each user in the group inherits the permissions of the group.
* Policies, json format documents describe the permissions that user, group or role can or cannot do.
* Roles can be assigned to AWS resources.
* IAM created users are global and not tied down to a region, same for group and role.
* A root account is the one that created AWS account, it has complete admin access.
* When a user is created, it has no permissions by default.

## S3

### Basics
* When uploading a file, a HTTP 200 code is received to indicate upload is successful.
* Creating a bucket will generate a unique HTTPS link.
* Object-based, think of objects as files. You cannot use it as databases. It can consist of the following:
  * Key, the name of the object.
  * Value, the data, made up of sequence of bytes.
  * Version ID, S3 allows you to have multiple versions of a file.
  * Metadata, data about the data you store.
  * Subresources:
    * Access Control Lists.
    * Torrent.

### Data Consistency
* Read after write consistency, if you write a file and immediately read after, you will be able to view the data.
* Eventual consistency, If you update an existing file or delete a file, and read immediately, you may get the old version or you may not. Basically changes to existing objects can take a bit of time to propagate.

### Features
* Tiered storage
  * S3 standard, 99.99% availability, 99.99..% (11 9s’) durability.
  * Stored redundantly across multiple devices in multiple facilities, designed to sustain the loss of 2 facilities.
  * S3 IA (infrequently accessed), for data access less frequently, but require rapid access when needed. Cost less than S3, but charged on retrieval.
  * S3 One Zone IA, same as IA, low cost, do not require multiple available availability zone.
  * S3 Intelligent Tiering, using ML to determine tier automatically based on how your data is accessed. Introduced in 2018.
  * S3 Glacier, secure, durable, and low cost storage, retrieval time varies from minutes to hours.
  * S3 Glacier Deep Archive, lowest cost storage, retrieval time of 12 hours is acceptable.
* Lifecycle management.
* Versioning.
* Encryption.
* MFA authentication for deleting objects, data protection.
* Secure your data using Access Control Lists and Bucket Policies, basically permissions for objects and buckets.
* Cross region replication, upload in one region data will be replicated to other regions.
* S3 Transfer Acceleration, takes advantage of CloudFront global distributed edge locations, data is routed to S3 using optimised network path.

*Read S3 FAQ before taking the exam*.

### Security and Encryption
* Security is achieved by using Access Control Lists (for files)  and Bucket Policies (for buckets).
* Encryption in transit, using SSL/TLS, like HTTPS.
* Encryption at rest, server side encrypted, is achieved using:
  * S3 managed key, used to encrypt and decrypt objects. SSE-S3 (server side encryption S3), managed by Amazon.
  * AWS Key Management Service. SSE-KMS.
  * SSE-C with customer provided key.
* Client side encryption, you upload encrypted objects to S3.

### Versioning
* Once the bucket with the versioning turned on, it cannot be turned off.
* Has MFA delete capability.
* Integrates with Lifecycle rules.
* Upload a newer version of a file changes it’s permission to default access denied.
* With versioning turned on the size of your bucket will grow exponentially.
* Delete versioned file only puts a delete marker to the file, versioned files can be restored by deleting the delete marker. Individual versions can be deleted.

### Lifecycle Management
* Automates moving objects between the different storage tiers.
* Can be used with versioning.
* Can be applied to current version and previous versions.

### Cross Region Replication
* Region must be unique.
* Existing objects on source bucket won’t be replicated. Only new files are replicated.
* Delete marker won’t be replicated.
* Delete individual files or delete marker won’t be replicated.

## CloudFront
* CloudFront is a global service, not based on region.
* You can restrict access using signed URLs.
* CDN, content delivery network, content cached on Edge location, initial content request will be fetched from the origin, but subsequent content request will be faster.
* Edge Location, the location where the content will be cached, this is separate the a region/AZ.
* Origin, the location stores the content CDN will distribute. This could be a S3 bucket, EC2 instance, Elastic load balancer, or Route53.
* Distribution, a collection of ELs.
* Web Distribution, used for websites.
* RTMP, used for video streaming.
* ELs are not just for reading, but also writing, e.g. S3 accelerated transfer uses CloudFront.
* Objects are cached for TTL (time to live in seconds), which can be configured.
* You can clear cache, but you will be charged for that.

## Snowball
* Used to transfer large amounts of data.
* Can be imported to S3.
* Can be exported from S3.

## Storage Gateway
* File Gateway, for flat files stored directly to S3.
* Volume Gateways
  * Stored volumes, entire dataset is stored on site and synchronously backed up to S3.
  * Cached volumes, entire dataset is stored on S3, only the most frequently accessed data will be stored on site.
* Gateway Access Tape Library, options available if backing up tape libraries.

## EC2
Amazon Elastic Compute Cloud is a web service that provides resizable compute capacity in the cloud. Reduce the time required to boot new server instances to minutes, quickly scale capacity, both up and down.
* Termination Protection is turned off by default.
* Root volume is where the OS is installed.
* EBS-backed instance, the default action is for the root EBS volume to be deleted when the instance is terminated.
* EBS root volumes cannot be encrypted. Additional volumes can be encrypted.

### Pricing Models
* On Demand allows you to pay a fixed rate by hours or seconds, with no commitment.
  * Low cost without any upfront payment or long term commitment.
  * Applications with short term, spiky, or unpredictable workloads that cannot be interrupted.
  * Application being developed or tested on EC2 for the first time.
* Reserved provides you with a capacity reservation, and offer a significant discount on hourly charge for an instance. Contract terms are 1 year or 3 years.
  * Steady state or predictable usage.
  * Require reserved capacity.
  * Users are able to make upfront payments to reduce their computing cost even further.
* Spot enables you to bid whatever price you want for instance capacity, works like a stock market.
  * Flexible start end times.
  * Applications that are only feasible at very local computer prices.
  * Users with urgent computing needs for large amounts of additional capacity.
* Dedicated
  * Useful for regulatory requirements not support multi-tenant virtualization.
  * Licensing doesn’t support multi-tenancy or cloud deployments.
* Note you won’t be charged for partial hours of usage if your spot instance is terminated. If you terminate the instance yourself, you will be charged for any hour in which the instance ran.

### EC2 Security Groups
* Every time you make a change to the security group the change will take effect immediately.
* When an Inbound rule is created a corresponding Outbound rule is created for the inbound rule. It is Stateful.
* Everything is blocked by default.
* All outbound traffic us allowed.
* You can have any number of EC2 instances within a security group.
* You can have multiple security groups attached to EC2 instances.
* You cannot block specific IP addresses, you can only specify allow rules not deny rules.

### EBS
* Virtual hard disk, snapshots exist on S3.
* Snapshots are incremental, this means that only the blocks that have changed since your last snapshot are moved to S3.
* You should stop the instance before taking the snapshot, but you can take a snapshot while it is running.
* You can create AMIs from both volumes and snapshots.
* EBS volume size and type can be changed on the fly.
* Wherever your EC2 instance is, the volume will alway be in the same region.
* If EC2 instance is deleted the root device is deleted too by default. However additional volumes attached to the instance stay persist.
* To move an EC2 volume from one AZ to another, take a snapshot of it, create an AMI from the snapshot and then use the AMI to launch the EC2 instance in a new AZ. SImilarly the EC2 instance can be moved to another region.

### Instance Store Volumes
* Instance Store Volumes (Ephemeral) cannot be stopped, if the underlying hosts fails, you lose your data.
* You can reboot EBS or ISV, data will not be lost.
* Both root volumes will be deleted on termination.
* It is mainly used for constant changing data, since ISVs are closer to physical memory and much faster than EBS.

### Encrypted Root Device
* Snapshots of encrypted volumes are encrypted automatically.
* Volumes restored from encrypted snapshots are encrypted automatically.
* Snapshots can only be shared if they are unencrypted.
* Snapshots can be shared with other AWS accounts or made public.
* You can now encrypt root device volumes upon creation of the ECs instance.
* On existing volume, create a snapshot, copy the snapshot enable it with encryption, then create an AMI.
* New instances can be launched using the encrypted AMI.
* You cannot take an encrypted AMI and launch it as an unencrypted instance.

### CloudWatch
* CloudWatch is used to monitor performance.
* CloudWatch can monitor most of AWS as well as your applications.
* CloudWatch with EC2 will monitor events every 5 minutes by default and you can have 1 minute intervals by turning on detailed monitoring.
* CloudWatch alarms can trigger notifications.
* CloudWatch is all about performance and CloudTrial is all about auditing.

### IAM Roles with EC2
* Roles are more secure than storing your access key and secret access key on EC2 instance.
* Roles are easier to manage, and can be assigned to an EC2 instance after it is created.
* Roles are universal, you can use them in any region.

### EFS (Elastic File System)

