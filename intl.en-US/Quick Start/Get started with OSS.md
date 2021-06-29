# Get started with OSS

Alibaba Cloud Object Storage Service \(OSS\) provides you with network-based data storage and access services. OSS enables you to store and retrieve a variety of objects, such as texts, images, audio files, and videos over the network at any time.

## Use the OSS console

You can create a bucket and upload objects to the bucket in the OSS console. After you upload objects to a bucket, you can download the objects to local disks or generate signed URLs for the objects to share the objects with third parties for download or preview. For more information, see [Operations in the OSS console](/intl.en-US/Quick Start/OSS console/Operations in the OSS console.md).

## Use ossbrowser

ossbrowser is a graphical object management tool for OSS. This tool supports Windows, Linux, and macOS. You can use ossbrowser to manage buckets, upload and download objects and folders, and authorize users based on policies in the GUI. For more information, see [ossbrowser](/intl.en-US/Quick Start/ossbrowser.md).

ossbrowser is a graphical desktop tool with lower transmission speed and performance than ossutil.

## Use ossutil

ossutil is a tool that allows you to use command lines to manage OSS data and supports the following operating systems: Windows, Linux, and macOS. ossutil provides simple commands to manage buckets and objects in an efficient manner. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Use API operations and SDKs

OSS provides API operations and SDK packages in a variety of programming languages such as Java, Python, PHP, and Go to facilitate secondary development. For demos of OSS SDKs for a variety of programming languages, see [Introduction](/intl.en-US/SDK Reference/Introduction.md). For more information about the OSS API operations, see [OSS API Reference](/intl.en-US/API Reference/Description.md).

## OSS-based file system management

OSS does not use a hierarchical structure for objects, but instead uses a flat structure. All elements are stored as objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. To use OSS in a manner similar to local folders and disks, you can configure Cloud Storage Gateway \(CSG\). CSG uses the NFS, SMB \(CIFS\), or iSCSI protocol to map OSS buckets to local folders or disks. By using CSG, you can access OSS resources by reading and writing files in local folders or disks. This way, you can use applications compliant with POSIX and protocols for block-based access to reduce the cost for application modification and learning. For more information, see [Use CSG to attach OSS to ECS](/intl.en-US/Console User Guide/Upload, download, and manage objects/Use CSG to attach OSS to ECS.md).

