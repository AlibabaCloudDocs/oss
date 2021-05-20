# Benefits

Object storage service \(OSS\) is a secure, cost-effective, and high-durability cloud storage service that enables you to store large amounts of data in the cloud. This topic compares OSS with the traditional user-created server storage to help you better understand the benefits of OSS.

## Advantages of OSS over user-created server storage

|Item|OSS|User-created server storage|
|:---|:--|---------------------------|
|Durability|OSS is the core infrastructure of data storage for Alibaba Group. This reliable and highly available service is one of the backbone services that ensure stability during the peak hours of Double 11 shopping festival. OSS features a multi-redundant architecture to provide reliable data storage. In addition, OSS is built on a high-availability architecture to eliminate single points of failure \(SPOFs\) and ensure the continuity of your services. -   Provides service availability of at least 99.995%.
-   Provides data durability of at least 99.9999999999% \(twelve 9’s\) \(designed for\).
-   Automatically expands capacities without affecting your services.
-   Supports automatic backup for data redundancy.

|-   Prone to errors due to hardware failures. If a disk has a bad sector, data may be lost.
-   Manual data restoration is complex and requires a lot of time and technical resources. |
|Security|-   Provides multi-level security protection for enterprises, including features such as server-side encryption, client-side encryption, hotlink protection, bucket policies that specify IP address blacklists or whitelists, fine-grained permission control, log audit, and Write Once Read Many \(WORM\) policies.
-   Provides resource isolation mechanisms for multiple tenants and supports geo-disaster recovery.
-   Complies with the regulations of multiple organizations including the U.S. Securities and Exchange Commission \(SEC\) and Financial Industry Regulatory Authority, Inc. \(FINRA\), delivering services that can meet the requirements of your enterprise on data security and compliance.

|-   Requires additional scrubbing devices and black hole policy-related services.
-   Requires a separate security mechanism. |
|Cost|-   Provides multi-line backbone networks that support Border Gateway Protocol \(BGP\) with sufficient bandwidth resources. Upstream traffic is free of charge.
-   Requires no hosting fees or the need to hire maintenance technicians.

|-   Storage space is limited by hard disk capacity. The storage space must be manually resized.
-   Single-line or double-line access is slow, and bandwidth is limited. The bandwidth must be manually resized during peak hours.
-   Maintenance technicians are required, which results in high costs. |
|Intelligent storage|Provides multiple data processing capabilities, such as Image Processing \(IMG\), video snapshot, document preview, image scenario recognition, facial recognition, and SQL in-place query. OSS can be seamlessly integrated with the Hadoop ecosystem and Alibaba Cloud services such as Function Compute, E-MapReduce \(EMR\), Data Lake Analytics \(DLA\), BatchCompute, MaxCompute, and Database Backup to manage and analyze data of enterprises.|Data processing capabilities must be purchased and separately deployed.|

## More benefits of OSS

-   Ease of use
    -   OSS provides standard RESTful API operations, a wide range of SDKs, client tools, and the OSS console. You can upload, download, retrieve, and manage large amounts of data used for websites and mobile apps in the same way you use regular file systems.
    -   The capacity of each bucket is unlimited. Therefore, you can expand your buckets in OSS based on your requirements.
    -   OSS supports streaming writes and reads. It is suitable for business scenarios that require simultaneous write and read of large files such as videos.
    -   OSS supports lifecycle management. You can configure lifecycle rules to batch delete expired objects or convert the storage classes of expired objects to cost-effective Infrequent Access \(IA\), Archive, or Cold Archive.
-   Powerful and flexible security mechanisms
    -   OSS provides Security Token Service \(STS\) and URL authentication and authorization. OSS also supports IP address blacklists or whitelists, hotlink protection, and the RAM mechanism.
    -   OSS provides resource isolation mechanisms for users. You can also use the multi-cluster synchronization service.
    -   OSS provides server-side encryption, client-side encryption, and encrypted transmission based on SSL or TLS to protect data from potential security risks on the cloud.
    -   OSS provides the versioning feature to prevent objects from being accidentally deleted or overwritten.
-   Data redundancy mechanism

    OSS uses a data redundant storage mechanism to store copies of each object on multiple devices of different facilities in the same region. This way, OSS ensures data durability and availability even if hardware failures occur.

    -   Operations performed on objects in OSS are highly consistent. For example, after you upload or copy an object, the uploaded or copied object can be read immediately and copies of the object are written to multiple devices for redundancy.
    -   To ensure that data is completely transmitted, OSS calculates the checksum of the network traffic packets to check for errors when packets are transmitted between the client and the server.
    -   The data redundancy mechanism of OSS can prevent data loss when two storage devices are damaged at the same time.
        -   After data is stored in OSS, OSS regularly checks whether copies of the data are lost. Then, OSS recovers lost copies to ensure the durability and availability of the data.
        -   OSS periodically verifies the integrity of data to detect data corruption caused by errors such as hardware failures. If data is partially corrupted or lost, OSS uses the other copies to reconstruct and repair the corrupted data.
-   Rich and powerful value-added services
    -   IMG: supports format conversion, thumbnails, cropping, watermarking, resizing for objects in formats such as JPG, PNG, BMP, GIF, WebP, and TIFF.
    -   Audio or video transcoding: provides high-quality, high-speed, and parallel audio or video transcoding capabilities. This way, your audio or video files can be played on different terminal devices.
    -   Transfer acceleration over Internet: provides the transfer acceleration service, which uses optimal route selection and protocol stack tuning to reduce timeouts in remote transmission and improve user experience. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).
    -   Accelerated content delivery: uses OSS as the origin with Content Delivery Network \(CDN\) to improve user experience when the same object is repeatedly downloaded.

