# Features

This topic lists the common features of OSS.

Before you use OSS, we recommend that you have a good understanding of the basic terms used in OSS, including buckets, objects, regions, and endpoints. For more information, see [Terms](/intl.en-US/Developer Guide/Terms.md).

The following table lists the features provided by OSS.

|Scenario|Description|Reference|
|:-------|:----------|:--------|
|Upload objects|Before you upload objects to OSS, you must create a bucket in an Alibaba Cloud region to store your objects. After you create a bucket, you can upload objects to the bucket.|-   [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md)
-   [Create folders](/intl.en-US/Console User Guide/Upload, download, and manage objects/Create folders.md)
-   [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md) |
|Search for objects|You can search for objects or folders in a bucket.|[Search for objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Search for objects.md)|
|View, share, and download objects|You can view, share, and download objects by using their URLs.|[Download objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md)|
|Delete objects or folders|You can delete objects or folders in OSS buckets. You can also delete parts generated in multipart upload to save storage space.|-   [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md)
-   [Delete folders](/intl.en-US/Console User Guide/Upload, download, and manage objects/Delete folders.md)
-   [Manage parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md) |
|Automatically delete multiple objects at a specified point in time|You can specify and manage the lifecycle of specific objects or all objects in a bucket. For example, you can delete an object or convert it to a less expensive storage class after a specified period of time.|[Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md)|
|Accelerate data upload and download|You can use transfer acceleration to reduce the amount of time required for your businesses to deliver services to your customers and improve customer experience.|[Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md)|
|Recover accidentally deleted data|You can use versioning to recover overwritten or deleted data.|[Configure versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md)|
|Zone-disaster recovery|OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data still remains accessible. This feature can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995%service availability.|[Zone-redundant storage](/intl.en-US/Developer Guide/Data security/Disaster recovery/Zone-redundant storage.md)|
|Geo-disaster recovery|You can use cross-region replication \(CRR\) to synchronize operations performed on data such as create, update, and delete operations from the source bucket to the destination bucket. This way, you can implement geo-disaster recovery.|[Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md)|
|Data retention compliance|You can create a retention policy to set the data retention period. Your data cannot be deleted by any users within this period.|[Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md)|
|Control access to data|You can control access to OSS resources by using the following methods: -   ACL: You can set access control lists \(ACLs\) for buckets and objects, including public read/write, public read, and private.
-   Bucket policy: You can use bucket policies to authorize other users to access your OSS resources by using the console. For example, you can authorize RAM users of other Alibaba Cloud accounts to access your OSS resources, and authorize anonymous users to access your OSS resources from specific IP addresses.
-   RAM policy: You can create RAM policies to control access to buckets and folders. OSS provides RAM Policy Editor to generate required RAM policies. For more information, see [RAM Policy Editor](/intl.en-US/Tools/RAM Policy Editor.md).
-   STS temporary authorization: You can use Alibaba Cloud Security Token Service \(STS\) to grant a third-party application or RAM user a temporary access credential that has a custom validity period.
-   Hotlink protection: You can configure a Referer whitelist to prevent unauthorized users from accessing your OSS resources.

|-   [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md)
-   [Configure ACL for objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure ACL for objects.md)
-   [Bucket Policy](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md)
-   [RAM Policy](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md)
-   [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md)
-   [Configure hotlink protection](/intl.en-US/Developer Guide/Data security/Access and control/Configure hotlink protection.md) |
|Encrypt data|You can use client-side and server-side encryption to encrypt your data before you store the data in OSS.|-   [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md)
-   [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md) |
|Manage data based on the data category|You can configure tags to manage data based on the data category:-   Configure bucket tags: You can manage data based on bucket tags. For example, you can list buckets that have specified tags and configure the ACL for buckets that have specified tags.
-   Configure object tags: You can manage data based on object tags. For example, you can configure lifecycle rules for objects that have specified tags and configure the ACL for objects that have specified tags.

|-   [Configure bucket tagging](/intl.en-US/Developer Guide/Buckets/Configure bucket tagging.md)
-   [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md) |
|Record access to OSS resources|You can enable logging to record the detailed information about access to OSS resources.|[Logging](/intl.en-US/Developer Guide/Manage logs/Logging.md)|
|Access OSS resources by using custom domain names|You can bind a custom domain name to an OSS bucket and then use the custom domain name to access data in the bucket. If you want to use your custom domain name to access OSS by using HTTPS, you can host your SSL certificate in OSS.|-   [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md)
-   [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md) |
|Static website hosting|You can configure static website hosting for your bucket and use the bucket endpoint to access this static website.|[Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md)|
|CORS|OSS provides Cross-origin resource sharing \(CORS\) based on HTML5. CORS allows client web applications that are loaded in one domain name to interact with resources in another domain name.|[Configure CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md)|
|Obtain content from the origin|You can configure back-to-origin rules to specify whether to obtain source data by using mirroring or redirection. You can configure back-to-origin rules for hot migration and specific request redirection.|[Manage back-to-origin configurations](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md)|
|View object metadata|You can configure bucket inventory rules to export the metadata of a specified object including the object size and encryption status.|[Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md)|
|Modify HTTP headers|OSS allows you to configure HTTP headers to customize HTTP request policies such as the cache policy and forced object download policy.|[Configure object HTTP headers](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object HTTP headers.md)|
|View resource usage|You can view OSS service usage information such as the status and performance of the basic system operation in real time.|[Overview of the monitoring service](/intl.en-US/Developer Guide/Monitoring service/Overview.md)|
|Traffic throttling|You can configure single-connection bandwidth throttling for upload, download, and copy operations on OSS to ensure sufficient bandwidth for other applications.|[Single-connection bandwidth throttling](/intl.en-US/Developer Guide/Objects/Single-connection bandwidth throttling.md)|
|Analyze and process data|You can analyze and process data stored in OSS:-   Image Processing \(IMG\): You can perform operations such as format conversion, cropping, scaling, rotating, watermarking, and style encapsulation on images stored in OSS.
-   Capture video snapshots: You can capture images from video objects in the H.264 format.
-   Intelligent Media Management \(IMM\): OSS uses IMM to support various data analysis and processing operations such as document preview, document format conversion, facial recognition, image analysis, and QR code recognition.

|-   [IMG](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md)
-   [Capture video snapshots](/intl.en-US/Developer Guide/Data Processing/Capture video snapshots.md)
-   [IMM](/intl.en-US/Developer Guide/Data Processing/Intelligent Media Management (IMM)/Quick start.md) |
|Use tools to manage OSS resources|OSS provides graphical, CLI, file mounting, and FTP tools for you to manage OSS resources.|[OSS tools](/intl.en-US/Tools/OSS tools.md)|
|Use SDKs to manage OSS resources|OSS provides SDKs for a variety of programming languages to facilitate secondary development.|[SDK sample code](SDK sample codet22258.dita#concept_dcn_tp1_kfb)|

