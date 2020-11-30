# Modify the storage class of objects

OSS provides the following storage class: Standard, Infrequent Access \(IA\), Archive, and Cold Archive. Each storage class differs in the access frequency and billing. You can select a suitable storage class. This topic describes how to modify the storage class of an object in the OSS console.

-   When you modify the storage class of an object in the OSS console, the object size cannot exceed 1 GB. We recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md) to modify the storage classes of objects that are larger than 1 GB.
-   You can modify the storage class of an Archive or Cold Archiveobject only after the object is restored. For more information, see [Restore objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Restore objects.md).
-   When you modify the storage class of an object, the original object is replaced with a new object that has the selected storage class. Therefore, if the modified objects are of the IA, Archive, or Cold Archivestorage class and stored for less than the minimum storage duration, you are charged for the minimum storage duration, including the remaining duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, click **Files**.

4.  Move the pointer over **More** in the Actions column corresponding to the object for which you want to modify the storage class and select **Modify Storage Class** from the drop-down list.

5.  Select the storage class to which you want to convert, and click **OK**.

    We recommend that you keep **Retain User Metadata** enabled to retain the user metadata of the object after you modify the storage class.


