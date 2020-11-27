# Create buckets

After you activate Alibaba Cloud OSS, you must create a bucket in the OSS console to store objects.

## Use the OSS console

To create a bucket in the OSS console, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.

    You can also click **Overview**. Click **Create Bucket** in the upper-right corner.

3.  In the Create Bucket pane, set the parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Bucket Name**|Set the name of the bucket. The name cannot be changed after the bucket is created. Naming conventions:     -   The bucket name must be globally unique in Alibaba Cloud OSS.
    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length. |
    |**Region**|Select the region for the bucket. The region cannot be changed after the bucket is created. To access OSS from an ECS instance over the internal network, select the region in which the ECS instance is located. For more information, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).**Note:** If the bucket is located in mainland China, you must complete real-name registration by submitting your relevant information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page. |
    |**Storage Class**|Select the storage class for the bucket.     -   **Standard**: provides highly reliable, highly available, and high-performance object storage services that can handle frequent data access. Standard is suitable to store images for social networking and sharing applications and data for audio and video applications, large websites, and big data analytics.
    -   **IA**: provides highly durable object storage services at low costs. Objects of the IA storage class have a minimum storage period of 30 days and a minimum billable size of 64 KB. You can access objects of the IA storage class in real time. You are charged for the data retrieval. IA applies to scenarios where stored data is infrequently accessed \(once or twice each month\).
    -   **Archive**: provides highly durable object storage services at costs lower than Standard and IA. Objects of the Archive storage class have a minimum storage period of 60 days and a minimum billable size of 64 KB. You must restore an object of the Archive storage class before you can access it. The restoration takes about one minute, and you are charged for the data retrieval. Archive is suitable for data that you want to store for a long period of time such as archival data, medical images, scientific materials, and video footage.
    -   **Cold Archive**: provides highly durable object storage services at the lowest cost of the four storage classes. Objects of the Cold Archive storage class have a minimum storage period of 180 days and a minimum billable size of 64 KB. You must restore an object of the Cold Archive storage class before you can access it. The time required to restore an Cold Archive object depends on the object size and restore mode. You are charged for the data retrieval when you restore a Cold Archive object. Cold Archive is suitable to store extremely cold data that you want to store for an ultra-long period. Such data includes data that must be retained for an extended period of time due to compliance requirements, raw data that is accumulated over an extended period of time in the big data and AI fields, media resources that are retained in the film and television industries, and archived videos from the online education industry.

**Note:** The Cold Archive storage class is in public preview in the Australia \(Sydney\), Singapore, US \(Silicon Valley\), Germany \(Frankfurt\), Malaysia \(Kuala Lumpur\), Indonesia \(Jakarta\), India \(Mumbai\), and China \(Hong Kong\) regions. You can contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
    |**Zone-redundant Storage**|For buckets in Singapore, China \(Shenzhen\), China \(Beijing\), China \(Hangzhou\), and China \(Shanghai\) regions, you can select whether to enable zone-redundant storage \(ZRS\).     -   Enable: After ZRS is enabled, OSS backs up your data to three zones within the same region. If the storage class of the bucket is Standard, the objects in the bucket are Standard \(ZRS\) objects by default. For more information, see [Zone-redundant storage](/intl.en-US/Developer Guide/Data security/Disaster recovery/Zone-redundant storage.md).

**Note:** This feature incurs extra costs and cannot be disabled after it is enabled. Exercise caution when you enable this feature.

    -   Disable: After ZRS is disabled, the redundancy type of the objects in the bucket is locally redundant storage \(LRS\). If the storage class of the bucket is Standard, the objects in the bucket are Standard \(LRS\) objects by default. |
    |**Versioning**|Select whether to enable versioning.     -   **Enable**: When versioning is enabled for a bucket, an object that is overwritten or deleted is saved as a previous version of the object. Versioning allows you to restore objects in a bucket to any previous point in time, and protects your data from being accidentally overwritten or deleted. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
    -   **Disable**: disables versioning.
**Note:** Versioning is supported in all regions except for Japan \(Tokyo\). |
    |**Access Control List \(ACL\)**|Select the bucket ACL.     -   **Private**: Only the bucket owner can perform read and write operations on objects in the bucket. Other users cannot access the objects in the bucket.
    -   **Public Read**: Only the bucket owner can perform write operations on objects in the bucket. Other users, including anonymous users, can perform only read operations on the objects in the bucket.

**Warning:** All Internet users can access objects in the bucket. This may cause unexpected access to the data in your bucket, and cause an increase in your fees. Exercise caution when you set your bucket ACL to Public Read.

    -   **Public Read/Write**: All users, including anonymous users, can perform read and write operations on objects in the bucket.

**Warning:** All Internet users can access objects in the bucket and write data to the bucket. This may cause unwanted access to the data in your bucket, and cause an increase in your fees. If a user uploads prohibited data or information, it may affect your legitimate interests and rights. Therefore, we recommend that you do not set your bucket ACL to Public Read/Write except in special cases. |
    |**Encryption Method**|Select whether to enable server-side encryption.     -   **Encryption Method**: Select an encryption method for the object.
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
    |**Real-time Log Query**|Select whether to enable real-time log query for OSS.     -   **Enable**: enables real-time log query for OSS. OSS uses Log Service to provide real-time OSS log queries for the last seven days free of charge. After this feature is enabled, you can query and analyze records of access to objects in OSS buckets by using the OSS console in real time. For more information, see [Real-time log query](/intl.en-US/Developer Guide/Manage logs/Real-time log query.md).
    -   **Disable**: disables real-time log query. |
    |**Scheduled Backup**|Select whether to create a scheduled backup plan to back up your OSS data by using Hybrid Backup Recovery \(HBR\).     -   **Enable**: After scheduled backup is enabled, OSS creates a backup plan to back up data once a day and retain the backup files for one week. You can choose **Files** \> **Scheduled Backup** to view the backup plan that is created.
    -   **Disable**: creates no scheduled backup plans.
**Note:** If you do not enable HBR or authorize HBR to access OSS, you fail to create a scheduled backup plan. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md). |

4.  Click **OK**.


## Use ossutil

ossutil is a command line tool for OSS. You can use ossutil to create a bucket. For more information, see [mb](/intl.en-US/Tools/ossutil/Common commands/mb.md).

## Use APIs and SDKs

OSS provides APIs and SDKs that use a variety of programming languages to facilitate secondary development. For more information, see the following topics:

-   API operation: [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md)
-   Java SDK: [Create buckets](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)
-   Python SDK: the "Create a bucket" section in [Manage buckets](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)
-   PHP SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)
-   Go SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)
-   C SDK: the "Create a bucket" section in [Bucket](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)

For more information about SDK examples in more programming languages, see [t22258.dita\#concept\_dcn\_tp1\_kfb](/intl.en-US/SDK Reference/Introduction.md).

[Upload objects](/intl.en-US/Quick Start/Upload objects.md)

