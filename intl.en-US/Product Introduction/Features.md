# Features

OSS provides secure, cost-effective, and high-durability services for you to store large amounts of data in the cloud. This topic lists the common scenarios and corresponding features of OSS. You can select the best solution based on your business requirements.

Before you use OSS, we recommend that you have a good understanding of the basic terms used in OSS, including buckets, objects, regions, and endpoints. For more information, see [Terms](/intl.en-US/Developer Guide/Terms.md).

The following table lists the features provided by OSS.

|Scenario|Description|Reference|
|:-------|:----------|:--------|
|Upload objects|Before you upload objects to OSS, you must create a bucket in an Alibaba Cloud region to store your objects. After you create a bucket, you can upload objects to the bucket.|-   [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md)
-   [Create folders](/intl.en-US/Console User Guide/Upload, download, and manage objects/Create folders.md)
-   [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md) |
|Search for objects|OSS allows you to search for objects and folders and find the object you want to access in a bucket.|[Search for objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Search for objects.md)|
|View, share, and download objects|OSS allows you to use the signed URL of an object to view, share, and download the object.|[Download objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md)|
|Delete objects or folders|OSS allows you to delete one or more objects, folders, and parts at a time. You can delete expired objects at regular intervals to save storage space.|-   [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md)
-   [Delete folders](/intl.en-US/Console User Guide/Upload, download, and manage objects/Delete folders.md)
-   [Manage parts](/intl.en-US/Console User Guide/Upload, download, and manage objects/Manage parts.md) |
|Automatically delete multiple objects at a specified point in time|OSS supports lifecycle rules. You can configure lifecycle rules to periodically convert the storage class of non-hot data to Infrequent Access \(IA\), Archive, or Cold Archive, and delete expired data.|[Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md)|
|Accelerate the upload and download of data|OSS supports transfer acceleration. Transfer acceleration uses optimal route selection and protocol stack tuning to reduce timeouts in remote transmission and improve user experience.|[Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md)|
|Recover accidentally deleted data|OSS supports versioning. When you enable versioning, OSS saves the overwritten and deleted objects as previous versions. Versioning allows you to recover objects in a bucket to any previous version, and prevents data loss caused by accidental deletion or overwriting of objects.|[Configure versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md)|
|Zone-disaster recovery|OSS uses the multi-zone mechanism to distribute user data across three zones within the same region. Even if one zone becomes unavailable, the data still remains accessible. This feature can provide 99.9999999999% \(twelve 9's\) data durability \(designed for\) and 99.995% service availability.|[ZRS](/intl.en-US/Developer Guide/Data security/Disaster recovery/ZRS.md)|
|Geo-disaster recovery|OSS supports cross-region replication \(CRR\). You can use CRR to synchronize operations performed on data such as create, update, and delete operations from the source bucket to the destination bucket in a different zone. This way, you can implement geo-disaster recovery.|[Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md)|
|Data retention compliance|OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time.|[Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md)|
|Control access to data|OSS supports flexible authorization and authentication mechanisms. You can control access to OSS resources by using the following methods: -   ACL: You can set access control lists \(ACLs\) for buckets and objects, including public read/write, public read, and private.
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
|Encrypt data|OSS supports client-side and server-side encryption. You can select an encryption method to encrypt and store your data in OSS.|-   [Client-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Client-side encryption.md)
-   [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md) |
|Manage data by category|OSS allows you to configure tagging to manage data based on the data category:-   Configure bucket tags: You can manage data based on bucket tags. For example, you can list buckets that have specified tags and configure ACL for buckets that have specified tags.
-   Configure object tags: You can manage data based on object tags. For example, you can configure lifecycle rules for objects that have specified tags and configure ACL for objects that have specified tags.

|-   [Configure bucket tagging](/intl.en-US/Developer Guide/Buckets/Configure bucket tagging.md)
-   [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md) |
|Record the access information for OSS resources|OSS supports logging. You can configure logging to audit operations, collect access statistics, track exceptions, and troubleshoot problems in OSS.| |
|Use custom domain names to access OSS resources|OSS allows you to bind custom domain names to OSS buckets, and use custom domain names to access data in the buckets. If you want to use your custom domain name to access OSS by using HTTPS, you can host your SSL certificate in OSS.|-   [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md)
-   [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md) |
|Configure static website hosting|OSS supports static website hosting. You can configure static website hosting for your bucket and access static websites by using the bucket domain name.|[Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md)|
|Cross-origin resource sharing \(CORS\)|OSS supports CORS in HTML5. CORS allows client web applications that are loaded in one domain to interact with resources in another domain.|[Configure CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md)|
|Obtain data from the origin|OSS supports back-to-origin configurations. If your user accesses data in a bucket that has no back-to-origin rules configured and the data does not exist, 404 Not Found is returned. However, if you configure back-to-origin rules that contain the correct origin URL, your user can obtain the data based on the back-to-origin rules. You can configure back-to-origin rules for hot data migration and specific request redirection.|[Manage back-to-origin configurations](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md)|
|View object metadata|OSS supports the inventory feature. You can configure inventory rules for buckets to export the metadata of specified objects including the object sizes and encryption status.|[Bucket inventory](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md)|
|Modify HTTP headers|OSS allows you to modify the HTTP headers of objects. You can configure HTTP headers to customize HTTP request policies such as the cache policy and forced object download policy.|[Configure object HTTP headers](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object HTTP headers.md)|
|View resource usage|OSS supports the monitoring feature. You can use the monitoring feature to view real-time information about OSS service usage such as the running status and performance of the system.|[Overview of the monitoring service](/intl.en-US/Developer Guide/Monitoring service/Overview.md)|
|Traffic throttling|OSS supports single-connection bandwidth throttling. You can configure single-connection bandwidth throttling for upload, download, and copy operations on OSS to ensure sufficient bandwidth for other applications.|[Single-connection bandwidth throttling](/intl.en-US/Developer Guide/Objects/Single-connection bandwidth throttling.md)|
|Analyze and process data|OSS supports Image Processing \(IMG\) and video snapshot capturing for you to analyze and process data stored in OSS:-   IMG: You can perform operations such as format conversion, cropping, scaling, rotating, watermarking, and style encapsulation on images stored in OSS.
-   Capture video snapshots: You can capture images from video objects in the H.264 format.
-   Intelligent Media Management \(IMM\): OSS uses IMM to support various data analysis and processing operations such as document preview, document format conversion, facial recognition, image analysis, and QR code recognition.

|-   [IMG](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md)
-   [Capture video snapshots](/intl.en-US/Developer Guide/Data Processing/Capture video snapshots.md)
-   [IMM](/intl.en-US/Developer Guide/Data Processing/Intelligent Media Management (IMM)/Quick start.md) |
|Use tools to manage OSS resources|OSS provides graphical, CLI, file mounting, and FTP tools for you to manage OSS resources.|[OSS tools](/intl.en-US/Tools/OSS tools.md)|
|Use SDKs to manage OSS resources|OSS provides SDKs for a variety of programming languages to facilitate secondary development.|[SDK sample code](SDK sample codet22258.dita#concept_dcn_tp1_kfb)|

