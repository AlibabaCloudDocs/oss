# FAQ

This topic provides answers to frequently asked questions about Object Storage Service \(OSS\).

## General FAQ

-   What is Alibaba Cloud OSS?

    Alibaba Cloud OSS is a secure, cost-effective, and scalable storage service that has high durability and is capable of processing large amounts of data. OSS supports a designed durability of at least 99.9999999999% \(twelve 9's\) and an availability of at least 99.995%.

-   What are the features of OSS?

    OSS supports RESTful APIs that are independent of the console. You can store and access any type of data anytime, anywhere, and from any application. OSS is highly scalable. You are charged only for the resources that you use. You can scale OSS resources based on your requirements without compromising performance or durability.

    You can call API operations and use SDKs or OSS migration tools provided by Alibaba Cloud to transfer large amounts of data to and from Alibaba Cloud OSS. You can use OSS buckets of the Standard storage class to store image, audio, and video objects for apps and websites. You can use OSS buckets of the Infrequent Access \(IA\), Archive, or Cold Archive storage class to store infrequently accessed objects at a low cost.

    For more information about the features of OSS, see [Features](/intl.en-US/Product Introduction/Features.md).

-   Who are the target users of OSS?

    OSS is an ideal solution for users who need to store large amounts of data. These users include app and software developers, game development enterprises, and webmasters of online communities, media-sharing sites, and e-commerce websites.

-   What data does OSS store?

    OSS is suitable for storing attachments, high-definition images, audio and video objects, and backup objects for forums, websites, and software applications, as well as objects for a variety of applications, file synchronization software, and online storage systems.

-   What are the advantages of OSS over local storage solutions?

    OSS allows developers to make full use of the economy of scale of Alibaba Cloud at minimal cost without additional investments or affecting performance. Developers can focus on their own innovations without performance bottlenecks and security risks that come with increasing business scales. OSS is cost-effective and easy to use.

-   What is the upper limit of the data volume that OSS can support?

    OSS does not impose limits on the total storage capacity and the capacity of a bucket. You can use the OSS console to upload an object up to 5 GB in size. To upload an object larger than 5 GB in size, use multipart upload, resumable upload, ossbrowser, or ossutil. For more information, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md), [Quick start](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md), and [Overview](/intl.en-US/Tools/ossutil/Overview.md).

