# What is OSS?

Object Storage Service \(OSS\) is a secure, cost-effective, and high-durability cloud storage service provided by Alibaba Cloud. It enables you to store large amounts of data in the cloud. OSS can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995% service availability \(designed for\).

OSS supports RESTful API operations that are independent of the console. You can store and access data from any application, anytime and anywhere.

You can call API operations and use SDKs or OSS migration tools provided by Alibaba Cloud to transfer large amounts of data to and from Alibaba Cloud OSS. You can use OSS buckets of the Standard storage class to store image, audio, and video objects for apps and websites. You can use OSS buckets of the Infrequent Access \(IA\), Archive, or Cold Archive storage class to store infrequently accessed objects at a low cost.

## Get started with OSS

-   FAQ

    For frequently asked questions about OSS, see [FAQ](/intl.en-US/Product Introduction/FAQ.md).

-   Learning path

    You can visit [OSS Learning Path](https://www.alibabacloud.com/getting-started/learningpath/oss) to get started with OSS. You can learn basic OSS operations, and then perform secondary development by using a variety of API operations, SDKs, and convenient tools.


## Terms

-   storage class

    OSS provides the following storage classes to cover a variety of data storage scenarios from hot data to cold data: Standard, IA, Archive, and Cold Archive. OSS Standard storage provides high-reliability, high-availability, and high-performance object storage services that can support frequent data access. OSS IA storage is suitable to store infrequently accessed data \(an average of once or twice per month\) that require long-term storage \(at least six months\). In addition, IA storage offers a lower storage unit price than Standard storage. OSS Cold Archive storage is suitable for storing cold data over a long period of time. For more information about the storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).

-   bucket

    A bucket is a container used to store objects in OSS. All objects are contained in buckets. You can configure a variety of bucket attributes such as the region, ACL, and storage class. You can create buckets of different storage classes to store your data.

-   object

    Objects are the basic unit for data storage in OSS. Objects are also known as files. An object consists of object metadata, object data, and an object key. The key of an object unique and identifies the object in a bucket. Object metadata consists of key-value pairs that describe object attributes, such as the last modified time and size of the object. You can also specify the user metadata of an object.

-   region

    The region of a bucket indicates the physical location of the OSS data center in which the bucket is located. The region of a bucket indicates the physical location of the OSS data center in which the bucket is located. You can select a region to create a bucket based on the cost and location of the requests to the bucket. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   endpoint

    Endpoints are domain names used to access OSS. OSS uses HTTP RESTful APIs to provide services. Different regions are accessed by using different endpoints. For the same region, access over the internal network or over the Internet also uses different endpoints. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   AccessKey pair

    An AccessKey pair consists of an AccessKey ID and an AccessKey secret. AccessKey pairs are used for access authentication. OSS uses symmetric encryption based on AccessKey pairs to authenticate the identities of requesters. The AccessKey ID is used to identify a user. The AccessKey secret is used to encrypt and verify signature strings. The AccessKey secret must be kept confidential. For more information about how to obtain an AccessKey pair, see [Create an AccessKey pair]().


## Common operations

-   Create buckets

    Before you can upload objects to OSS, you must create a bucket. The attributes of a bucket include the region, ACL, and other metadata. For more information about how to create a bucket, see [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md).

-   Upload objects

    After you create a bucket, you can upload objects of different sizes to the bucket. For more information about how to upload objects, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).

-   Download objects

    After you upload objects to a bucket, you can download the objects to the default download path of your browser or a specified local path. For more information about how to download objects, see [Download objects](/intl.en-US/Quick Start/OSS console/Download objects.md).

-   List objects

    You can list all objects in a bucket. If a large number of objects are stored in a bucket, you can also list part of the objects in the bucket. For more information about how to list objects, see [List objects](/intl.en-US/Developer Guide/Objects/Manage files/List objects.md).

-   Delete objects

    You can manually delete a single object or multiple objects that you no longer need. You can also configure lifecycle rules to allow OSS to automatically delete a single object or multiple objects. For more information about how to delete objects, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md).


## Features

-   Versioning

    OSS allows you to configure versioning for a bucket to protect objects stored in the bucket. After you enable versioning for a bucket, objects that are overwritten or deleted in the bucket are saved as previous versions. You can use versioning to recover a previous version of an object that is accidentally overwritten or deleted. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   Bucket policy

    The owner of a bucket can configure bucket policies to grant users different permissions to access the specified OSS resources in the bucket. For example, you can configure bucket policies to authorize other Alibaba Cloud accounts or anonymous users to access or manage all resources or part of resources in your bucket. You can also configure bucket policies to grant different permissions such as read-only, read/write, or complete management to the RAM users of the same Alibaba Cloud account. This way, the RAM users have different permissions to access or manage resources in your bucket. For more information about how to configure bucket policies, see [Configure bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md).

