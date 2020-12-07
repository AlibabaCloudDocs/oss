# Benefits

OSS provides secure, cost-effective, and high-durability services for you to store large amounts of data in the cloud. This topic compares OSS with the traditional user-created server storage to show the benefits of OSS.

## Advantages of OSS over user-created server storage

|Item|OSS|User-created server storage|
|:---|:--|---------------------------|
|Durability|OSS is the core infrastructure of data storage for Alibaba Group. This reliable, high-availability service has proven itself by providing support during the peak hours of Double 11. OSS features a multi-redundant architecture to provide reliable data storage. In addition, OSS is designed based on a high-availability architecture to eliminate single points of failure \(SPOFs\) and ensure the continuity of data-based services. -   Provides service availability of at least 99.995%.
-   Provides data durability of at least 99.9999999999% \(twelve 9’s\) \(designed for\).
-   Automatically expands capacities without affecting your services.
-   Automatically stores multiple copies of data for backup.

|-   Prone to errors due to low hardware durability. If a disk has a bad sector, data may be lost.
-   Manual data restoration is complex and requires a lot of time and technical resources. |
|Security|-   Provides multi-level security protection for enterprises, including features such as server-side encryption, client-side encryption, hotlink protection, IP address blacklist or whitelist, fine-grained permission control, log audit, and Write Once Read Many \(WORM\) policies.
-   Provides resource isolation mechanisms for multiple tenants and supports geo-disaster recovery.
-   Obtains a number of compliance certifications, including certifications from institutions such as SEC and FINRA. This way, the data security and compliance requirements of enterprises can be met.

|-   Requires additional scrubbing devices and black hole policy-related services.
-   Requires a separate security mechanism. |
|Cost|-   Provides multi-line BGP backbone networks with sufficient bandwidth resources. Upstream traffic is free of charge.
-   Requires no maintenance staff or hosting fees.

|-   Storage space is limited by hard disk capacity. The storage space must be manually resized.
-   Single-line or double-line access is slow, and bandwidth is limited. The bandwidth must be manually resized during peak hours.
-   Professional maintenance staff is required, which results in high costs. |
|Intelligent storage|Provides multiple data processing capabilities, such as Image Processing \(IMG\), video snapshot, document preview, image scenario recognition, facial recognition, and SQL in-place query. OSS seamlessly integrates with the Hadoop ecosystem and Alibaba Cloud services such as Function Compute, E-MapReduce \(EMR\), Data Lake Analytics \(DLA\), Batch Compute, MaxCompute, and Database Backup to manage data of enterprises.|Data processing capabilities must be purchased and separately deployed.|

## More benefits of OSS

-   Ease of use
    -   OSS provides standard RESTful API operations, a wide range of SDKs, client tools, and the OSS console. You can upload, download, retrieve, and manage large amounts of data used for websites and mobile apps in the same way you use regular file systems.
    -   The capacity of each bucket is unlimited. Therefore, you can expand your buckets in OSS based on your requirements.
    -   Streaming writes and reads are supported, which can be used in business scenarios where you must simultaneously read and write videos and other large objects.
    -   Lifecycle management is supported. You can configure lifecycle rules to batch delete expired objects or convert the storage classes of expired objects to cost-effective Infrequent Access \(IA\), Archive, or Cold Archive.
-   Powerful and flexible security mechanisms
    -   OSS provides STS and URL authentication and authorization. OSS also supports IP address blacklists or whitelists, hotlink protection, and the RAM mechanism.
    -   OSS provides resource isolation mechanisms for users. You can also use the multi-cluster synchronization service.
    -   OSS provides server-side encryption, client-side encryption, and encrypted transmission based on SSL or TLS to protect data from potential security risks on the cloud.
    -   OSS provides the versioning feature to prevent objects from being accidentally deleted or overwritten.
-   Data redundancy mechanism

    OSS uses a data redundant storage mechanism to store copies of each object on multiple devices of different facilities in the same region. This way, OSS ensures data durability and availability even if hardware failures occur.

    -   Operations performed on objects in OSS are highly consistent. For example, after you upload or copy an object, the uploaded or copied object can be read immediately and copies of the object are written to multiple devices for redundancy.
    -   To ensure complete data transmission, OSS calculates the checksum of the network traffic packets to check for errors when packets are transmitted between the client and the server.
    -   The data redundancy mechanism of OSS can prevent data loss when two storage devices are damaged at the same time.
        -   After data is stored in OSS, OSS regularly checks whether copies of the data are lost. Then, OSS recovers lost copies to ensure the durability and availability of the data.
        -   OSS periodically verifies the integrity of data to detect data corruption caused by errors such as hardware failures. If data is partially corrupted or lost, OSS uses the other copies to reconstruct and repair the corrupted data.
-   Rich and powerful value-added services
    -   IMG: supports format conversion, thumbnails, cropping, watermarking, resizing for objects in formats such as JPG, PNG, BMP, GIF, WebP, and TIFF.
    -   Audio or video transcoding: provides high-quality, high-speed, and parallel audio or video transcoding capabilities. This way, your audio or video files can be played on different terminal devices.
    -   Transfer acceleration over Internet: provides the transfer acceleration service, which uses optimal route selection and protocol stack tuning to reduce timeouts in remote transmission and improve user experience. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).
    -   Accelerated content delivery: uses OSS as the origin with CDN to improve user experience when the same object is repeatedly downloaded.

