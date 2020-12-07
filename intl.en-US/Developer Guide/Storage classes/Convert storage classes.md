# Convert storage classes

OSS provides the following storage classes: Standard, Infrequent Access \(IA\), Archive, and Cold Archive. You can convert the storage class of an object based on your requirements.

You can use one of the following methods to convert the storage class of an OSS object:

-   Method 1: Configure lifecycle rules for automatic conversion
-   Method 2: Use the console, OSS tools, or SDKs to manually convert the storage class of an object

## Automatic conversion

OSS lifecycle rules provide the object transition mechanism to support automatic conversion of object storage classes.

-   Rules for storage class conversion based on lifecycle rules
    -   Locally redundant storage \(LRS\)

        ![LRS](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0759807061/p179966.png)

        The storage class of LRS objects can be converted based on the following rules:

        -   Conversions from Standard LRS to IA LRS, Archive LRS, or Cold Archive LRS are supported.
        -   Conversions from IA LRS to Archive LRS or Cold Archive LRS are supported.
        -   Conversions from Archive LRS to Cold Archive LRS are supported.
    -   Zone-redundant storage \(ZRS\)

        ![ZRS](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0759807061/p179968.png)

        The storage class of LRS objects can be converted based on the rule: Only conversions from Standard ZRS to IA ZRS are supported.

-   Examples of storage class conversion based on lifecycle rules

    For example, you can configure a lifecycle rule for objects whose names contain a specified prefix in an LRS bucket:

    ![Lifecycle](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6195688951/p132749.png)

    The storage class of objects in the bucket is converted based on the following rules:

    -   Objects are converted to IA objects after they are stored for 30 days.
    -   Objects are converted to Archive objects after they are stored for 180 days.
    -   Objects are converted to Cold Archive objects after they are stored for 360 days.
    -   Objects are deleted after they are stored for more than 720 days.
    **Note:** If a bucket has Transit to IA Storage Class, Transit to Archive Storage Class, Transit to Cold Archive Storage Class,and Delete configured at the same time, the retention periods in days must comply with the following rule:

    Days for Transit to IA Storage Class < Days for Transit to Archive Storage Class< Days for Transit to Cold Archive Storage Class < Days for Delete

-   Implementation modes of storage class conversion based on lifecycle rules

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md)|A user-friendly and intuitive web application|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/lifecycle.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Manage lifecycle rules.md)|SDK demos for a variety of programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Manage lifecycle rules.md)|
    |[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Manage lifecycle rules.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Manage lifecycle rules.md)|
    |[C SDK](/intl.en-US/SDK Reference/C/Buckets/Manage lifecycle rules.md)|
    |[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Manage lifecycle rules.md)|
    |[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Manage lifecycle rules.md)|
    |[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Manage lifecycle rules.md)|


## Manual conversion

You can use the CopyObject operation to convert the storage class of an object by overwriting the object. If the storage class of the converted object is IA, Archive, or Cold Archive and the object is stored for less than the specified period of time, you are charged for the minimum storage duration, including the remaining duration. For more information, see [Storage fees](/intl.en-US/Pricing/Billing items and methods/Storage fees.md).

-   Rules for storage class conversion by using CopyObject
    -   LRS

        Conversions between any storage classes are supported.

        **Note:** Archive or a Cold Archive objects must be restored before the storage class of the objects can be converted. For more information about how to restore an object, see [Restore objects](/intl.en-US/Developer Guide/Objects/Manage files/Restore objects.md).

    -   ZRS

        Only conversions between Standard ZRS and IA ZRS are supported.

-   Implementation modes of storage class conversion by using CopyObject

    |Implementation mode|Description|
    |-------------------|-----------|
    |[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Modify the storage class of objects.md)|A user-friendly and intuitive web application|
    |[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md)|A high-performance command-line tool|
    |[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Convert the storage class of an object.md)|SDK demos for a variety of programming languages|
    |[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Convert the storage class of an object.md)|
    |[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Convert the storage class of an object.md)|
    |[C++ SDK](/intl.en-US/SDK Reference/C++/Manage objects/Convert the storage class of an object.md)|


## Usage notes

Take note of the following items after the storage class of an object is converted to IA, Archive, or Cold Archive:

-   Minimum billable size

    Objects smaller than 64 KB are charged as 64 KB.

-   Minimum storage period

    The minimum storage period that you can set for an IA Object is 30 days. The minimum storage period that you can set for an Archive object is 60 days. The minimum storage period that you can set for a Cold Archive object is 180 days. If an object is stored for less than the minimum storage period, you are still charged for the minimum storage duration.

    -   Automatic conversion

        If you configure lifecycle rules to automatically convert the storage class of an object, OSS does not recalculate the retention period when the storage class of the object changes. Example: An object named a.txt is a Standard object. After the object is stored in OSS for 10 days, its storage class is converted to IA based on lifecycle rules. The IA object must be stored for a minimum of 20 days. For more information, see [FAQ](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

    -   Manual conversion

        If you manually convert the storage class of an object, OSS recalculates the retention period of the object. Example: An object named a.txt is a Standard object. After the object is stored in OSS for 10 days, its storage class is manually converted to IA. The retention period of the IA object is reset to 0 and the object must be stored for a minimum of 30 days.

-   Restoration period

    It takes a while to restore Archive or Cold Archiveobjects to the readable state. If your business requires your objects to be read in real time, we recommend that you do not convert the storage classes of your objects to Archiveor Cold Archive.

-   Data retrieval fees

    Data retrieval fees are incurred when you access IA objects. You are charged for data restoration when you restore Archiveor Cold Archive objects. Fees incurred by data restoration are calculated independently of the generated outbound traffic. If an object is accessed for more than once a month on average, the cost on the object may be higher if you convert the storage class of the object to IA, Archive, or Cold Archive.

-   Temporary storage fees \(not charged in public preview\)

    When you restore a Cold Archive object, a Standard replica is generated for temporary access. OSS charges the temporary storage fees of the replica for the duration during which the replica is available based on Standard storage.