-   CRR

    Cross-region replication \(CRR\) provides automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as creating, overwriting, and deleting objects can be synchronized from a source bucket to a destination bucket. CRR can meet your requirements on geo-disaster recovery and data replication. For more information about CRR, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).

-   Data encryption

    Server-side encryption: When you upload an object to a bucket for which server-side encryption is enabled, OSS encrypts the object and stores the encrypted object. When you download the encrypted object from OSS, OSS automatically decrypts the object and returns the decrypted object to you. A header is added in the response to indicate that the object is encrypted on the OSS server. For more information about server-side encryption, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

    Client-side encryption: Before you upload an object to a bucket, the object is encrypted on the local client. For more information about client-side encryption, see [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md).


## Manage OSS

You can upload objects, download objects, and manage OSS by using a variety of methods.

-   Manage OSS in the OSS console

    OSS provides a web-based console. You can log on to the [OSS console](https://oss.console.aliyun.com/overview) to manage your OSS resources. For more information, see [Use Alibaba Cloud accounts to log on to the OSS console](/intl.en-US/Console User Guide/OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md).

-   Manage OSS by using APIs or SDKs

    OSS provides RESTful API operations and SDKs for multiple programming languages to facilitate secondary development. For more information, see [API Reference](/intl.en-US/API Reference/API overview.md) and [SDK Reference](SDK Referencet22258.dita#concept_dcn_tp1_kfb).

-   Manage OSS by using tools

    OSS provides multiple management tools, such as ossbrowser, ossutil, and ossftp. For more information, see [OSS tools](/intl.en-US/Tools/OSS tools.md).

-   Manage OSS by using Cloud Storage Gateway

    OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. To use OSS in a manner similar to local file systems, you can configure Cloud Storage Gateway \(CSG\). For more information, visit the [CSG product homepage](https://www.alibabacloud.com/product/cloud-storage-gateway).


## Pricing

Traditional service providers require you to purchase a fixed amount of storage capacity and network traffic in advance. If your storage capacity or traffic exceeds the amount, your service is deactivated or you are charged excess fees. At the same time, if your storage capacity or traffic does not exceed the amount, you are charged for the full amount of the storage and traffic.

OSS charges you only for the storage capacity and traffic that you consume. Therefore, you do not need to purchase storage capacity and traffic in advance. You can take the cost advantages of the flexible infrastructure provided by Alibaba Cloud to grow your business.

For more information about OSS pricing, visit [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss/pricing). For more information about OSS billing methods, see [Overview](/intl.en-US/Pricing/Billing items and methods/Overview.md).

## Other services

After you upload your data to OSS, you can use other Alibaba Cloud services to manage your data.

The following services are frequently used with OSS:

-   Image Processing \(IMG\): a service that allows you to perform a variety of operations such as format converting, resizing, cropping, rotating, and adding watermarks to images stored in OSS. For more information, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).
-   Elastic Compute Service \(ECS\): a cloud computing service that offers elastic and efficient computing capability. For more information, visit the [ECS product homepage](https://www.alibabacloud.com/product/ecs).
-   Alibaba Cloud CDN: a distributed network that caches resources from an origin server to edge nodes in different regions to accelerate content delivery. For more information, visit the [CDN product homepage](https://www.alibabacloud.com/product/cdn).
-   E-MapReduce \(EMR\): a big data processing system solution built on ECS. EMR is based on Apache Hadoop and Apache Spark to facilitate data analysis and processing. For more information, visit the [E-MapReduce product homepage](https://www.alibabacloud.com/product/e-mapreduce).
-   ApsaraVideo for Media Processing: a service that converts audio or video objects stored in OSS into files that are suitable for playback on PCs, TVs, and mobile devices. Based on the deep learning results of large amounts of data, ApsaraVideo for Media Processing performs multimodal analysis on the content, text, audio, and scenario of audio or video files. Based on the analysis result, ApsaraVideo for Media Processing can intelligently audit, understand, and edit the content of the audio or video files. For more information, visit the [ApsaraVideo for Media Processing product homepage](https://www.alibabacloud.com/product/mts).
-   Data Online Migration: a service that allows you to migrate data from a third-party storage service such as Amazon Web Services \(AWS\) and Google Cloud to OSS. For more information, see [Data Online Migration documentation](https://www.alibabacloud.com/help/product/94157.htm).
-   Data Transport: a service that helps you migrate a large amount of data from local storage to OSS. For example, you can use Data Transport to migrate terabytes or petabytes of local data to OSS when the local network bandwidth is insufficient and expansion costs are high. For more information, see [What is Data Transport?](/intl.en-US/Product Introduction/What is Data Transport?.md)

## Alibaba Cloud storage services

In addition to OSS, Alibaba Cloud also provides a variety of storage services such as file storage and block storage for different scenarios. For more information about Alibaba Cloud storage services, see [Overview]().

For more information about use cases and solutions of Alibaba Cloud storage services, see [Alibaba Cloud Storage](https://www.alibabacloud.com/product/storage).

