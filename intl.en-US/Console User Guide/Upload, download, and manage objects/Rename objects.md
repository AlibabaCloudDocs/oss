# Rename objects

This topic describes how to rename an existing object in the OSS console.

## Usage notes

When you rename an object in the OSS console, the [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md) operation is called to overwrite the original object with an object with the new name. Take note of the following items when you rename an object:

-   You can rename only objects that are up to 1 GB in size in the OSS console. To rename objects that are larger than 1 GB, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Copy objects.md).
-   In a bucket for which [versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) is enabled, you can rename an object only when you set **Display Previous Versions** to **Hide**. After you rename an object, a delete marker is created for the original object.
-   If the object that you rename is an IA, Archive, or Cold Archive object and is stored for less than the specified minimum storage duration, you are charged for the entire minimum storage duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).
-   Folders cannot be renamed.

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  Click **Files**.

4.  Move the pointer over the object you want to rename and click the ![Rename](../images/p182159.png) icon to the right of the object name.

5.  In the Rename dialog box, enter the new object name and click **OK**.

    The object name must comply with the following conventions:

    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 254 characters in length.
    -   The name cannot include forward slashes \(/\) or backslashes \(\\\).

