# Create buckets

A bucket is a container for objects stored in OSS. You must create a bucket before you upload any types of objects to OSS. This topic describes how to create a bucket in the OSS console.

OSS is activated. For more information, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click **Create Bucket**.

    You can also click **Overview**. Then, click **Create Bucket** in the upper-right corner.

3.  In the Create Bucket panel, configure required parameters as described in the following table. You can keep the default settings for other parameters or configure the parameters after the bucket is created.

    |Parameter|Description|
    |---------|-----------|
    |**Bucket Name**|Set the name of the bucket. The name of a bucket cannot be changed after the bucket is created.The bucket name must comply with the following conventions:

    -   The name must be globally unique.
    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length. |
    |**Region**|Select the region where the bucket is located. The region of a bucket cannot be changed after the bucket is created.To access the bucket from an ECS instance over the internal network, select the region in which the ECS instance is located. For more information, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).

**Note:** If the bucket that you want to create is located in a region in mainland China, you must complete real-name registration by submitting your relevant information on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page. |
    |**Zone-redundant storage**|OSS provides zone-redundant storage \(ZRS\) to replicate user data across three zones within the same region. Even if one zone becomes unavailable, the data still remains accessible.     -   **Enable**: Enable ZRS for the bucket. Objects in the bucket are stored by using ZRS. For example, if the storage class of the bucket is Standard, the objects in the bucket are Standard ZRS objects by default. For more information, see [ZRS](/intl.en-US/Developer Guide/Data security/Disaster recovery/ZRS.md).

**Note:**

        -   ZRS is available only in the China \(Shenzhen\), China \(Beijing\), China \(Shanghai\), China \(Hong Kong\)and Singapore regions.
        -   You can enable ZRS for a bucket only when you create the bucket. ZRS cannot be disabled after you enable it for a bucket. Exercise caution when you enable this feature.
    -   **Disable**: Disable ZRS for the bucket. By default, ZRS is disabled for a bucket to be created. Objects in the bucket are stored by using locally redundant storage \(LRS\). For example, if the storage class of the bucket is Standard, the objects in the bucket are Standard ZRS objects by default. |

4.  Click **OK**.


## Other implementation methods

|Operation|Implementation method|
|---------|---------------------|
|Create buckets|[ossutil](/intl.en-US/Tools/ossutil/Common commands/mb (create buckets).md)|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md)|
|[API operations](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md)|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Create buckets.md)|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Create a bucket.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Create buckets.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Buckets/Create buckets.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Create buckets.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Create buckets.md)|
|[.NET](/intl.en-US/SDK Reference/. NET/Buckets/Create buckets.md)|
|[Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Create buckets.md)|
|[Android](/intl.en-US/SDK Reference/Android/Buckets/Create buckets.md)|
|[iOS](/intl.en-US/SDK Reference/iOS/Manage buckets.md)|

[Upload objects](/intl.en-US/Quick Start/OSS console/Upload objects.md)

