# FAQ

This topic provides answers to commonly asked questions about how to get started with OSS.

## General FAQ

-   What is Alibaba Cloud OSS?

    Alibaba Cloud Object Storage Service \(OSS\) is a secure, cost-effective, and scalable storage service that is of high durability and capable of processing large amounts of data. OSS can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995% service availability \(designed for\).

-   What are the features of OSS?

    OSS supports RESTful APIs that are independent of the console. You can store and access any type of data anytime, anywhere, and from any application. OSS is highly scalable. You are charged only for the resources that you use. You can scale OSS resources based on your requirements without compromising performance or durability.

    You can use API operations and SDKs provided by Alibaba Cloud or OSS migration tools to transfer large amounts of data to and from Alibaba Cloud OSS. You can use OSS buckets of the Standard storage class to store image, audio, and video files for apps and large websites. You can use OSS buckets of the Infrequent Access \(IA\), Archive, or Cold Archive storage class to store objects that not accessed often at a low cost.

    For more information about the features of OSS, see [Features](/intl.en-US/Product Introduction/Features.md).

-   Who are target users of OSS?

    OSS is an ideal solution for users who need to store large amounts of data. These users include app and software developers, game development enterprises, and webmasters of online communities, media-sharing sites, and e-commerce websites.

-   What data does OSS store?

    OSS is suitable for storing attachments, high-definition images, audio and video objects, and backup objects for forums, websites, and software applications, as well as objects for a variety of applications, file synchronization software, and online storage systems.

-   What are the advantages of OSS over local storage solutions?

    OSS allows developers to make full use of the economy of scale of Alibaba Cloud at minimal cost without having to make additional investments or affecting performance. Developers can focus on their own innovations without having to worry about the performance bottlenecks and security risks that come with increasing business scales. OSS is cost-effective and easy to use.

-   What is the upper limit of the data volume that OSS can support?

    OSS does not impose limits on the total storage capacity of OSS and the capacity of a bucket. You can use the OSS console to upload an object up to 5 GB in size. To upload an object larger than 5 GB in size, use multipart upload, resumable upload, ossbrowser, or ossutil. For more information, see [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md), [ossbrowser quick start](/intl.en-US/Tools/ossbrowser/Quick start.md), and [ossutil overview](/intl.en-US/Tools/ossutil/Overview.md).

