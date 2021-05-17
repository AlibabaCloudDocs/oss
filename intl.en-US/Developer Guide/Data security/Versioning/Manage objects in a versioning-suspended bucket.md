# Manage objects in a versioning-suspended bucket

Object Storage Service \(OSS\) allows you to suspend versioning for a bucket so that no additional versions are generated for the objects in the bucket. When versioning is suspended for a bucket, you can upload objects to the bucket and download or delete previous versions of objects in the bucket by specifying the version IDs.

## Upload objects

When you upload an object to a versioning-suspended bucket, OSS stores the uploaded object as a version whose ID is null. Each object in a versioning-suspended bucket has only one version whose ID is null.

-   In the following figure, when you use the PutObject operation to upload an object to a versioning-suspended bucket, OSS stores the uploaded object as a version whose ID is null.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8595688951/p40340.jpg)

-   In the following figure, an object named example.jpg in a versioning-suspended bucket has a version whose ID is 111111. When you use the PutObject operation to upload an object that has the same name as the existing object, OSS stores the uploaded object as the current version of example.jpg and assigns the ID null for the current version. Version 111111 of the object is saved as a previous version.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8595688951/p40343.jpg)

-   In the following figure, an object named example.jpg in a versioning-suspended bucket has a version whose ID is null. When you use the PutObject operation to upload an object that has the same name, the version whose ID is null is overwritten.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8595688951/p40352.jpg)


You can use the [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md) command provided by ossutil and the following OSS SDKs to upload objects to a versioning-suspended bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Upload objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Upload objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Upload objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Upload objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Upload objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Upload objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Upload objects.md).

## Download objects

OSS allows you to download the current or specified version of an object from a versioning-suspended bucket.

The following examples describe how an object is downloaded from a versioning-suspended bucket when a version ID is specified and not specified in the GetObject request:

-   By default, if no version ID is specified in the request, the current version of the object is returned. In the following figure, the current version of the object, whose ID is null, is returned.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8595688951/p40355.jpg)

-   If you specify a version ID in the GetObject request, the specified version of the object is returned. In the following figure, the version whose ID is 222222 is returned.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9595688951/p40356.jpg)


You can use the [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Download objects.md) command provided by ossutil and the following OSS SDKs to download objects from a versioning-suspended bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Download objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Download objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Download objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Download objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Download objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Download objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Download objects.md).

## Delete objects

The following examples describe how an object is deleted from a versioning-suspended bucket in different scenarios:

-   When you use the DeleteObject operation to delete an object whose current version ID is not null, OSS adds a delete marker whose ID is null as the current version of the object.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9595688951/p40362.jpg)

-   When you use the DeleteObject operation to delete an object whose current version ID is null, OSS adds a delete marker whose ID is null as the current version of the object. An object can have only one version whose ID is null. Therefore, the original current version of the object is overwritten by the newly-added delete marker whose ID is null.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9595688951/p40363.jpg)

-   If you specify a version ID in the DeleteObject request, the specified version of the object is permanently deleted. In the following figure, the version whose ID is 333333 is deleted.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9595688951/p40360.jpg)


You can use the [rm](/intl.en-US/Tools/ossutil/Common commands/rm.md) command provided by ossutil and the following OSS SDKs to delete objects from a versioning-suspended bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Delete objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Delete objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Delete objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Delete objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Delete objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Delete objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Delete objects.md).