-   What storage classes does OSS provide?

    OSS provides the following storage classes to cover a variety of data storage scenarios from hot data to cold data: Standard, IA, Archive, and Cold Archive. OSS Standard storage provides high-reliability, high-availability, and high-performance object storage services that can support frequent data access. OSS IA storage is suitable to store infrequently accessed data \(an average of once or twice per month\) that require long-term storage \(at least six months\). In addition, IA storage offers a lower storage unit price than Standard storage. OSS Cold Archive storage is suitable for storing cold data over a long period of time. For more information about the storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   Will Alibaba Cloud use the data that I store in OSS?

    Alibaba Cloud does not use or release any of your data without authorization. Alibaba Cloud only processes user data based on your service requirements or requirements of laws and regulations. Please refer to the [Service terms](https://www.alibabacloud.com/help/doc-detail/42416.htm) for details.

-   Does Alibaba Cloud use OSS to store its data?

    Yes, Alibaba Cloud developers also use OSS to store authorized data for many projects. These projects rely on OSS to perform key business operations.

-   How does OSS ensure availability when my OSS service experiences bursts of traffic from applications?

    OSS is designed to address bursts of traffic from Internet applications. Pay-as-you-go is used as a billing method. To ensure service continuity during bursts of traffic, OSS does not impose capacity limits. OSS ensures load balancing so that applications are not affected by bursts of traffic.

-   How is OSS data organized?

    OSS is a distributed object storage service that stores data as key-value pairs. When you store an object, you must specify an object name \(key\). The key can then be used to obtain the content of the object.

    Keys can also be used to simulate certain features of folders. OSS uses a flat structure for objects instead of a hierarchical structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. When you use APIs or SDKs to configure an object, you can specify the key value, a full name that includes a prefix for the object to manage other objects. For example, if you set the key of an object to `dir/example.jpg`, a folder named `dir` is created in the current bucket and an object named `example.jpg` is created in the folder. If you delete the `dir/example.jpg` object, the `dir` folder is also deleted.

-   How do I get started with OSS?

    Before you use OSS, ensure that you have created an Alibaba Cloud account and completed real-name verification. If you do not have an Alibaba Cloud account, the system prompts you to create an Alibaba Cloud account when you activate OSS. For more information, see [Create Your Alibaba Cloud Account](https://account.aliyun.com/register/register.htm). After you create the Alibaba Cloud account, visit the [Object Storage Service product homepage](https://www.aliyun.com/product/oss). Click **Activate Now**. By default, the billing method after you activate OSS is [Pay-as-you-go](/intl.en-US/Pricing/Billing methods/Pay-as-you-go.md). To further minimize OSS costs, we recommend that you purchase resource plans. For more information, see [Buy OSS resource plans \(subscription\)](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7950270.3682168050.4.7174ab91cz9kJX&commodityCode=oss_bag_intl#/buy).

    You can get started with OSS by using the OSS console, graphical management tool, command-line management tool, and SDKs for various programming languages. For more information, see [Get started with OSS](/intl.en-US/Quick Start/Get started with OSS.md).


## FAQ about Alibaba Cloud regions

-   What is an Alibaba Cloud region?

    An Alibaba Cloud region is a geographical region that contains multiple, geographically isolated zones. These zones are connected to each other by using networks that feature short latency, high throughput, and high redundancy.

-   What is a zone?

    Each region contains several isolated locations known as zones. The power supply and network of each zone are independent. The network latency between instances within the same zone is less. Zones within the same region can access each other. However, faults that occur in one zone do not affect other zones.

-   How do I determine which region to store my data?

    When you select a region, we recommend that you consider factors such as physical locations, relationships between cloud services, and resource prices.


## FAQ about billing

-   How does OSS charge fees?

    OSS provides the pay-as-you-go billing method that allows you to pay for resources you use. No minimum usage fees are imposed. You can also purchase resource plans. Resource plans are used to deduct fees incurred by resource usage. In most cases, resource plans are more cost-effective. For more information about the prices, visit the [OSS pricing page](https://www.alibabacloud.com/product/oss/pricing?spm=a3c0i.7950270.1834322160.1.7bf3ab915anLR3).

-   How does OSS charge fees when other accounts are used to access my OSS resource?

    When other accounts access your OSS resources, OSS charges you based on standard pricing. You can enable the pay-by-requester mode for your bucket so that requesters pay the fees generated by sending requests and downloading OSS data. For more information, see [Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md).


## FAQ about data security and protection

-   Is data stored in OSS secure?

    OSS is secure. By default, only the resource owner can access resources within a bucket. OSS provides user identity verification to control access to data. You can use various access control policies on the bucket or object level, such as access control lists \(ACLs\), to grant specific permissions to specific users and user groups. The OSS console displays buckets that are public. You can set the bucket ACL to private if you do not want other users to access your bucket or object. If you set the bucket ACL to public read or public read/write, a message that indicates security risks appears. For more information about the security of OSS, see [Overview](/intl.en-US/Security and compliance/Overview.md).

-   How do I control access to my OSS data?

    OSS provides multiple access control methods, including ACLs, RAM policies, and bucket policies, for access to objects stored in buckets. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

-   What data encryption methods does OSS provide?

    Server-side encryption: When you upload an object to a bucket for which server-side encryption is enabled, OSS encrypts the object and stores the encrypted object. When you download the encrypted object from OSS, OSS automatically decrypts the object and returns the decrypted object to you. A header is added in the response to indicate that the object is encrypted on the OSS server. For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

    Client-side encryption: Before you upload an object to a bucket, the object is encrypted on the local client. For more information about client-side encryption, see [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md).

-   How do I prevent data stored in buckets from being accidentally deleted or overwritten?

    OSS allows you to configure versioning for a bucket to protect objects stored in the bucket. After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. You can use versioning to recover a previous version of an object that is accidentally overwritten or deleted. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   What is a retention policy?

    OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time. You can configure time-based retention policies for buckets. After a retention policy is configured and locked for a bucket, you can read objects from or upload objects to the bucket. However, objects in the bucket or the retention policy cannot be deleted within the retention period of the retention policy. You can delete the objects only after the retention period expires.

    You can configure retention policies when you need to store infrequently accessed important data that is not allowed to be modified or deleted. Such data includes medical records, technical documents, and contracts. You can store these objects in a specified bucket and configure a retention policy for the bucket.


## FAQ about data replication

-   How do I replicate data from a bucket to another bucket in a different region?

    You can configure multiple cross-region replication rules for a bucket to store multiple copies of the data in different regions. Cross-region replication \(CRR\) provides automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as creating, overwriting, and deleting objects can be synchronized from a source bucket to a destination bucket.

-   What are the advantages of CRR?
    -   Compliance requirements: Although OSS stores multiple replicas of each object in physical disks, replicas must be stored at a distance from each other to comply with regulations. CRR allows you to replicate data between geographically distant OSS data centers to satisfy these compliance requirements.
    -   Minimum latency: You have users who are located in two geographical locations. To minimize the latency when the users access objects, you can maintain replicas of objects in OSS data centers that are geographically closer to these users.
    -   Data backup and disaster recovery: You have high requirements for data security and availability, and want to explicitly maintain replicas of all written data in a second data center. If one OSS data center is damaged in a catastrophic event such as an earthquake or a tsunami, you can use backup data from the other data center.
    -   Data replication: For business reasons, you may need to migrate data from one OSS data center to another data center.
    -   Operational reasons: You have compute clusters deployed in two different data centers that need to analyze the same group of objects. You can choose to maintain object replicas in these regions.
-   How does OSS charge fees when I use CRR?

    After CRR is enabled, cross-region traffic is generated when you replicate objects across buckets in the source and destination regions. You are charged for the traffic that is generated when you use CRR. Each time an object is synchronized, OSS calculates the number of requests and charges the requests in a pay-as-you-go method. The traffic that is generated when you use CRR can be charged only on a pay-as-you-go basis. Resource plans are unavailable for CRR.


## FAQ about data query

How do I query data in OSS?

OSS supports the SelectObject operation that allows you to use SQL statements to query specific data in a CSV or JSON object instead of querying the entire object. The SelectObject operation simplifies the process used to query data and filter the data into smaller and more specific data sets. Therefore, this operation is suitable for the multipart query of large objects, query of JSON objects, and analysis of log objects. For more information about data query, see [SelectObject](/intl.en-US/Developer Guide/Objects/Manage files/SelectObject.md).

## FAQ about storage management

-   What is OSS lifecycle management? How do I use lifecycle management to minimize OSS storage costs?

    You can configure a lifecycle rule to regularly delete objects that are no longer accessed or convert the storage class of cold data to IA, Archive, or Cold Archive. This makes data management easier and saves storage costs. For example, you can configure lifecycle rules in the following scenarios:

    -   A medical institution stores its medical records in OSS. These objects are occasionally accessed within six months after they are uploaded, and almost never after that. You can configure a lifecycle rule to convert the storage class of these objects to Archive 180 days after they are uploaded.
    -   A company stores the call records of its service hotline in OSS. These objects are frequently accessed within the first two months, occasionally after two months, and almost never after six months. After two years, these objects no longer need to be stored. You can configure a lifecycle rule to convert the storage class of these objects to IA 60 days after they are uploaded and to Archive 180 days after they are uploaded, and delete them 730 days after they are uploaded.
    -   You can manually delete up to 1,000 objects each time. If a bucket contains more than 1,000 objects, you must delete the objects multiple times. To delete all objects in the bucket at the same time, you can configure a lifecycle rule that deletes all objects in the bucket after one day.
    For more information about lifecycle management, see [Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md).

-   How do I regularly obtain the information about objects stored in a bucket?

    You can use the bucket inventory feature to export the information about specific objects in a bucket on a daily or weekly basis. Exported object information includes the number, sizes, storage classes, and encryption status of the objects. For more information about the bucket inventory feature, see [Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).


