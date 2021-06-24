# Tutorial: Implement data sharing across departments based on bucket policies

This topic describes how to implement data sharing across different departments or projects of an enterprise to allow that the data shared by a department can be only downloaded but cannot be written or deleted by users of other departments. This way, you can reduce the risks of accidental deletion and modification of the shared data.

In this example, department A shares the data stored in a bucket named example-bucket with users of department B and allows these users to download the shared data. This example shows how to follow the principle of least privilege to perform access control on shared data. The following figure shows the relationship between the shared bucket and the administrators and users of department A and B.

![policy1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9585821061/p148072.png)

## Procedure

In this example, the administrator of department A can configure bucket policies to allow users of department B to download but not write or delete the shared data. To achieve the goal, the following steps must be performed:

-   [Step 1: Create a bucket](#section_5fk_ibf_m39)

    The administrator of department A creates a bucket named example-bucket to store shared data.

-   [Step 2: Grant permissions to upload shared data](#section_0ii_5qq_1ki)

    The administrator of department A configures a bucket policy for example-bucket to allow users of department A to upload shared data to the bucket.

-   [Step 3: Grant permissions to download but not write or delete shared data](#section_vn6_z53_h52)

    The administrator of department A configures a bucket policy for example-bucket to allow users of department B to download but not write or delete shared data.

-   [Step 4: Upload shared data](#section_rdb_qkb_jea)

    Users of department A upload shared data to example-bucket.

-   [Step 5: Verify permissions](#section_4qi_gsg_euw)

    Verify the permissions of the users of department B to ensure they can only download but cannot write or delete shared data.


## Prerequisites

-   RAM users for the administrators and users of department A and B are created by the Alibaba Cloud account of the enterprise.

    For more information about how to create RAM users, see [Create a RAM user](/intl.en-US/RAM User Management/Basic operations/Create a RAM user.md).

-   The UIDs of the RAM users are obtained.

    For more information about how to view the basic information about a RAM user such as the UID, see [View the basic information of a RAM user](/intl.en-US/RAM User Management/Basic operations/View the basic information of a RAM user.md).

-   Appropriate permissions are granted to the RAM users.

    In this example, the administrator of department A needs to create buckets and configure bucket policies. Therefore, the user group of the RAM users for administrators must have the AliyunOSSFullAccess permission. For more information about how to grant permissions to RAM users, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Authorization management/Grant permissions to a RAM user.md).


## Step 1: Create a bucket

Perform the following steps to create a bucket as the administrator of department A:

1.  Use the RAM user of the administrator of department A to log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the page that appears, click **Create Bucket**.

3.  In the Create Bucket dialog box that appears, configure parameters for the bucket.

    In this example, set the bucket name to example-bucket. For more information about how to configure parameters to create a bucket, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

4.  Click **OK**.


## Step 2: Grant permissions to upload shared data

Perform the following steps to grant users of department A permissions to upload shared data to example-bucket as the administrator of department A:

1.  Click example-bucket created in [Step 1](#section_5fk_ibf_m39).

2.  You can also choose **Access Control** \> **Bucket Policy**. In the Bucket Policy section, click **Configure**.

3.  In the Authorize dialog box that appears, click **Authorize**.

4.  In the Authorize dialog box that appears, configure parameters for the bucket policy.

    |Parameter|Description|
    |---------|-----------|
    |**Applied To**|Select **Whole Bucket**, which indicates that the policy applies to the whole bucket.|
    |**Accounts**|Select **RAM Users**.You can select the RAM users to which you can grant permissions to upload shared data from the drop-down list. You can also enter a keyword in the search box to search for specific RAM users. Fuzzy matching is supported. |
    |**Authorized Operation**|Select **Read/Write**.This option indicates that authorized users can perform read and write operations on the specified resources. |

5.  Click **OK**.

    A bucket policy is created to allow users of department A to upload shared data.


## Step 3: Grant permissions to download but not write or delete shared data

Perform the following steps to grant users of department B permissions to download shared data from example-bucket as the administrator of department A:

1.  Click example-bucket created in [Step 1](#section_5fk_ibf_m39).

2.  You can also choose **Access Control** \> **Bucket Policy**. In the Bucket Policy section, click **Configure**.

3.  In the Authorize dialog box that appears, click **Authorize**.

4.  In the Authorize dialog box that appears, configure parameters for the bucket policy.

    |Parameter|Description|
    |---------|-----------|
    |**Applied To**|Select **Whole Bucket**, which indicates that the authorization policy applies to the whole bucket.|
    |**Accounts**|Select **Other Accounts**. Enter the UIDs of the RAM users to which you want to grant permissions to download shared data.|
    |**Authorized Operation**|Select **Read Only**.This option indicates that authorized users can only view, list, and download but cannot write or delete the data stored in example-bucket. |

5.  Click **OK**.

    A bucket policy is created to allow users of department B to download but not write or delete shared data.


## Step 4: Upload shared data

Perform the following steps to upload data to example-bucket as a user of department A:

1.  Use the RAM user of a user of department A to log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the page that appears, click example-bucket.

3.  Choose **Files** \> **Upload**.

4.  In the Upload dialog box that appears, configure parameters to upload shared data.

    Select Current for Upload To. For more information about how to configure the ACL and upload method of the object to upload, see [Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md).

5.  After the upload is complete, close the Upload Tasks dialog box.

    The shared data is uploaded to example-bucket.


## Step 5: Verify permissions

Perform the following steps in the OSS console to verify that users of department B can only download but cannot write or delete shared data:

1.  Use the RAM user of a user of department B to log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the page that appears, click example-bucket.

3.  On the page that appears, click the **Files**.

4.  Verify permissions.

    1.  Verify the download permissions of users of department B on shared data.

        In the Actions column corresponding to any object, choose **More** \> **Download**.

        -   If the download fails, the download permissions are not correctly configured. Check whether the bucket policy is correctly configured.
        -   If the download is successful, the download permissions are correctly configured.
    2.  Verify the upload permissions of users of department B on shared data.

        Follow [Step 4](#section_rdb_qkb_jea) to upload data to example-bucket.

        -   If the upload fails, the upload permissions are correctly configured.
        -   If the upload is successful, the upload permissions are not correctly configured. Check whether the bucket policy is correctly configured.
    3.  Verify the deletion permissions of users of department B on shared data.

        In the Actions column corresponding to any object in example-bucket, choose **More** \> **Completely Delete**.

        -   If the object cannot be deleted, the deletion permissions are correctly configured.
        -   If the object is deleted, the deletion permissions are not correctly configured. Check whether the bucket policy is correctly configured.

