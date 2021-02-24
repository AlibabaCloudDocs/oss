# What is OSS?

Object Storage Service \(OSS\) is a secure, cost-effective, and high-durability cloud storage service provided by Alibaba Cloud. It enables you to store large amounts of data in the cloud. OSS can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995% service availability.

OSS supports RESTful APIs independent from the console. You can store and access any type of data anytime, anywhere, and from any application.

You can use API operations and SDKs provided by Alibaba Cloud or OSS migration tools to transfer large amounts of data to and from Alibaba Cloud OSS. You can use OSS buckets of the Standard storage class to store image, audio, and video files for apps and large websites. You can use OSS buckets of the Infrequent Access \(IA\), Archive, or Cold Archive storage class to store objects that not accessed often at a low cost.

## Terms

-   storage class

    The storage class of an object. OSS provides the following storage classes to cover a variety of data storage scenarios from hot data to cold data: Standard, IA, Archive, and Cold Archive. OSS Standard storage provides high-reliability, high-availability, and high-performance object storage services that can support frequent data access. OSS IA storage is suitable for storing long-lived but less frequently accessed data \(an average of once or twice per month\). IA storage offers a storage unit price that is lower than Standard storage. OSS Archive storage is suitable for long-term storage \(at least six months\) of infrequently accessed data. OSS Cold Archive storage is suitable for storing extremely cold data over an ultra-long period of time. For more information, see [Introduction to storage classes](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   bucket

    The container used to store objects in OSS. All objects are contained in buckets. You can configure a variety of bucket properties such as the region, ACL, and storage class. You can create buckets of different storage classes to store data based on your requirements. For more information about how to create a bucket, see [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md).

-   object

    The basic unit for data operations in OSS. Objects are also known as files. Each object has metadata, data, and a key. The key is the unique object name in a bucket. Object metadata describes object attributes such as the last modified time and the object size in the key-value pairs format. You can also specify custom metadata of an object.

-   region

    The physical location of an OSS data center. You can select the region in which to create your buckets based on the cost and the location where requests come from. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   endpoint

    The domain name used to access OSS. OSS uses HTTP RESTful APIs to provide services. Different regions are accessed by using different endpoints. For the same region, access over the internal network or over the Internet also uses different endpoints. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   AccessKey pair

    The key pair consists of an AccessKey ID and an AccessKey secret. The AccessKey pair is used to verify access identities. OSS uses an AccessKey pair, which includes an AccessKey ID and an AccessKey secret to implement symmetric encryption and verify the identity of a requester. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt and verify the signature string. The AccessKey secret must be kept confidential. For more information about how to obtain an AccessKey pair, see [Create an AccessKey pair]().


## Related services

After you upload your data to OSS, you can use other Alibaba Cloud products and services to perform operations on the data.

The following services are frequently used with OSS:

-   Image Processing \(IMG\): a service that allows you to perform a variety of operations such as format converting, resizing, cropping, rotating, and adding watermarks on images stored in OSS. For more information, see [Image processing](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).
-   Elastic Compute Service \(ECS\): a cloud computing service that offers elastic and efficient computing capability. For more information, see [ECS product homepage](https://www.alibabacloud.com/product/ecs).
-   Alibaba Cloud CDN: a content delivery service that caches the resources from a source site to edge nodes in different regions for quick access. For more information, see [CDN product homepage](https://www.alibabacloud.com/product/cdn).
-   E-MapReduce \(EMR\): a big data processing system solution built on ECS. EMR is based on Apache Hadoop and Apache Spark to facilitate data analysis and processing. For more information, see [E-MapReduce product homepage](https://www.alibabacloud.com/product/e-mapreduce).
-   ApsaraVideo for Media Processing: a service that converts an audio or video file into one or more audio or video files. ApsaraVideo for Media Processing helps you produce audios and videos that are suitable for for playback on PCs, TVs, and mobile devices. Based on deep learning of large amounts of data, ApsaraVideo for Media Processing performs multi-model analysis based on the content, text, audio, and scenario of audio or video files. It is able to intelligently detect, understand, and edit content. For more information, see [ApsaraVideo for Media Processing product homepage](https://www.alibabacloud.com/product/mts).

## Manage OSS

-   Manage OSS in the OSS console

    OSS provides a web-based console. You can log on to the [OSS console](https://oss.console.aliyun.com/overview) to manage your OSS resources. For more information, see [Console User Guide](/intl.en-US/Console User Guide/OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md).

-   Manage OSS by using APIs or SDKs

    OSS provides RESTful API operations and SDK packages for multiple programming languages to facilitate secondary development. For more information, see [API Reference](/intl.en-US/API Reference/API overview.md) and [SDK Reference](SDK Referencet22258.dita#concept_dcn_tp1_kfb).

-   Manage OSS by using tools

    OSS provides various tools for you to manage your OSS resources. For more information, see [Tools](/intl.en-US/Tools/OSS tools.md).


## OSS pricing

Traditional storage providers require you to purchase a fixed amount of storage and network transfer capacity in advance. If the amount and capacity is exceeded, your service is disabled or you are charged excess fees. If you do not fully utilize your capacity, you are charged as if you had.

OSS charges you only for the storage space and bandwidth that you actually consume. You can take the cost advantages of the flexible infrastructure from Alibaba Cloud as your business grows and your storage and bandwidth requirements change.

For more information about OSS pricing, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss#pricing). For more information about OSS billing methods, see [Billing items and methods](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Learning path

You can visit the [OSS Learning Path](https://www.alibabacloud.com/getting-started/learningpath/oss) to get started with OSS, learn the basic OSS operations, and perform secondary development by using a variety of API operations, SDKs, and convenient tools.

