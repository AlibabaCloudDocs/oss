# OSS tools

The following table lists the tools that can be used to streamline OSS operations.

|Tool|Description|
|:---|:----------|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md)|A graphical object management tool. -   Provides an easy-to-use graphical interface.
-   Provides features similar to those of Windows Explorer.
-   Allows you to browse objects.
-   Allows you to upload and download directories \(folders\).
-   Allows you to use concurrent upload and resumable upload to upload objects.
-   Allows you to configure authorization policies and grant permissions to RAM users.
-   Supports Windows, Linux, and macOS.

Limits:

-   The transmission speed and performance of ossbrower are not as good as those of ossutil because ossbrower is a graphical tool.
-   Only objects smaller than 5 GB can be moved or replicated.
-   Objects larger than 48.8 TB cannot be uploaded. |
|[ossutil](/intl.en-US/Tools/ossutil/Overview.md)|A command-line tool used to manage objects and buckets. -   Provides a wide range of simple and convenient commands to manage objects and buckets while high performance of operations is ensured.
-   Allows you to use concurrent upload and resumable upload to upload objects.
-   Allows you to upload and download directories \(folders\). |
|[osscmd \(unavailable\)](/intl.en-US/Tools/osscmd (unavailable)/Overview.md)|A command-line tool used to manage objects and buckets. -   Provides a wide range of commands to manage objects and buckets.
-   Supports Windows and Linux.

Limits:

-   osscmd is compatible only with Python 2.5, 2.6, and 2.7. osscmd is incompatible with Python 3.x.
-   osscmd does not allow you to configure new features such as the storage class of Infrequent Access \(IA\) or Archive, Cold Archive, cross-region replication \(CRR\), and mirroring-based back-to-origin.

**Note:** The osscmd operation commands have been integrated into [ossutil](/intl.en-US/Tools/ossutil/Overview.md). osscmd has been unavailable since July 31, 2019. Alibaba Cloud regrets the inconvenience caused, and appreciates your patience and understanding. |
|[ossfs](/intl.en-US/Tools/ossfs/Overview.md)|A tool used to attach a bucket to the local file system. After you attach OSS buckets to the local file system of Linux, you can perform operations on the objects in OSS to access or share these objects by using the local file system. -   Supports most features of the POSIX-compliant file systems, including file reading/writing, directories, link-related operations, permissions, UID or GID, and extended attributes.
-   Allows you to use multipart upload to upload large objects.
-   Supports MD5 verification to ensure data integrity.

Limits:-   Archive and Cold Archivebuckets cannot be attached to local file systems by using ossfs.
-   If you use ossfs to edit an object that was uploaded, the object is uploaded again.
-   The performance of metadata-related operations such as `list directory` is compromised because you must remotely access the OSS server.
-   Errors may occur if you rename an object or a folder. Operation failures may cause data inconsistencies.
-   ossfs is unsuitable for scenarios that require highly concurrent read and write operations.
-   If an OSS bucket is attached to multiple clients, you are responsible for maintaining data consistency. We recommend that you schedule the time when your users can use objects, which prevents multiple clients from writing to the same object at the same time.
-   Hard links are not supported.

**Note:** We recommend that you use Cloud Storage Gateway \(CSG\) to attach buckets. For more information, see [Use CSG to attach OSS to ECS](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use CSG to attach OSS to ECS.md). |
|[ossftp](/intl.en-US/Tools/ossftp/Overview.md)|An FTP-based tool used to manage objects in OSS. -   You can use FTP clients such as FileZilla, WinSCP, and FlashFXP to manage objects in OSS.
-   ossftp is an FTP server that receives FTP requests and performs operations on objects and folders in OSS.
-   ossftp is based on Python 2.7 and later.
-   ossftp supports Windows, Linux, and macOS. |
|[ossimport](/intl.en-US/Tools/ossimport/Architectures and configurations.md)|A tool used to synchronize data to OSS.-   Allows you to synchronize data from a third-party data source to OSS.
-   Supports distributed deployment. You can use multiple servers to migrate data simultaneously.
-   Supports migration of more than terabytes of data.
-   Supports Windows and Linux.
-   Applicable to Java 7 and Java 8. |
|[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)|A tool used to automatically generate OSS-related authorization policies. We recommend that you use this tool to generate custom authorization policies.-   Generates an authorization policy based on the specified requirements. You can call this operation to create a custom policy. Then, the policy can be added to the [custom policy](https://ram.console.aliyun.com/policies/new) in the RAM console.
-   Supports the following browsers: Google Chrome, Firefox, and Safari. |

