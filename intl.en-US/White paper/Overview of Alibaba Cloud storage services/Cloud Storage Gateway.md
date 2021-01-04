# Cloud Storage Gateway

Cloud Storage Gateway \(CSG\) is a gateway service that can be deployed in on-premises data centers and Alibaba Cloud. CSG uses Alibaba Cloud Object Storage Service \(OSS\) buckets as backend storage devices and provides on-premises and in-cloud applications with standard file services by using the Network File System \(NFS\) and Server Message Block \(SMB\) protocols and block storage services by using the Internet Small Computer Systems Interface \(iSCSI\) protocol.

[CSG](https://sgwnew.console.aliyun.com/?spm=5176.144914.752642.btn1.595f7d70qYzCyZ) supports two types of gateways:

-   File gateways

    File gateways map the objects and folders in OSS buckets to the files and directories in Apsara File Storage NAS file systems. This way, you can read and write all objects in a specified OSS bucket by using standard NFS and SMB protocols. File gateways use local storage as the cache for hot data to provide high-performance data access to the large storage space provided by OSS buckets. File gateways are compatible with Portable Operating System Interface \(POSIX\) and third-party backup software. If you want to back up small files or share small files for reading and writing, we recommend that you use standard or basic types of file gateways. If you have high performance requirements or want to use multiple clients to simultaneously access shared data, we recommend that you use enhanced and advanced types of file gateways.

-   Block gateways

    Block gateways create storage volumes in OSS and allows you access OSS by using the Internet Small Computer Systems Interface \(iSCSI\) protocol. Local applications can access these volumes as iSCSI targets. Block gateways run in two modes: write-through and cache. In the write-through mode, data stored in volumes is sliced and synchronized to the cloud. This mode is applicable to scenarios where you use high-speed links such as leased lines. In the cache mode, local cache disks are created to accelerate read and write operations and transfer the cached data to the cloud asynchronously. This mode is applicable to scenarios where you want to access locally cached data quickly and transfer data to OSS in a normal speed.


## Scenarios

CSG is applicable to the following scenarios:

-   Storage scale-out and data migration

    CSG is integrated with intelligent caching algorithms, which automatically identify hot and cold data. This allows CSG to store cold data in the cloud and retain hot data in the local cache to accelerate access to hot data. When you connect your on-premises data center to cloud storage services, your users can access the requested data without noticing the connection. This enables you to expand your storage from your own data center to cloud storage. CSG also stores a full copy of data in the cloud to guarantees data integrity.

-   Cloud disaster recovery

    With the development of cloud computing, an increasing number of users run their workloads in the cloud. Therefore, the reliability and continuity of workloads running in the cloud become critical issues. CSG supports virtualization technologies. You can easily connect third-party services to Alibaba Cloud services to implement disaster recovery.

-   Data sharing and distribution in multiple regions

    You can deploy CSG instances in multiple regions and associate the CSG instances with the same OSS bucket to quickly share and distribute files across multiple regions. This feature is applicable to branch offices where data needs to be synchronized and shared.

-   Compatibility with legacy applications

    Some users may run both legacy and modern applications in the cloud. In this case, the legacy applications migrated from an on-premises data center use standard storage protocols, such as NFS, SMB, and iSCSI. Modern applications are typically developed based on new technologies and support object access protocols. Data communication among applications that use different protocols requires complicated processes. CSG can establish communications among applications that use different protocols and enable data exchange between legacy and modern applications.

-   An alternative to ossfs and ossftp

    ossfs and ossftp are open-source tools developed based on file protocols. You can use ossfs and ossftp to upload files to OSS. However, ossfs and ossftp are not supported in the production environment due to their low compatibility with POSIX. If you use ossfs and ossftp to mount file systems to a client, additional configurations and caches are required. In scenarios where you need to use ossfs and ossftp to mount file systems to multiple clients, the configuration process may take a long time. CSG can be used as an alternative to ossfs and ossftp. To accelerate access to data stored on OSS, you need only to create a CSG instance and mount an NFS share to your local client in Linux or map an SMB share to a network drive in Windows. You can then manage data in the OSS bucket in the same way as you manage it in the local file system.


## Performance

CSG is deployed between applications and OSS. Therefore, the performance of CSG is determined by multiple factors, including the speed and configurations of local disks, the bandwidth between the iSCSI initiator and the gateway, the storage capacity allocated to the gateway virtual machine, and the bandwidth between the gateway virtual machine and OSS. To allow local applications to access data with low latency, you must configure sufficient local cache to store the data that is accessed recently. The cache capacity of a share can be calculated in the following formula: Recommended local cache capacity = \[Application bandwidth \(Mbit/s\) - Backend bandwidth of the gateway \(Mbit/s\)\] × Write duration \(seconds\) × 1.2. To obtain better performance when you access data from local clients, you can estimate the total amount of hot data. Compare the total amount of hot data with the recommended local cache capacity, and select the larger value as the capacity of the local cache disk. The synchronization bandwidth of a gateway is determined by the bandwidth of OSS. OSS provides up to 10 Gbit/s of bandwidth for each user. The bandwidth slightly varies among clusters in different regions. For more information, contact the technical support of OSS in each region.

CSG supports the transfer acceleration feature that increases the data transfer rate across regions by using the public bandwidth of gateways. You can enable transfer acceleration when or after you create a share.

## Data durability and service availability

In the cache mode, data is written to a disk by using synchronous I/O to prevent data loss due to power failures. CSG ensures the durability and reliability of data in the local cache disks by using Alibaba Cloud disks. The reliability of on-premises gateways depend on the reliability of the backend storage in your virtual environment. We recommend that you use Redundant Array of Independent Disks \(RAID\) storage or a distributed storage system as local cache disks. CSG refreshes and uploads data in the local cache disks to OSS buckets. Alibaba Cloud OSS guarantees 99.9999999999% \(twelve 9's\) designed data reliability. This ensures data security and reliability when you transfer data from CSG to Alibaba Cloud.

## Scalability and elasticity

CSG uses OSS buckets as backend storage devices. Therefore, it inherits the scalability and elasticity of OSS. You may encounter problems if you store a large number of files in the same directory in a common file system. In contrast, the total storage capacity of OSS and the capacity of a single bucket are not limited. You can store an unlimited number of objects in a bucket. OSS stores the copies of your data in different servers in the same region. All copies have the same performance provided by the high-performance infrastructure of Alibaba Cloud.

## Security

CSG supports user identity management and resource access control based on Resource Access Management \(RAM\). RAM implements access control based on users. RAM allows you to create and manage multiple RAM users under an Alibaba Cloud account and grant diverse permissions to each RAM user. This way, you can authorize different RAM users to access different Alibaba Cloud resources. You can use RAM to avoid sharing your AccessKey pairs with other users. You can assign minimal permissions to each RAM user to reduce data security risks for your enterprise. For more information, see [Use RAM to implement account-based access control](/intl.en-US/Best Practice/Use RAM to implement account-based access control.md).

CSG provides data encryption feature to encrypt the data transferred from gateways to OSS buckets. You can also enable the server-side encryption feature provided by OSS and configure a CMK. This way, files uploaded from a share to OSS are automatically encrypted on the OSS server by using the specified CMK.

## Operations

You can perform the following operations on the CSG console: deploy a gateway virtual machine to an on-premises data center or an ECS instance, configure a file gateway or block gateway, and create cache disks and shares. You can also manage CSG by using API operations. CSG provides API operations that you can use to send GET requests by using HTTP or HTTPS.

## Cost model

CSG is billed based on the used resources. Gateways deployed on the cloud and on-premises data centers are billed based on different items. Gateways deployed on the cloud are billed based on the gateway specifications, gateway cache types, and the public network bandwidth. Gateways deployed on the cloud supports the pay-as-you-go and subscription billing methods. Gateways deployed on on-premises data centers run with your own virtual machines and cache resources. Alibaba Cloud only charges for the gateway software license. For more information, see [Pricing](/intl.en-US/Pricing/Pricing.md).

