# OSS tools

In addition to the OSS console, you can use the following tools to streamline OSS operations.

|Tool|Description|
|:---|:----------|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|A graphical object management tool. -   Provides an easy-to-use graphical interface.
-   Provides features similar to those of Windows Explorer.
-   Allows you to browse objects.
-   Allows you to upload and download directories \(folders\).
-   Allows you to use concurrent upload and resumable upload to upload objects.
-   Allows you to configure authorization policies and grant permissions to RAM users.
-   Supports Windows, Linux, and macOS.

Limits:

-   The transmission speed and performance of ossbrower are not as good as those of ossutil because ossbrower is a graphical tool.
-   Only objects smaller than 5 GB can be uploaded or replicated.
-   Objects larger than 48.8 TB cannot be uploaded. |
|[ossutil](/intl.en-US/Tools/ossutil/Overview.md)|A command line tool used to manage objects and buckets. -   Provides a wide range of simple and convenient commands to manage objects and buckets while ensuring high operation performance.
-   Allows you to use concurrent upload and resumable upload to upload objects.
-   Allows you to upload and download directories \(folders\). |
|[osscmd \(unavailable\)](/intl.en-US/Tools/osscmd (unavailable)/Overview.md)|A command line tool used to manage objects and buckets. -   Provides a wide range of commands to manage objects and buckets.
-   Supports Windows and Linux.

Limits:

-   osscmd is compatible with Python 2.5, 2.6, and 2.7 only. It is not compatible with Python 3.x.
-   osscmd does not allow you to configure new features such as the storage class of Infrequent Access \(IA\), Archive, Cold Archive, cross-region replication \(CRR\), and back-to-origin.

**Note:** Commands supported by osscmd have been integrated into [ossutil](/intl.en-US/Tools/ossutil/Overview.md). osscmd is no longer available for downloads as of July 31, 2019. |
|[ossfs](/intl.en-US/Tools/ossfs/Overview.md)|A tool used to attach a bucket to the local file system. After you attach OSS buckets to the local file system of Linux, you can perform operations on the objects in OSS through the local file system to access or share these objects.

-   Supports most functions provided by POSIX-based file systems, including file read/write, directories, symbolic links, permissions, UIDs or GIDs, and extended attributes.
-   Allows you to use multipart upload to upload large objects.
-   Supports MD5 verification to ensure data integrity.

Limits: -   You cannot attach a bucket of the Archive and Cold Archive storage class.
-   If you use ossfs to edit an uploaded object, the object is uploaded again.
-   The performance of metadata-related operations, such as `list directory`, is compromised because you must remotely access the OSS server.
-   Errors may occur when you rename an object or a folder. Operation failures may cause data inconsistency.
-   ossfs is not suitable for scenarios that require highly concurrent read and write operations.
-   If an OSS bucket is attached to multiple clients, you are responsible for maintaining data consistency. For example, you must schedule when users can use objects to prevent multiple clients from writing to the same object at the same time.
-   Hard links are not supported. |
|[ossftp](/intl.en-US/Tools/ossftp/Use ossftp.md)|An FTP-based tool used to manage objects in OSS. -   You can use FTP clients such as FileZilla, WinSCP, and FlashFXP to manage objects in OSS.
-   ossftp is an FTP server that receives FTP requests and performs operations on objects and folders in OSS.
-   ossftp is based on Python 2.7 and later.
-   Supports Windows, Linux, and macOS. |
|[ossimport](/intl.en-US/Tools/ossimport/Architectures and configurations.md)|A tool used to synchronize data to OSS. -   Allows you to synchronize data from a third-party data source to OSS.
-   Supports distributed deployment. You can use multiple servers to migrate data simultaneously.
-   Supports migrating TB-grade data volumes and above.
-   Supports Windows and Linux.
-   Applicable to Java 1.7 and 1.8. |
|[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)|A tool used to achieve automatic generation of OSS-related permission policies. -   Generates permission policies based on specified information. You can add the generated policies to the [custom policy](https://ram.console.aliyun.com/policies/new) in the RAM console.
-   Supports the following browsers: Google Chrome, Firefox, and Safari.

**Note:** We recommend that you use this tool to generate custom authorization policies. |