-   What storage classes does OSS provide?

    The storage class of an object. OSS provides the following storage classes to cover a variety of data storage scenarios from hot data to cold data: Standard, IA, Archive, and Cold Archive. OSS Standard storage provides high-reliability, high-availability, and high-performance object storage services that can support frequent data access. OSS IA storage is suitable for storing long-lived but less frequently accessed data \(an average of once or twice per month\). IA storage offers a storage unit price that is lower than Standard storage. OSS Archive storage is suitable for long-term storage \(at least six months\) of infrequently accessed data. OSS Cold Archive storage is suitable for storing extremely cold data over an ultra-long period of time. For more information, see [Introduction to storage classes](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   Will Alibaba Cloud use the data that I store in OSS?

    Alibaba Cloud does not use or release any of your data without authorization. Alibaba Cloud only processes user data based on your service requirements or requirements of laws and regulations. For more information, see [Service terms](https://www.alibabacloud.com/help/doc-detail/42416.htm).

-   Does Alibaba Cloud use OSS to store their own data?

    Yes, Alibaba Cloud developers also use OSS to store authorized data for many projects. These projects rely on OSS to perform key business operations.

-   How does OSS ensure its durability?

    OSS provides developers with a secure, cost-effective, and highly scalable cloud storage infrastructure that is of high durability. Alibaba Cloud also uses this infrastructure to run their websites and networks globally. The OSS Standard and Infrequent Access \(IA\) storage classes are designed to achieve availability of 99.995%. The Archive storage class is designed to achieve availability of 99.99%. All these storage classes are subject to [Service Level Agreement](https://www.alibabacloud.com/help/doc-detail/42438.htm).

-   How does OSS ensure availability when my OSS service experiences bursts of traffic from applications?

    OSS is designed to address bursts of traffic from Internet applications at the beginning. Pay-as-you-go is used as a billing method. To ensure service continuity during bursts of traffic, OSS does not impose limits on capacity. OSS ensures load balancing so that applications are not affected by bursts of traffic.

-   How is OSS data organized?

    OSS is a distributed object storage service that stores data as key-value pairs. When you store an object, you must specify an object name \(key\). The key can then be used to obtain the content of the object.

    Keys can also be used to simulate certain features of folders. OSS uses a flat structure for objects instead of a hierarchical structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. When you use APIs or SDKs to configure an object, you can specify the key value, a full name that includes a prefix for the object to manage other objects. For example, if you set the key of an object to `dir/example.jpg`, a folder named `dir` is created in the current bucket and an object named `example.jpg` is created in the folder. If you delete the `dir/example.jpg` object, the `dir` folder is also deleted.

-   How do I get started with OSS?

    Before you use OSS, ensure that you have created an Alibaba Cloud account and completed real-name verification. If you do not have an Alibaba Cloud account, the system prompts you to create an Alibaba Cloud account when you activate OSS. For more information, see [Create Your Alibaba Cloud Account](https://account.aliyun.com/register/register.htm). After you create the Alibaba Cloud account, open the [Object Storage Service product homepage](https://www.alibabacloud.com/product/oss). Click **Activate OSS**. By default, the billing method after you activate OSS is [Pay-as-you-go](/intl.en-US/Pricing/Billing methods/Pay-as-you-go.md). To further minimize OSS costs, we recommend that you purchase resource plans. For more information, see [OSS Resource Plan](https://common-buy-intl.alibabacloud.com/?spm=a3c0i.7950270.3682168050.4.7174ab91cz9kJX&commodityCode=oss_bag_intl#/buy).

    You can get started with OSS by using the OSS console, graphical management tool, command-line management tool, and SDKs for various programming languages. For more information, see [Get started with OSS](/intl.en-US/Quick Start/Get started with OSS.md).


## FAQ about Alibaba Cloud regions

-   What is an Alibaba Cloud region?

    An Alibaba Cloud region is a geographical region that contains multiple, geographically isolated zones. These zones are connected to each other by using networks that feature short latencies, high throughput, and high redundancy.

-   What is a zone?

    Each region contains several isolated locations known as zones. The power supply and network of each zone are independent. The network latency for access between instances within the same zone is shorter. Zones within the same region can access each other. However, faults that occur in one zone do not affect other zones.

-   How do I determine which region to store my data in?

    When you select a region, we recommend that you consider factors such as physical locations, relationships between cloud services, and resource prices.


## FAQ about billing

-   How does OSS charge fees?

    OSS provides the pay-as-you-go billing method that allows you to pay for resources you use. No minimum usage fees are imposed. You can also purchase resource plans. Resource plans are used to deduct fees incurred by resource usage. In most cases, resource plans are more cost-effective. For more information about prices, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing?spm=a3c0i.7950270.1834322160.1.7bf3ab915anLR3).

-   How does OSS charge fees when other accounts are used to access my OSS resource?

    When other accounts access your OSS resources, OSS charges you based on standard pricing. You can enable the pay-by-requester mode for your bucket so that requesters pay the fees generated by sending requests and downloading OSS data. For more information, see [Enable pay-by-requester](/intl.en-US/Developer Guide/Buckets/Enable pay-by-requester.md).


## FAQ about security

-   Is data stored in OSS secure?

    OSS is secure. By default, only the resource owner can access resources within a bucket. OSS provides user identity verification to control access to data. You can use various access control policies on the bucket or object level, such as ACLs, to grant specific permissions to specific users and user groups. The OSS console displays buckets that can be publicly accessed. You can set the ACL to private if you do not want other users to access your bucket or object. If you set the ACL to public read or public read/write, a message that indicates security risks appears.

-   How do I control access to my OSS data?

    OSS provides multiple methods to control access to objects in a bucket, such as ACL, RAM policies, and bucket policies. For more information, see [Overview of access control](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

-   What data encryption methods does OSS provide?

    OSS provides server-side encryption and client-side encryption.

    -   Server-side encryption encrypts data before the data is stored in the disk of a data center and decrypts data when an object is downloaded. OSS provides the following server-side encryption methods:
    -   Client-side encryption encrypts objects on the client before they are uploaded to OSS. You can use either of the following methods to implement client-side encryption:
        -   Use KMS to manage CMKs

            If you use KMS-managed CMKs for client-side encryption, you do not need to provide keys to the client. You need only to specify CMK IDs when you upload objects.

        -   Use customer-managed CMKs

            This method requires you to generate and manage keys on your own. When you implement client-side encryption, you must upload your symmetric or asymmetric key to the client.


## FAQ about storage management

-   What is OSS lifecycle management? How do I use lifecycle management to minimize OSS storage costs?

    You can configure a lifecycle rule to regularly convert the storage class of non-hot data to IA, Archive, or Cold Archive, and delete data that is no longer accessed, which minimizes human resource or storage costs. Examples:

    -   A medical institution stores their medical records in OSS. These objects are occasionally accessed within six months after they are uploaded, and almost never after that. A lifecycle rule can be used to convert the storage class of these objects to Archive 180 days after they are uploaded.
    -   A company stores the call records of their hotline service in OSS. These objects are frequently accessed within the first two months, occasionally after the two months, and almost never after six months. After two years, these objects no longer need to be stored. A lifecycle rule can be used to convert the storage class of these objects to IA 60 days after they are uploaded and to Archive after 180 days, and delete them 730 days after they are uploaded.
    -   You can manually delete up to 1,000 objects each time. If a bucket contains more than 1,000 objects, you must perform multiple delete operations. You can configure a lifecycle rule that applies to the whole bucket to delete all objects in the bucket after one day.
    For more information about lifecycle management, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

-   What is OSS CRR?

    Cross-region replication \(CRR\) enables the automatic and asynchronous replication of objects across buckets in different OSS regions. This feature synchronizes operations such as the creation, overwrite, and deletion of objects from the source bucket to the destination bucket. This feature can help you implement data replication and provide an ideal cross-region disaster recovery method for buckets. Objects in the destination bucket are extra replicas of objects in the source bucket. They have the same object name, object content, and object metadata such as the creation time, owner, user metadata, and object ACL.

-   How does OSS charge fees when I use CRR?

    After CRR is enabled, cross-region traffic is generated when you replicate objects across buckets in the source and destination regions. You are charged for the traffic that is generated when you use CRR. Each time an object is synchronized, OSS counts the number of requests and calculates the charges on a pay-as-you-go basis. The traffic that is generated when you use CRR can be charged only on a pay-as-you-go basis. Resource plans are unavailable for CRR.


