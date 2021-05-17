# Manage objects in a versioning-enabled bucket

When versioning is enabled for a bucket, Object Storage Service \(OSS\) generates a unique ID for each version of all objects in the bucket. The content and access control list \(ACL\) of existing objects in the bucket remain unchanged. Versioning prevents your data from being accidentally overwritten or deleted and allows you to query or recover previous versions of objects.

## Usage notes

Take note of the following items when you perform the following operations in versioned buckets: upload objects, list objects, download objects, delete objects, and recover objects.

-   When versioning is enabled for a bucket, a current version and its previous versions are stored for each object in the bucket.
-   The version ID of an object that is uploaded before versioning is enabled is set to null.
-   For ease of viewing, all version IDs in the following figures are in the simple format.

For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## Upload objects

When you upload an object to a versioned bucket, OSS generates a unique version ID for the object.

**Note:** OSS generates unique version IDs for objects uploaded by using PutObject, PostObject, CopyObject, and MultipartUpload.

In the following figure, when you use the PutObject operation to upload an object whose key is example.jpg, OSS generates a unique version ID of 111111 for the object.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40175.jpg)

When you use the PutObject operation to upload an object that has the same key as the existing object example.jpg, OSS generates a new version with a unique version ID of 222222 for the object and stores the new version as the current version of the object. Version 111111 is stored as a previous version. When you use the PutObject operation to upload an object that has the same key again, OSS generates a new version with a unique version ID of 333333 for the object and stores the new version as the current version of the object. In the following figure, versions 111111 and 222222 are stored as previous versions.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40176.jpg)

You can use the [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md) command provided by ossutil and the following SDKs to upload objects to versioned buckets: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Upload objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Upload objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Upload objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Upload objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Upload objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Upload objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Upload objects.md).

## List objects

You can call the GetBucketVersions \(ListObjectVersions\) operation to obtain the information of all object versions and delete markers in a versioned bucket.

-   Unlike GetBucketVersions \(ListObjectVersions\), the GetBucket \(ListObject\) operation returns only the current object versions that are not delete markers in a bucket.
-   Up to 1,000 object versions can be returned for a single GetBucketVersions \(ListObjectVersions\) request. You can send multiple GetBucketVersions \(ListObjectVersions\) requests to obtain all object versions in a versioned bucket.

    For example, if a bucket contains two objects whose names are example.jpg and photo.jpg. The example.jpg object has 900 versions. The photo.jpg object has 500 versions. If you send a GetBucketVersions \(ListObjectVersions\) request, the 900 versions of the example.jpg object and 100 versions of the photo.jpg object are returned. Versions are returned in alphabetical order of object keys first and then in the order of time when the versions are generated.


In the following figure, all object versions including delete markers are returned when the GetBucketVersions \(ListObjectVersions\) operation is called. Only current object versions that are not delete markers are returned when the GetBucket\(ListObject\) operation is called. Therefore, only the current version of the photo.jpg, whose version ID is 444444, is returned.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40404.jpg)

You can use the [ls](/intl.en-US/Tools/ossutil/Common commands/ls (list).md) command provided by ossutil and the following OSS SDKs to list objects in a versioned bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/List objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/List objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/List objects.md), and [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/List objects.md).

## Download objects

You can download the current or a specified version of an object from a versioned bucket.

By default, the current version of an object is returned if a GetObject request in which no version ID is specified is sent to download the object. In the following figure, the current version of the object, whose version ID is 333333, is returned.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40220.jpg)

If the current version of the object to download is a delete marker, 404 Not Found is returned for the GetObject request.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40666.jpg)

To download the specified version of an object, you must specify the version ID in the GetObject request. In the following figure, a request in which a version ID is specified is sent to download the object version whose ID is 222222.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6595688951/p40221.jpg)

You can use the [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Download objects.md) command provided by ossutil and the following OSS SDKs to download objects from a versioned bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Download objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Download objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Download objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Download objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Download objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Download objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Download objects.md).

## Delete objects

You can specify an object version ID in the DeleteObject request or configure lifecycle rules to permanently delete a version of an object in a versioned bucket. If you do not specify an object version ID in the DeleteObject request, OSS adds a delete marker as the current version of the object.

**Note:** By default, if you do not specify a version ID in the DeleteObject request, the current version and previous versions of the object are not deleted.

In addition, you can configure the Expiration element in lifecycle rules to delete the expired current version of objects in a versioned bucket. You can also configure the NoncurrentVersionExpiration element in lifecycle rules to permanently delete the expired previous versions of objects in a versioned bucket. The two elements have the following differences:

-   The Expiration element is specified in lifecycle rules to delete the expired current version of objects. When an object is deleted based on a lifecycle rule with Expiration configured, OSS stores the current version of the object as a previous version and adds a delete marker as the new current version of the object.
-   The NoncurrentVersionExpiration element is specified in lifecycle rules to delete expired previous versions of objects. A previous version deleted based on a lifecycle with NoncurrentVersionExpiration configured is permanently deleted and cannot be recovered.

For more information about how to use versioning with lifecycle rules, see [Configuration elements](/intl.en-US/Developer Guide/Objects/Object lifecycle/Configuration elements.md).

The following examples describe how OSS deletes an object when the version ID is specified and not specified in the DeleteObject request.

-   If the version ID is not specified in the DeleteObject request, OSS adds a delete marker as the current version of the object to delete. The delete marker has a unique version ID but does not have data and ACL. In the following figure, a delete marker with the version ID of 444444 is added as the current version.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7595688951/p40237.jpg)

-   If the version ID is specified in the DeleteObject request, the specified version is permanently deleted. In the following figure, the version whose ID is 333333 is permanently deleted.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7595688951/p40245.jpg)


You can use the [rm](/intl.en-US/Tools/ossutil/Common commands/rm.md) command provided by ossutil and the following OSS SDKs to delete objects from a versioned bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Delete objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Delete objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Delete objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Delete objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Delete objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Delete objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Delete objects.md).

## Recover objects

When versioning is enabled for a bucket, all versions of objects in the bucket are preserved. You can recover the specified previous version of an object as the current version.

You can recover a previous version of an object as the current version by using the following two methods:

-   Use CopyObject to copy the previous version to the same bucket

    The copied previous version becomes the current version of the object and all versions of the object are preserved.

    In the following figure, a previous version whose ID is 222222 is copied to the same bucket. OSS adds the copied version as a new version whose ID is 444444. The new version becomes the current version of the object. Therefore, the object has two versions that have the same content: 222222 and 444444, in which version 222222 is a previous version and version 444444 is the current version.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7595688951/p40309.jpg)

-   Permanently delete the current version of the object

    In the following figure, the current version whose ID is 222222 is permanently deleted by a DeleteObject request in which the version ID is specified. The most recent previous version whose ID is 111111 becomes the current version of the object.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7595688951/p40310.jpg)


**Note:** We recommend that you use CopyObject to recover a previous version as the current version because the current version cannot be recovered after it is permanently deleted.

You can use the [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Copy objects.md) command provided by ossutil and the following OSS SDKs to recover previous versions in a versioned bucket: [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Versioning/Copy objects.md), [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Versioning/Copy objects.md), [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Copy objects.md), [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Versioning/Copy objects.md),[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Versioning/Copy objects.md), [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Versioning/Copy objects.md), and [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Versioning/Copy objects.md).

