# Create buckets

A bucket is a container for objects stored in OSS. Before you upload an object to OSS, you must create a bucket.

## Usage notes

-   When you create a bucket, you are charged only for the storage of objects in the bucket and the traffic generated when objects are accessed. For more information, see [Billable items and pricing](/intl.en-US/Pricing/Billing items and methods/Overview.md).
-   After a bucket is created, you cannot change its name or region.
-   The capacity of the bucket is scalable. You do not need to purchase capacity in advance.

For more information about buckets, see [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.

    You can also click **Overview**. Click **Create Bucket** in the upper-right corner.

3.  In the Create Bucket panel, configure parameters as described in the following table.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |**Bucket Name**|Yes|Set the name of the bucket. Naming conventions:     -   The bucket name must be globally unique in Alibaba Cloud OSS.
    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length. |
    |**Region**|Yes|Select the region for the bucket.To access OSS from an ECS instance over the internal network, select the region in which the ECS instance is located. For more information, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).

**Note:** If the bucket is located in mainland China, you must complete real-name registration by submitting your relevant information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page. |
    |**Storage Class**|Yes|Select the storage class for the bucket.     -   **Standard**: provides highly reliable, highly available, and high-performance object storage services that can handle frequent data access. Standard is suitable to store images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics.
    -   **IA**: provides highly durable object storage services at low costs. Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in real time. You are charged for the data retrieval. IA applies to scenarios where stored data is infrequently accessed \(once or twice each month\).
    -   **Archive**: provides highly durable object storage services at costs lower than Standard and IA. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. Objects of the Archive storage class must be restored before they can be accessed. The restoration takes about one minute, and you are charged for the data retrieval. Archive is suitable for data that you want to store for a long period of time such as archival data, medical images, scientific materials, and video footage.
    -   **Cold Archive**: provides highly durable object storage services at the lowest cost of the four storage classes. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. Objects of the Cold Archive storage class must be restored before they can be accessed. The time required to restore an Cold Archive object depends on the object size and restore mode. You are charged for data retrieval when you restore a Cold Archive object. Cold Archive is suitable to store extremely cold data for an ultra-long period of time. Such data includes data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, retained media resources in the film and television industries, and archived videos from the online education industry.

**Note:** Cold Archive is in public preview in the following regions: China \(Beijing\), China \(Zhangjiakou\), China \(Hangzhou\), China \(Shanghai\), China \(Shenzhen\), China \(Chengdu\), Australia \(Sydney\), Singapore, US \(Silicon Valley\), Germany \(Frankfurt\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), India \(Mumbai\), and China \(Hong Kong\). Contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
    |**Zone-redundant Storage**|No|For buckets in , you can select whether to enable zone-redundant storage \(ZRS\).     -   Enable: After ZRS is enabled, OSS backs up your data to three zones within the same region. By default, if the storage class of the bucket is Standard, the objects in the bucket are Standard \(ZRS\) objects. For more information, see [Zone-redundant storage](/intl.en-US/Developer Guide/Data security/Disaster recovery/Zone-redundant storage.md).

**Note:** This feature incurs extra costs and cannot be disabled after it is enabled. Exercise caution when you enable this feature.

    -   Disable: After ZRS is disabled, the redundancy type of the objects in the bucket is locally redundant storage \(LRS\). By default, if the storage class of the bucket is Standard, the objects in the bucket are Standard \(LRS\) objects. |
    |**Versioning**|No|Select whether to enable versioning.     -   **Enable**: When versioning is enabled for a bucket, an object that is overwritten or deleted is saved as a previous version of the object. Versioning allows you to recover objects in a bucket to any previous version, and protects your data from being accidentally overwritten or deleted. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
    -   **Disable**: Versioning is disabled.
**Note:** Versioning is supported in all regions except for Japan \(Tokyo\). |
    |**Access Control List \(ACL\)**|Yes|Select the bucket ACL.     -   **Private**: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
    -   **Public Read**: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket.

**Warning:** All Internet users can access objects in the bucket. This may result in unexpected access to the data in your bucket and an increase in your fees. Exercise caution when you set your bucket ACL to Public Read.

    -   **Public Read/Write**: All users, including anonymous users, can perform read and write operations on the objects in the bucket.

**Warning:** All Internet users can access objects in the bucket and write data to the bucket. This may result in unexpected access to the data in your bucket and an increase in your fees. If a user uploads prohibited data or information, it may affect your legitimate interests and rights. Therefore, we recommend that you do not set your bucket ACL to Public Read/Write except in special cases. |
    |**Encryption Method**|No|Select whether to enable server-side encryption for the bucket.     -   **Encryption Method**: Select an encryption method for the object.
        -   **None**: disables server-side encryption.
        -   **OSS-Managed**: uses keys managed by OSS for encryption. OSS uses data keys to encrypt objects and manages the data keys. In addition, OSS uses master keys that are regularly rotated to encrypt data keys.
        -   **KMS**: uses the default CMK stored in KMS or a specified CMK ID to encrypt and decrypt data. For more information about KMS-based encryption, see [Implement server-side encryption with CMKs stored in KMS \(SSE-KMS\)](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

**Note:**

            -   Before you use the KMS-based encryption, you must[activate KMS](https://www.alibabacloud.com/zh/products/kms).
            -   You are charged for calling API operations when you use CMKs to encrypt or decrypt data. For more information about the fees, see [KMS pricing](/intl.en-US/Pricing/Billing.md).
    -   **Encryption algorithm**:Only AES-256 is supported.
    -   **CMK**: You can configure this parameter if you select **KMS** in the **Encryption Method** section. You can configure the following parameters for a CMK:
        -   **alias/acs/oss**: The default CMK stored in KMS is used to encrypt different objects and decrypt the objects when they are downloaded.
        -   CMK ID: The keys generated by a specified CMK are used to encrypt different objects and the specified CMK ID is recorded in the metadata of the encrypted object. Objects are decrypted when they are downloaded by users who are granted decryption permissions. Before you specify a CMK ID, you must create a normal key or an [external key](/intl.en-US/Key service/Key type/Use symmetric keys/Import key material.md) in the same region as the bucket in the [KMS console](https://kms.console.aliyun.com). |
    |**Real-time Log Query**|No|Select whether to enable real-time log query for OSS.     -   **Enable**: enables real-time log query for OSS. OSS uses Log Service to provide real-time OSS log queries for the last seven days free of charge. After this feature is enabled, you can query and analyze records of access to objects in OSS buckets by using the OSS console in real time. For more information, see [Real-time log query](/intl.en-US/Developer Guide/Manage logs/Real-time log query.md).
    -   **Disable**: disables real-time log query. |
    |**Scheduled Backup**|No|Specify whether to use Hybrid Backup Recovery \(HBR\) to back up your OSS data on a scheduled basis.    -   **Enable**: enables scheduled backup. After you enabled scheduled backup, OSS automatically creates a backup plan to back up data once a day and save the backup files for one week.

You can choose **Files** \> **Scheduled Backup** to view the backup plan that was created.

    -   **Disable**: disables scheduled backup.
**Note:** If HBR is not activated or HBR is not authorized to access OSS, scheduled backup plans cannot be created. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md). |

4.  Click **OK**.


