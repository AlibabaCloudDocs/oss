# Get started with OSS

Alibaba Cloud Object Storage Service \(OSS\) provides you with network-based data storage and access services. OSS enables you to store and retrieve a variety of objects, such as texts, images, audio files, and videos over the network at any time.

**Note:** Before you use OSS, we recommend that you see the introduction documents to familiarize yourself with the basic concepts, scenarios, and limits of OSS.

You can upload a variety of files as objects to an OSS bucket. You can perform the following operations in OSS:

-   Create buckets and upload objects to buckets.
-   Share or download an object by using its URL assigned by OSS.
-   Use access control lists \(ACLs\), bucket policies, and RAM policies to authorize and authenticate users who access OSS.
-   Perform basic and advanced OSS operations in the OSS console or by using various convenient tools and SDKs.

## Quick start

The following flowchart shows the process of basic operations in OSS.

![quickstart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1801646061/p140852.png)

1.  [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md)
2.  [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md)
3.  [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md)
4.  [Download objects](/intl.en-US/Quick Start/OSS console/Download objects.md)

## Use the OSS console

You can perform basic and advanced OSS operations in the OSS console. For more information, see [OSS Console User Guide](/intl.en-US/Console User Guide/Log on to the OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md).

## Use ossbrowser

ossbrowser is a graphical object management tool for OSS. This tool supports Windows, Linux, and macOS. You can use ossbrowser to manage buckets, upload and download objects and folders, and authorize users based on policies in the GUI. For more information about ossbrowser, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).

ossbrowser is a graphical desktop tool with lower transmission speed and performance than ossutil.

## Use ossutil

ossutil is a tool that allows you to use command lines to manage OSS data and supports the following operating systems: Windows, Linux, and macOS. ossutil provides simple commands to manage buckets and objects in an efficient manner. ossutil supports the following operations: creation and management of buckets, upload and download of objects and folders, resumble upload and download, and concurrent upload. For more information about ossutil, see [Quick start](/intl.en-US/Tools/ossutil/Overview.md).

## Use APIs and SDKs

OSS provides APIs and SDK packages in a variety of programming languages such as Java, Python, PHP, and Go to facilitate secondary development. For demos of OSS SDKs for a variety of programming languages, see [Introduction](/intl.en-US/SDK Reference/Introduction.md). For more information about the OSS APIs, see [OSS API Reference](/intl.en-US/API Reference/Description.md).

## OSS-based file system management

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS conceptually supports folders to group objects and simplify management. To use OSS in a manner similar to local folders and disks, you can configure Cloud Storage Gateway \(CSG\). CSG uses the NFS, SMB \(CIFS\), or iSCSI protocol to map OSS buckets to local folders or disks. By using CSG, you can access OSS resources by reading and writing files in local folders or disks. This way, you can use applications compliant with POSIX and protocols for block-based access to reduce the cost for application modification and learning. For more information, see [Configure CSG](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure CSG.md).

## References

For more information about advanced OSS operations, see [OSS Developer Guide](/intl.en-US/Developer Guide/Terms.md).

