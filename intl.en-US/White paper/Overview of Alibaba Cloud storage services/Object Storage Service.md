# Object Storage Service

Object Storage Service \(OSS\) is a secure, cost-effective, and high-durability cloud storage service provided by Alibaba Cloud. It enables you to store a large amount of data in the cloud. OSS supports a designed durability of at least 99.9999999999% \(twelve 9's\) and a designed availability of at least 99.995%. OSS supports RESTful APIs independent from the console. You can store and access any type of data anytime, anywhere, and from any application.

[OSS](https://common-buy.aliyun.com/?spm=5176.7933691.1309819..68542a669quNbn&commodityCode=oss) provides the following storage classes to cover various data storage scenarios from hot data storage to cold data storage: Standard, Infrequent Access \(IA\), Archive, and Cold Archive.

|Standard|Standard storage provides high-durability, high-availability, and high-performance object storage services that can handle frequent data access. Standard storage is ideal for storing images for social networking and sharing, storing data for audio and video applications, large websites, and big data analytics.|
|Infrequent Access \(IA\)|IA storage is suitable for data that is accessed infrequently, such as only once or twice a month. IA storage offers a storage unit price lower than that of Standard storage and is suitable for long-term data backup of mobile apps, smart devices, and enterprises. It also supports real-time data access.|
|Archive|Archive storage is suitable for long-term \(at least six months\) storage of data that is infrequently accessed. OSS takes up to one minute to restore an Archive object before it can be read. Archive storage is suitable for data that you want to store for a long period of time such as archival data, medical images, scientific materials, and video footage.|
|Cold Archive|Cold Archive storage is suitable for extremely cold data that you want to store for an extremely long period of time. Examples: data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, media resources that are retained in the film and television industries, and archived videos from the online education industry.|

## Scenarios

OSS is applicable to the following scenarios:

-   Storage and distribution of audio, video, and static website data

    Each OSS object can be accessed through a unique HTTP URL, which can be used to distribute data. In addition, OSS buckets can be used as the origins for Alibaba Cloud Content Delivery Network \(CDN\). The storage space of OSS is not partitioned. Therefore, OSS is ideal to store the data of user-driven and data-intensive websites, such as image and video sharing websites. Various devices, websites, and mobile applications can directly read data from or write data to OSS. You can write data to OSS by uploading files or using streams.

-   Static website hosting

    As a cost-efficient, high-availability, and highly scalable storage solution, OSS can be used to store static HTML files, images, videos, and client scripts such as JavaScript scripts.

-   Data warehouse for computing and analysis

    OSS provides high scalability to simultaneously access data from multiple computing nodes and prevent you from being restricted by the performance of a single node.

-   Data backup and archiving

    OSS provides a highly available, scalable, secure, and reliable solution for the backup and archiving of critical data. You can configure lifecycle rules to automatically convert the storage class of cold data to IA or Archive to reduce storage costs. You can also configure cross-region replication \(CRR\) rules to automatically replicate data between buckets in different regions in an asynchronous manner in near real-time .


## Performance

To achieve the fastest speed when you access an OSS bucket from your ECS instance, the ECS instance must be located within the same region as the bucket. Because of how OSS is designed, the latency at the server side is negligible compared with the network latency. In addition, OSS can support a large number of web applications because it can be scaled based on the storage capacity, number of requests, and number of users. If you use multiple threads, applications, or clients to simultaneously access OSS, the total throughput is scaled to a value far higher than the throughput that can be provided or consumed by a single server.

OSS provides the multiple upload feature to improve the upload speed of objects larger than 5 GB in size. By using multipart upload, you can split an object into multiple data blocks \(parts\) and upload the parts separately. After all parts are uploaded, OSS combines these parts into a complete object. You can use this method to implement resumable upload. Multipart upload is suitable for scenarios in which network connectivity is poor. If a part fails to be uploaded, you can reupload only the failed part instead of the complete object.

To accelerate data access, many developers use OSS in conjunction with search engines such as OpenSearch or databases such as Tablestore and RDS for MySQL. OSS is used to store data, while search engines or databases are used to store metadata, such as the object names, object sizes, and keywords. Metadata stored in databases can be indexed and queried. You can use OSS in conjunction with search engines or databases to locate and query objects stored in OSS.

OSS provides [transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md) to accelerate the upload and download of large objects from a distance, which allows you to dynamically update objects and accelerate the download of objects that are not frequently accessed. Transfer acceleration provides an end-to-end acceleration solution by combining smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations.

OSS supports Alibaba Cloud Content Delivery Network \(CDN\) to accelerate the download of static hot objects. Alibaba Cloud CDN uses OSS buckets as origins and distributes content from the origins to edge nodes. Alibaba Cloud CDN uses its precise scheduling system to distribute requests to optimal edge nodes, which allows users to quickly access the required content. This way, Internet traffic congestion can be eased and the response time is shortened.

## Data durability and service availability

The data durability and service availability of Standard and IA storage classes are ensured by automatic synchronization. OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data can still remain accessible. This mechanism can provide 99.9999999999% \(twelve 9's\) data durability and 99.995% data availability.

You can also enable CRR for a bucket. After CRR is enabled, data is replicated between buckets in different regions in an asynchronous and nearly real-time manner. The source bucket and destination bucket can both provide 99.9999999999% \(twelve 9's\) data durability and 99.995% data availability.

## Scalability and elasticity

OSS provides high scalability and elasticity. You may encounter problems if you store excessive files in the same directory in a common file system. In contrast, the total storage capacity of OSS and the capacity of a single bucket are not limited. You can store an unlimited number of objects in a bucket. OSS stores the copies of your data in different servers in the same region. All copies have the same performance provided by the high-performance infrastructure of Alibaba Cloud.

## Security

OSS provides a variety of security features such as server-side encryption, client-side encryption, hotlink protection based on Referer whitelists, fine-grained access control, log audit, and retention policies based on WORM. OSS is the only cloud service in China that meets the audit and certification requirements of Cohasset Associates and the specific requirements for electronic data storage. OSS buckets configured with retention policies can be used for business subject to regulations such as SEC Rule 17a-4\(f\), CFTC Rule 1.31\(c\)-\(d\), and FINRA Rule 4511\(c\). For more information, see [Overview](/intl.en-US/White paper/Security and compliance/Overview.md).

## Operations

OSS provides standard RESTful API operations. You can use these API operations to store objects in buckets whose names are globally unique. Each object has a key that uniquely identifies the object in the bucket. OSS does not use folders. All elements are stored as objects. However, you can create a simulated folder by creating an object whose name ends with a forward slash \(/\), such as folder1/folder2/file.

You can use tools or OSS SDKs that encapsulate the API operations to develop applications. OSS provides [SDKs](/intl.en-US/SDK Reference/Introduction.md) for more than 10 programming languages, including Java, Python, PHP, Go, Android, and iOS. [ossutil](/intl.en-US/Tools/ossutil/Overview.md) is a high-performance command-line tool that provides a variety of simple commands, such as ls, cp, cat, and config, for you to manage buckets and objects. ossutil supports concurrent upload and is supported by the following operating systems: Windows, Linux, and macOS. In addition, you can use the graphic tool [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md) to perform basic operations, such as browsing objects and uploading or downloading objects and folders. You can also use the [OSS console](/intl.en-US/Console User Guide/Log on to the OSS console/Overview of the OSS console.md), which is a graphic web application, to manage your OSS resources.

You can use the event notification feature to immediately learn about the operations performed on your OSS resources. For more information, see [Event notification](/intl.en-US/Developer Guide/Event notification.md).

## Cost model

OSS fees include storage fees, traffic fees, API operation calling fees, data processing fees. OSS charges fees based on the actual amount of resources used. The fees incurred within an hour are billed in the following hour. Fees are calculated based on the following formula: Fees = Actual usage × Unit price. You can use the subscription billing method for some billing items to reduce costs. Subscription allows you to use resources only after you purchase resource plans. Resource plans are used to deduct fees incurred by resource usage. For more information, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

