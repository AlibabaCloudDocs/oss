# Get started with OSS

Alibaba Cloud Object Storage Service \(OSS\) provides you with network-based data storage and access services. OSS enables you to store and retrieve a variety of data objects, such as texts, images, audios, and videos over the network at any time.

Before you use OSS, we recommend that you familiarize yourself with OSS usage limits. For more information, see [Limits](/intl.en-US/Product Introduction/Limits.md).

OSS uploads data files as objects to a bucket. You can perform the following operations in OSS:

-   Create one or more buckets and upload one or more objects to each bucket.
-   Share or download an object by using its URL assigned by OSS.
-   Configure the ACL for a bucket or object by modifying its attributes or metadata.
-   Perform basic and advanced OSS operations in the OSS console or by using various convenient tools and SDKs.

## Quick start

The following flowchart shows the process of basic operations in OSS.

![quickstart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1801646061/p140852.png)

1.  [Activate OSS](/intl.en-US/Quick Start/Sign up for OSS.md)
2.  [Create buckets](/intl.en-US/Quick Start/Create buckets.md)
3.  [Upload objects](/intl.en-US/Quick Start/Upload objects.md)
4.  [Download objects](/intl.en-US/Quick Start/Download objects.md)

## Use the console

You can perform basic and advanced OSS operations in the OSS console. For more information, see [OSS Console User Guide](/intl.en-US/Console User Guide/Log on to the OSS console/Use Alibaba Cloud accounts to log on to the OSS console.md).

## Use ossbrowser

ossbrowser is a graphical object management tool for OSS. This tool supports the Windows, Linux, and macOS. You can use ossbrowser to view, upload, and download objects and folders, upload or download objects in resumable mode, or perform policy-based authorization by using GUI. ossbrowser serves a desktop graphical tool whose transmission speeds and performance are not as high as ossutil. For more information, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).

## Use ossutil

ossutil allows you to use command lines to manage OSS data and supports the following operating systems: Windows, Linux, and macOS. ossutil provides simple commands to efficiently manage buckets and objects and supports concurrent upload tasks. ossutil supports the upload, download, and resumable transfer of objects and folders. For more information, see [Quick start](/intl.en-US/Tools/ossutil/Overview.md).

## Use APIs and SDKs

OSS provides APIs and SDK packages in multiple programming languages such as Java, Python, PHP, and Go to facilitate secondary development. For more information, see [OSS SDKs](/intl.en-US/SDK Reference/Introduction.md). For more information about OSS APIs, see [OSS API Reference](/intl.en-US/API Reference/Description.md).

## OSS-based file system management

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. To use OSS in a way similar to local folders and disks, you can configure Cloud Storage Gateway \(CSG\). Buckets in OSS are mapped to local folders or disks by using NFS, SMB \(CIFS\), or iSCSI provided by CSG. You can access OSS resources by reading and writing files, which allows you to use applications compliant with POSIX and protocols for block-based access. For more information, see [Configure Cloud Storage Gateway](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure CSG.md).

## What to do next

For more information about advanced OSS operations, see [OSS Developer Guide](/intl.en-US/Developer Guide/Terms.md).

