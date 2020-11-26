# Best practices for versioning

Versioning is used to manage different versions of an object in a bucket. If versioning is enabled for a bucket, you can save, retrieve, and restore objects of any versions. This topic describes several common scenarios in versioning.

**Note:** For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## Delete previous versions

When versioning is enabled for a bucket, deleted or overwritten objects are saved as previous versions. A large number of redundant previous version of objects use storage capacity and incur fees. You can configure lifecycle rules to delete previous versions of an object.

For more information about how to configure lifecycle rules in the console, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md). You can configure a lifecycle rule to delete the objects 30 days after the objects become previous versions. The following figure shows the configuration.

![Lifecycle](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3554449951/p112361.png)

## Recover previous versions

You can use versioning to recover a previous version of an object and recover objects that are deleted or overwritten due to unintended operations or application failures. The following two methods are used:

-   We recommend that you copy objects in a versioned bucket to recover a previous version of an object.

    You can copy a previous version of an object to the bucket that stores the object. The previous version overwrites the current version and the overwritten version becomes the previous version. For more information the implementation modes, see the following topics:

    -   [Console](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Versioning.md)
    -   [ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)
    -   [Java SDK](/intl.en-US/SDK Reference/Java/Versioning/Copy objects.md)
    -   [Python SDK](/intl.en-US/SDK Reference/Python/Versioning/Copy objects.md)
    -   [C++ SDK](/intl.en-US/SDK Reference/C++/Versioning/Copy objects.md)
    -   [Go SDK](/intl.en-US/SDK Reference/Go/Versioning/Copy objects.md)
-   Permanently delete the current version of an object

    You can delete the current version. The latest previous version becomes the current version. For more information the implementation modes, see the following topics:

    -   [Console](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Versioning.md)
    -   [ossutil](/intl.en-US/Tools/ossutil/Common commands/rm.md)
    -   [Java SDK](/intl.en-US/SDK Reference/Java/Versioning/Delete objects.md)
    -   [Python SDK](/intl.en-US/SDK Reference/Python/Versioning/Delete objects.md)
    -   [C++ SDK](/intl.en-US/SDK Reference/C++/Versioning/Delete objects.md)
    -   [Go SDK](/intl.en-US/SDK Reference/Go/Versioning/Delete objects.md)
    **Warning:** Previous versions cannot be recovered after they are deleted. Exercise caution when you perform this operation.


## Manage delete markers

When you delete an object in a versioned bucket, a delete marker is added. A large number of retained delete markers that are not deleted affect the performance of the GetBucket operation. We recommend that you configure lifecycle rules to remove delete markers on a regular basis. You can configure lifecycle rules in the following scenarios:

-   Automatically delete a delete marker of an object by using lifecycle rules

    After versioning is enabled, if you configure a lifecycle rule for the current version of an object, deleted objects are saved as previous versions and delete markers in the bucket. If you no longer use these objects, we recommend that you configure a lifecycle rule to delete previous versions of these objects. The following figure shows a configuration example.

    ![Expiration](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3554449951/p112545.png)

    OSS deletes each object by adding a delete marker 60 days after the current version of an object is created. The current version becomes the previous version and the delete marker becomes the current version. This lifecycle rule deletes each object 30 days after the object becomes the previous version. The current version \(a delete marker\) of an object is deleted 60 days later.

    **Note:** After all previous versions of an object are deleted, the lifecycle rule automatically deletes the object whose current version is a delete marker.

-   Manually delete a delete marker of an object

    When versioning is enabled for a bucket, all manually deleted objects are not deleted directly. These objects become previous versions and are stored in the bucket with delete markers. If you no longer use these objects, we recommend that you configure a lifecycle rule to delete the previous versions of the objects and delete markers. The following figure shows a configuration example.

    ![Delete marker](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3554449951/p112566.png)

    When a previous version of an object expires and is deleted, the delete marker is deleted.


**Note:** If you manually delete a delete marker, you need to delete both the previous versions of an object and the delete marker. Otherwise, the previous version of an object becomes the current version after the delete marker is deleted.

