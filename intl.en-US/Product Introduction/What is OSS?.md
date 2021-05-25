# What is OSS?

Object Storage Service \(OSS\) is a secure, cost-effective, and high-durability cloud storage service provided by Alibaba Cloud. It enables you to store large amounts of data in the cloud. OSS can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995% service availability \(designed for\).

OSS supports RESTful API operations that are independent of the console. You can store and access types of data anytime, anywhere, and from applications.

You can use API operations and SDKs provided by Alibaba Cloud or OSS migration tools to transfer large amounts of data to and from Alibaba Cloud OSS. You can use OSS buckets of the Standard storage class to store image, audio, and video objects for apps and large websites. You can use OSS buckets of the Infrequent Access \(IA\), Archive, or Cold Archive storage class to store infrequently accessed objects at a low cost.

## Obtain the details of OSS

-   FAQ

    For more information about questions that other users frequently ask and are concerned with, see [FAQ](/intl.en-US/Product Introduction/FAQ.md).

-   Learning path

    You can visit the [OSS Learning Path](https://www.alibabacloud.com/getting-started/learningpath/oss) to get started with OSS, learn the basic OSS operations, and then perform secondary development by using a variety of API operations, SDKs, and convenient tools.


## Migrate data to OSS

You can use Alibaba Cloud Data Online Migration to migrate data from a third-party storage service to OSS or between OSS buckets, such as Amazon Web Services \(AWS\) and Google Cloud. For more information, see [Data Online Migration](https://www.alibabacloud.com/help/product/94157.htm).

Assume that you must migrate terabytes or petabytes of data to OSS. You can use the Data Offline Migration \(Lighting Cube\) service when the insufficient local low-bandwidth and high expansion costs are taken into consideration. For more information, see [What is Data Transport?](/intl.en-US/Product Introduction/What is Data Transport?.md)

## Terms

-   storage class

    The storage class of an object. OSS provides the following storage classes to cover a variety of data storage scenarios from hot data to cold data: Standard, IA, Archive, and Cold Archive. OSS Standard storage provides high-reliability, high-availability, and high-performance object storage services that can support frequent data access. OSS IA storage is suitable for storing long-lived, but less frequently accessed data \(an average of once or twice per month\). IA storage offers a storage unit price that is lower than Standard storage. OSS Archive storage is suitable for long-term storage \(at least six months\) of infrequently accessed data. OSS Cold Archive storage is suitable for storing extremely cold data over an ultra-long period of time. For more information about storage types, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   bucket

    The container used to store objects in OSS. All objects are contained in buckets. You can configure a variety of bucket attributes such as the region, ACL, and storage class. You can create buckets of different storage classes to store data based on your requirements.

-   object

    The basic unit for data operations in OSS. Objects are also known as files. Each object has metadata, data, and a key. The key is the unique object name in a bucket. Object metadata describes object attributes such as the last modified time and the object size in the key-value pairs format. You can also specify custom metadata of an object.

-   region

    The physical location of an OSS data center. You can select the region in which to create your buckets based on the cost and the location where requests come from. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   endpoint

    The domain name used to access OSS. OSS uses HTTP RESTful APIs to provide services. Different regions are accessed by using different endpoints. For the same region, access over the internal network or over the Internet also uses different endpoints. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   AccessKey pair

    The key pair consists of an AccessKey ID and an AccessKey secret. The AccessKey pair is used to verify access identities. OSS uses an AccessKey pair, which includes an AccessKey ID and an AccessKey secret to implement symmetric encryption and verify the identity of a requester. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt and verify the signature string. The AccessKey secret must be kept confidential. For more information about how to obtain an AccessKey pair, see [Create an AccessKey pair]().


## Manage OSS

OSS provides multiple and flexible methods to upload, download, and manage objects.

-   Manage OSS in the OSS console

    OSS provides a web-based console. You can log on to the [OSS console](https://oss.console.aliyun.com/overview) to manage your OSS resources. For more information, see [Use Alibaba Cloud accounts to log on to the OSS console](/intl.en-US/Console User Guide/OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md).

-   Manage OSS by using APIs or SDKs

    OSS provides RESTful API operations and SDK packages for multiple programming languages to facilitate secondary development. For more information, see [API Reference](/intl.en-US/API Reference/API overview.md) and [SDK Reference](SDK Referencet22258.dita#concept_dcn_tp1_kfb).

-   Manage OSS by using tools

    OSS provides multiple types of management tools, such as ossbrowser, ossutil, and ossftp. For more information, see [OSS tools](/intl.en-US/Tools/OSS tools.md).

-   Manage OSS by using Cloud Storage Gateway

    Cloud Storage Gateway \(CSG\): OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS conceptually supports folders to group objects and simplify management. To use OSS in a manner similar to local folders and disks, you can configure CSG. For more information, see [Cloud Storage Gateway](https://www.alibabacloud.com/product/cloud-storage-gateway).


## OSS pricing

Traditional storage providers require you to purchase a fixed amount of storage and network transfer capacity in advance. If the amount and capacity is exceeded, your service is disabled or you are charged excess fees. If you do not fully utilize your capacity, you are charged as if you had.

OSS charges you only for the storage space and bandwidth that you actually consume. You can take the cost advantages of the flexible infrastructure from Alibaba Cloud as your business grows and your storage and bandwidth requirements change.

For more information about OSS pricing, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss#pricing). For more information about OSS billing methods, see [Overview](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Alibaba Cloud storage service

Additionally, Alibaba Cloud also provides storage services such as file storage and block storage in different scenarios. For more information, see [Alibaba Cloud Storage Overview]().

For more information about use cases and solutions of Alibaba Cloud storage services, see [Alibaba Cloud Storage](https://www.alibabacloud.com/product/storage).

## Other services

After you upload your data to OSS, you can use other Alibaba Cloud products and services to manage the data.

The following services are frequently used with OSS:

-   Image Processing \(IMG\): a service that allows you to perform a variety of operations such as format converting, resizing, cropping, rotating, and adding watermarks to images stored in OSS. For more information, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).
-   Elastic Compute Service \(ECS\): a cloud computing service that offers elastic and efficient computing capability. For more information, see the [Elastic Compute Service page](https://www.alibabacloud.com/product/ecs).
-   Alibaba Cloud CDN: a content delivery service that caches the resources from a source site to edge nodes in different regions for quick access. For more information, see the [CDN product page](https://www.alibabacloud.com/product/cdn).
-   E-MapReduce \(EMR\): a big data processing system solution built on ECS. EMR is based on Apache Hadoop and Apache Spark to facilitate data analysis and processing. For more information, see [the E-MapReduce page](https://www.alibabacloud.com/product/e-mapreduce).
-   ApsaraVideo for Media Processing: a service that converts an audio or video file into one or more audio or video files. ApsaraVideo for Media Processing helps you produce audios and videos that are suitable for playback on PCs, TVs, and mobile devices. Based on deep learning of large amounts of data, ApsaraVideo for Media Processing performs multi-model analysis based on the content, text, audio, and scenario of audio or video files. It is able to intelligently detect, understand, and edit content. For more information, see the [ApsaraVideo Media Processing page](https://www.alibabacloud.com/product/mts).

