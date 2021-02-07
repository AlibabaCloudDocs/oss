# Configure bucket inventory

You can use the bucket inventory feature to export the information about specific objects in a bucket, such as the number, sizes, storage classes, and encryption status of the objects. Compared with the ListObject operation, we recommend that you use the bucket inventory feature to list the information about a large number of objects.

The RAM user used to configure bucket inventory has permissions to perform the following operations: PutBucketInventory, GetBucketInventory, ListBucketInventory, DeleteBucketInventory, CreateRole, and GetRole. For more information, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md) and [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

When you use the bucket inventory feature, take note of the following items:

-   You can configure up to 10 inventories in the console.
-   You are charged if you use the bucket inventory feature. However, only storage fees for inventory lists and API calling fees are charged during public preview.
-   After an inventory is configured for a bucket, OSS generates inventory lists based on the inventory until the inventory is deleted. To save storage space, we recommend that you delete inventory lists that you no longer use in a timely manner.

For more information, see [Function](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md)

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Bucket Inventory**. In the Bucket Inventory section, click **Configure**.

4.  Click **Create Inventory**. In the Create Inventory panel, configure the following parameters to create an inventory.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |**Status**|Yes|Configure the status of the inventory. You can select **Enabled** or **Disabled**.|
    |**Rule Name**|Yes|Configure the name of the inventory. The name can contain only lowercase letters, digits, and hyphens \(-\) and cannot start or end with a hyphen \(-\).|
    |**Destination Bucket**|Yes|Select the bucket in which generated inventory lists are stored.The destination bucket must be in the same region as the bucket for which the inventory is configured. You can specify the bucket for which the inventory is configured as the destination bucket. |
    |**Inventory List Path**|No|Configure the folder in which generated inventory lists are stored. If you do not specify this parameter, inventory lists are stored in the root folder of the destination bucket.|
    |**Frequency**|Yes|Configure the frequency at which inventory lists are generated. You can select **Weekly** or **Daily**.We recommend that you configure the frequency based on the number of objects in the source bucket:

    -   If the number of objects is smaller than 1 billion, you can select Daily.
    -   If the number of objects is between 1 to 10 billion, you can select Weekly.
    -   If the number of objects is greater than 10 billion, we recommend that you select Weekly, configure individual inventory tasks for different object prefixes, and ensure that the number of objects in each inventory task is smaller than 10 billion. |
    |**Encryption Method**|No|Configure whether to encrypt inventory lists.     -   **None**: Inventory lists are not encrypted.
    -   **AES-256**: Inventory lists are encrypted by using AES-256.
    -   **KMS**: Inventory lists are encrypted by using a customer master key \(CMK\) managed by KMS.

To use a CMK to encrypt inventory lists, you must create a CMK in KMS in the same region as the destination bucket. For more information about how to configure CMKs, see [Create a CMK](/intl.en-US/Quick Start/Manage and use keys/Create a CMK.md).

**Note:** You are charged for calling API operations when you use CMKs to encrypt or decrypt data. |
    |**Object Versions**|Yes|Select the object version to which the inventory is applied. If versioning is enabled for the bucket, you can select **Current Version** or **All Versions** to export the current version or all versions of objects when you configure inventories for the bucket. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

By default, all objects in the bucket are exported if versioning is not enabled for the bucket. |
    |**Object Prefix**|No|Only objects whose names contain the specified prefix are scanned to generate inventory lists. If you do not specify the prefix, all objects in the bucket are scanned to generate inventory lists. **Note:** If objects whose names contain the specified prefix do not exist in the bucket, inventory lists are not generated. |
    |**Optional Fields**|Yes|Select the object information that you want to export to inventory lists. You can select the following fields: **Object Size**, **Storage Class**, **Last Update Time**, **ETag**, **Multipart Upload**, and **Encryption Status**.|

5.  Read and select **I understand the terms and agree to authorize Alibaba Cloud OSS to access the resources in my buckets.** Click **OK**.

    For more information about the content in inventory lists, see [t1855095.md\#section\_uyi\_xr6\_u4y](/intl.en-US/Developer Guide/Buckets/Bucket inventory.md).


