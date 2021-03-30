# Upload objects

You can upload objects up to 5 GB in size in the OSS console.

A bucket is created. For more information, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).

To upload objects larger than 5 GB in size, we recommend that you use [Multipart upload and resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) of OSS SDKs or OSS APIs, graphical management tool [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md), or command line tool [ossutil](/intl.en-US/Tools/ossutil/Overview.md).

In Alibaba Finance Cloud, you cannot upload objects by using the OSS console because OSS does not have a region connected to the Internet. To upload objects, you must use OSS SDKs, ossutil, or ossbrowser.

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload an object.

3.  In the left-side navigation pane, click **Files**. Then, click **Upload**.

4.  In the Upload panel, configure the parameters described in the following table.

    1.  The following table describes the basic settings.

        |Parameter|Description|
        |---------|-----------|
        |**Upload To**|Set the path in which to store an object after the object is uploaded to the bucket.         -   **Current**: Objects are uploaded to the current folder.
        -   **Specified**: Objects are uploaded to the specified folder. You must enter the folder name. If the folder whose name you entered does not exist, OSS automatically creates the folder and uploads the object to the folder.

The folder must meet the following naming conventions:

            -   The name can contain only UTF-8 characters. The name must be 1 to 254 characters in length.
            -   The name cannot start with a forward slash \(/\) or backslash \(\\\).
            -   The name cannot contain consecutive forward slashes \(/\)
            -   The name cannot contain two consecutive periods \(`..`\). |
        |**File ACL**|Set the access control list \(ACL\) for the object.         -   **Inherited from Bucket**: The ACL of the object is the same as that of the bucket.
        -   **Private**: Only the owner or authorized users of this bucket can read and write the object. We recommend that you set the ACL to private.
        -   **Public Read**: Only the owner or authorized users of this bucket can write the object. Other users, including anonymous users can only read the object. If you set the ACL to public read, the object may be unexpectedly accessed, which results in out-of-control costs.
        -   **Public Read/Write:**: All users, including anonymous users can read and write the object. If you set the ACL to public read/write, the object may be unexpectedly accessed, which results in out-of-control costs. If a user uploads prohibited data or information, your legitimate interests and rights may be infringed. Therefore, we recommend that you do not set your object ACL to public read/write except in special cases.
For more information about object ACLs, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
        |**Upload Acceleration**|After transfer acceleration is enabled for the bucket that contains the object, you can turn on **Upload Acceleration** if you want to accelerate the upload of the object. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md). |
        |**Files to Upload**|Select the object or folder you want to upload. You can click **Select Files** to select an object or **Select Folders** to select a folder. You can also drag the required object or folder to the Files to Upload section.

If you select an unnecessary object, click **Remove** in the Actions column that corresponds to the object to remove the object.

**Note:**

        -   When you upload an object that has the same name as an existing object in OSS to an unversioned bucket, the existing object is overwritten.
        -   When you upload an object that has the same name as an existing object in OSS to a versioned bucket, the existing object becomes a previous version, and the uploaded object becomes the latest version. |

    2.  Configure advanced settings such as Storage Class and Encryption Method.

        |Parameter|Description|
        |---------|-----------|
        |**Storage Class**|Set the storage class of the object.         -   **Inherited from Bucket**: The storage class of the object is the same as that of the bucket.
        -   **Standard**: Standard is suitable for objects that are frequently accessed.
        -   **IA**: Infrequent Access \(IA\) is suitable for objects that are less frequently accessed. Objects that are accessed less than once to twice a month fall into this category. IA objects have a minimum storage duration of 30 days. You are charged for data retrieval when you access these objects.
        -   **Archive**: Archive is suitable for objects that are infrequently accessed. Archive objects have a minimum storage duration of 60 days. Before you can access an object of the Archive storage class, you must restore the object.
        -   **Cold Archive**: Cold Archive is suitable for long-term storage of backup objects and raw data. Cold Archive objects have a minimum storage duration of 180 days. Before you can access an object of the Cold Archive storage class, you must restore the object. The amount of time required to restore a Cold Archive object depends on the data size and the restore mode. You are charged for the data retrieval when you restore a Cold Archive object.
For more information, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md). |
        |**Encryption Method**|Configure server-end encryption for an object.         -   **Inherited from Bucket**: The encryption method of the object is the same as that of the bucket.
        -   **OSS-Managed**: The keys managed by OSS are used for encryption. OSS uses data keys to encrypt objects and manages the data keys. In addition, OSS uses master keys that are regularly rotated to encrypt data keys.
        -   **KMS**: The default CMK stored in KMS or the specified CMK ID is used to encrypt and decrypt data. Descriptions of **CMK**:
            -   **alias/acs/oss**: The default CMK stored in KMS is used to encrypt different objects and decrypt the objects when the objects are downloaded.
            -   CMK ID: The keys generated by a specified CMK are used to encrypt different objects, and the specified CMK ID is recorded in the metadata of the encrypted object. Objects are decrypted when they are downloaded by users who are granted decryption permissions. Before you specify a CMK ID, you must create a normal key or an [external key](/intl.en-US/Key Service/Key type/Use symmetric keys/Import key material.md) in the same region as the bucket in the [KMS console](https://kms.console.aliyun.com).
        -   **Encryption algorithm**:Only AES-256 is supported.
For more information about object ACLs, see [Object ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md). |
        |**User Metadata**|Add the descriptive information for the object. You can add multiple pieces of user metadata as custom headers. However, the total size of the user metadata cannot exceed 8 KB. When you add user metadata, you must prefix parameters with `x-oss-meta-` and specify a value such as **x-oss-meta-location:hangzhou** for the parameters.|

    3.  Click **Upload**.

        You can view the upload progress of objects on the Upload Tasks tab.


