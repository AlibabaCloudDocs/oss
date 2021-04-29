# Use ossbrowser

This topic describes how to use ossbrowser. ossbrowser is a graphical management tool developed by Alibaba Cloud. This tool provides features similar to Windows Explorer. You can use ossbrowser to easily manage your objects and buckets.

ossbrowser supports the same operations related to buckets and objects as the Object Storage Service \(OSS\) console. You can follow the instructions of ossbrowser to manage your buckets and objects.

|Operation|Description|
|---------|-----------|
|Bucket-related operations|-   Create buckets

A bucket is a container for objects stored in OSS. Before you upload objects to OSS, you must create a bucket. For more information about how to specify the name, region, access control list \(ACL\), and storage class of a bucket when you create the bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

-   Delete a bucket

If you no longer use a bucket, you can delete it to avoid incurring further fees. For more information, see [Delete buckets](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Delete buckets.md). |
|Object-related operations|ossbrowser supports operations such as upload, download, preview, move, and copy objects as well as generate object URLs. When you use ossbrowser to perform object-related operations, take note of the following items:-   By default, ossbrowser uses multipart upload and resumable upload to upload objects. The object to upload cannot exceed 48.8 TB in size. If the upload of an object is unexpectedly interrupted before the object is uploaded, the uploaded content is stored as parts in an OSS bucket. To avoid additional storage fees, we recommend that you use the following methods to delete these parts if you no longer use these parts.
    -   Manually delete parts. For more information, see [Delete parts](#li_3rq_l1e_6gg).
    -   Configure lifecycle rules to delete parts. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   The object that you can move or copy by using ossbrowser cannot exceed 5 GB in size. To move or copy objects that are larger than 5 GB, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Overview.md).

For more information about other object-related operations, see OSS Console User Guide. |

