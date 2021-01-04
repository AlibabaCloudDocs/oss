# Block Storage

Block Storage is a high-performance, low-latency block storage service for Alibaba Cloud ECS. Block Storage is similar to a physical disk. You can format a Block Storage device and create a file system on it to meet the data storage requirements of your business.

## Scenarios

Alibaba Cloud provides a variety of [Block Storage devices](https://ecs-buy.aliyun.com/?spm=5176.54360.880806.btn1.77b76a81ly4Be3#/clouddisk) for ECS instances, such as cloud disks based on a distributed storage architecture and local disks located on the physical machines where the ECS instances are hosted. The following content describes the features of cloud disks and local disks:

-   Cloud disks are block-level storage devices provided by Alibaba Cloud for ECS instances. Cloud disks use a triplicate distributed mechanism and provide low latency, high performance, high durability, and high reliability. Cloud disks can be created, resized, and released at any time.
-   Local disks are physical disks attached to physical machines that host ECS instances. Local disks provide local storage access capabilities for ECS instances. Local disks are suitable for scenarios where high storage I/O performance and high cost performance for massive storage are required. Local disks provide low latency, high random IOPS and throughput, and high cost performance.

## Performance

The key metrics used to measure Block Storage performance include IOPS, throughput, and latency. Some Block Storage devices also have requirements on capacity. For example, enhanced SSDs of different performance levels \(PLs\) have different capacity ranges. For more information, see [EBS performance](/intl.en-US/Block Storage/Performance/EBS performance.md).

## Data durability and service availability

Three copies of your business data are stored in the Block Storage cluster in the same zone to ensure 99.9999999% \(nine 9's\) data reliability during read and write operations.

To improve the availability of Block Service, we recommend that you create snapshots on a regular basis to provide data backup capabilities for cloud disks to make sure that information such as logs and customer transactions are backed up. For more information, see [Snapshot overview](/intl.en-US/Snapshots/Snapshot overview.md).

## Scalability and elasticity

You can partition or release a cloud disk and adjust the storage capacity in real time based on your business requirements. You can scale out the cloud disk attached to your ECS instance to meet the storage requirements by using the following methods when your business and application data grow:

-   Resize the disk. In this case, you must resize the existing partitions of the disk or create new partitions for the disk.
-   Create a new disk, attach the disk to the ECS instance, and partition and format the disk.
-   Replace the system disk of the instance and specify a larger size for the new system disk.

## Security

You can use RAM to authorize other users to access your cloud disks.

We recommend that you encrypt the storage devices that you use if your applications are data-sensitive. Cloud disks and their snapshots are encrypted by using keys based on the standard AES-256 algorithm. Data is automatically encrypted when it is transmitted from ECS instances to cloud disks. Encrypted data is automatically decrypted when it is read. For more information, see [Encryption overview](/intl.en-US/Block Storage/Encrypt a disk/Encryption overview.md).

## Operations

Block Storage provides API operations that you can use to send GET and POST requests by using HTTP or HTTPS. If you are familiar with network protocols and one or more programming languages, we recommend that you call API operations to manage your Block Storage resources. You can use ECS SDKs, Alibaba Cloud Command Line Interface \(CLI\), or Alibaba Cloud API Explorer to call Block Storage API operations to perform the following operations: create, delete, query, attach, detach, and scale out cloud disks on ECS instances and create, delete, and query the cloud disk snapshots stored in OSS.

If you prefer graphical web application, you can use the ECS console to perform all operations supported by Block Storage API operations.

## Cost model

Block Storage supports two billing methods: pay-as-you-go and subscription. If you use the pay-as-you-go method, you can activate and release Block Storage resources based on your requirement and do not need to purchase a large number of resources in advance. This way, you can reduce the costs by 30% to 80% compared with traditional hosts. If you use the subscription mode to purchase resources in advance, you can further reduce the costs.

The billing methods of cloud disks depend on how the disks are created.

-   Cloud disks created along with an ECS instance use the same billing method as the ECS instance.
-   Cloud disks created for a subscription ECS instance are billed by using the subscription method.
-   Cloud disks created on the Disks page of the ECS console support only the pay-as-you-go method.
-   Cloud disks created from snapshots support only the pay-as-you-go method.

You can change the billing methods of your cloud disks. For more information, see [Change billing methods of disks](/intl.en-US/Block Storage/Cloud disks/Change billing methods of disks.md). SCUs can be used to offset bills of eligible pay-as-you-go cloud disks in the current region. For more information, see [Overview](/intl.en-US/Block Storage/Storage capacity units/Overview.md).

