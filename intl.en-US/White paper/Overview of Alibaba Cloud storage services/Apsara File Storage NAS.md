# Apsara File Storage NAS

Apsara File Storage NAS is a cloud service that provides file storage for compute nodes, including ECS instances, E-HPC nodes, and Alibaba Cloud Container Service for Kubernetes \(ACK\) nodes. Apsara File Storage NAS is a distributed file system that supports both the NFS and SMB protocols and features shared access, elastic scalability, high reliability, and high performance.

[Apsara File Storage NAS](https://nasnext.console.aliyun.com) provides the four storage types: Extreme NAS, NAS Performance, NAS Capacity, and Infrequent Access.

|Extreme NAS|Extreme NAS is a high-performance file sharing solution that is built on top of the latest generation of network architecture and all-flash storage. The maximum capacity is 256 TB. The bandwidth ranges from 150 Mbit/s to 1,200 Mbit/s. Extreme NAS can provide a constant latency of about 100 microseconds. Extreme NAS is applicable to latency-sensitive business in which a large amount of small files are handled.|
|NAS Performance|NAS Performance uses solid-state drives \(SSDs\) as the storage devices, and provides high throughput, high input/output operations per second \(IOPS\), and low latency for workloads. NAS Performance is a file sharing solution that is applicable to scenarios where high throughput, high concurrency, business scalability, and low read latency are required. You can use NAS Performance if you need to perform frequent read/write operations and have high requirements for response latency.|
|NAS Capacity|NAS Capacity uses SATA hard disk drives \(SATA HDDs\) as storage devices and provides high-performance storage space at low costs. NAS Capacity is a file sharing solution that is applicable to cost-sensitive scenarios where high throughput, high concurrency, and business scalability are required. NAS Capacity is more cost-efficient if you do not need to perform frequent read/write operations and do not have high requirements on response latency.|
|Infrequent Access|You can configure lifecycle rules to transfer data that is infrequently accessed and needs to be stored for long periods from NAS Performance or NAS Capacity to Infrequent Access to reduce costs. For more information, see [Implementation of lifecycle management]().|

## Scenarios

Apsara File Storage NAS is applicable to the following scenarios:

-   Container storage

    You can use containers to build microservices because containers support fast pre-configuration and flexible resource allocation, and can isolate processes. If some containers must access raw data each time they are started, you must create a shared file system for the containers. This way, the containers can access the file system regardless of the instance on which they run. You can use Apsara File Storage NAS as container storage because it provides persistent shared access to files.

-   Content management and web services

    Apsara File Storage NAS provides high persistence and high throughput. Therefore, you can use Apsara File Storage NAS in content management systems and web servers to store and provide data for websites, online publishing applications, and archiving applications. Apsara File Storage NAS follows the expected file system semantics, file naming conventions, and permissions that are preferred by web developers. You can integrate NAS with web applications and use NAS in websites, online publishing applications, and archiving applications.

-   Enterprise applications

    Apsara File Storage NAS provides high scalability, elasticity, availability, and persistence. Therefore, you can use Apsara File Storage NAS as storage solutions for your enterprise applications and applications delivered as services \(ADaaS\). Apsara File Storage NAS provides standard file system interfaces and semantics that allow you to migrate your enterprise applications to Alibaba Cloud or construct new applications.

-   Media and entertainment workflows

    You can use Apsara File Storage NAS to share and process large files in media workflows, such as video editing, audio and video production, broadcast processing, and audio design and rendering. Apsara File Storage NAS provides powerful data consistency models, high throughput, and shared access to files. This reduces the time required to complete the preceding workflows and merges multiple on-premises file repositories into a single repository that can be accessed by all users.

-   Big data analysis

    Apsara File Storage NAS provides high throughput for computing nodes, read and write consistency, and low latency to meet the scale and performance requirements of big data applications. Many analysis workloads use file system interfaces to access data based on file semantics such as file locking, and need to write data to files. In this case, you can use Apsara File Storage NAS that supports file system semantics such as file locking and provides scalable capacity and performance.


## Performance

The peak throughput of a file system is linearly proportional to the used capacity of the file system. A file system with larger capacity has higher peak throughput. Apsara File Storage NAS can be concurrently accessed and randomly read or written by thousands of ECS instances by using POSIX.

|Specifications|Capacity|Latency|IOPS|
|--------------|--------|-------|----|
|Extreme NAS|256 TB|About 100 microseconds|10,000 to 200,000|
|NAS Performance|1 PB|Milliseconds|Up to 30,000 \(4K random read/write\)|
|NAS Capacity|10 PB|About 10 milliseconds|Up to 15,000 \(4K random read/write\)|

## Operations

Apsara File Storage NAS operations on data, such as read and write operations, are implemented by using POSIX. You can easily migrate local applications to the cloud without modifying the code.

Apsara File Storage NAS management operations can be used to send GET and POST requests by using HTTP or HTTPS. If you are familiar with network protocols and one or more programming languages, we recommend that you call API operations to manage your Apsara File Storage NAS resources. You can use Apsara File Storage NAS SDKs, Alibaba Cloud Command Line Interface \(CLI\), or Alibaba Cloud API Explorer to call Apsara File Storage NAS management operations to perform the following operations on file systems, mount targets, permission groups, snapshots, and tags: create, delete, query, and modify. For more information, see [API operations](). If you prefer graphical web application, you can use the [Apsara File Storage NAS console](https://nasnext.console.aliyun.com/overview) to perform all operations supported by Apsara File Storage NAS management operations.

## Scalability and elasticity

When you use Apsara File Storage NAS, you do not need to perform complex operations that you perform on traditional storage systems, such as planning, purchasing, partitioning, and monitoring. The capacity of an Apsara File Storage NAS file system automatically scales in or out when you delete files from or add files to the file system. This way, Apsara File Storage NAS allocates storage resources based on your requirements without affecting your applications.

## Data durability and service availability

Apsara File Storage NAS provides multiple replicas for each piece of data that is stored in a file system. These replicas reside in devices that are isolated across different fault domains for geo-redundancy. Apsara File Storage NAS provides data reliability of 99.999999999% \(eleven 9's\). This reduces a large number of data security risks.

## Security

-   Permission group

    In Apsara File Storage NAS, a permission group is a whitelist that defines the permission information of a file system, including the authorized IP addresses, read and write permissions, and the permissions of users. You can add rules to a permission group to allow access to a file system from specific IP addresses or CIDR blocks. You can also grant different access permissions to different IP addresses or CIDR blocks.

-   RAM

    You can use Resource Access Management \(RAM\) to manage the users of Apsara File Storage NAS and control the access permissions of resources. RAM implements access control based on users. You can create and manage multiple RAM users under an Alibaba Cloud account. You can also grant different permissions to each RAM user. This allows each RAM user to have different access permissions on Alibaba Cloud resources. By using RAM, you do not need to share your AccessKey pairs with other users. You can assign minimal permissions to each RAM user to reduce data security risks for your enterprise. For more information, see [Perform access control based on RAM policies]().

-   ACL

    You can use Access Control Lists \(ACLs\) to manage the access permission of files and directories. ACL implements access control based on resources. Access control and user management are necessary for enterprises that want to share files among different users and groups by using a shared file system. Apsara File Storage NAS provides the ACL feature that allows you to grant users and groups different access permissions on directories and files. For more information, see [NAS NFS ACL]() and [NAS SMB ACL]().

-   Encryption

    Apsara File Storage NAS uses the 256-bit advanced encryption standard \(AES-256\) to encrypt static data stored in file systems and uses Key Management Service \(KMS\) to manage encryption keys. Apsara File Storage NAS encrypts data before it writes the data to file systems and automatically decrypts encrypted data before it reads the data and provides the data to applications. Apsara File Storage NAS automatically encrypts and decrypts data. Therefore, you do not need to modify your application code for data encryption and decryption.


## Cost model

The capacity of an Apsara File Storage NAS file system automatically scales in or out based on your business requirements. Therefore, you do not need to partition the file system in advance. Accordingly, Apsara File Storage NAS is billed in the pay-as-you-go mode. You are charged only for the storage space that is actually used. If a file is deleted from a file system, the storage space that was used to store the file no longer incurs fees. You can also purchase storage plans in advance to deduct the resource that you use. In most cases, storage plans are more cost-effective.

