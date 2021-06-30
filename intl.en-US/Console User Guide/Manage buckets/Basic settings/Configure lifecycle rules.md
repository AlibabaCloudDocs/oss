# Configure lifecycle rules

You can configure lifecycle rules to convert the storage classes of objects in a bucket or delete the specified objects and parts from a bucket in batch.

-   After a lifecycle rule is configured, it is loaded within 24 hours and takes effect within 24 hours after it is loaded. Check the configurations of a rule before you save the rule.
-   Objects that are deleted based on lifecycle rules cannot be recovered. Configure lifecycle rules based on your requirements.
-   You can configure up to 100 lifecycle rules for a bucket in the Object Storage Service \(OSS\) console. To configure more rules, use ossutil or OSS SDKs. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Lifecycle**. In the **Lifecycle** section, click **Configure**.

4.  On the page that appears, click **Create Rule**. In the Create Rule panel, configure the parameters described in the following table.

    -   Parameters for unversioned buckets

        |Category|Parameter|Description|
        |--------|---------|-----------|
        |**Basic Settings**|**Status**|Specify the status of the lifecycle rule. Valid values: **Enabled** and **Disabled**.|
        |**Applied To**|Specify the objects to which the lifecycle rule applies. Valid values: **Files with Specified Prefix** and **Whole Bucket**. **Note:** If you select **Files with Specified Prefix**, you can configure multiple lifecycle rules for objects whose names contain different prefixes. If you select **Whole Bucket**, you can configure only one lifecycle for the bucket. |
        |**Prefix**|If you set **Applied To** to **Files with Specified Prefix**, you must specify the prefix of the objects to which the rule applies. For example, if you want that the rule applies to objects whose names start with img, enter img.|
        |**Tagging**|Configure tags. The rule applies only to objects that have the specified tags. Example: If you select **Files with Specified Prefix** and set Prefix to img, Key to a, and Value to 1, The rule applies to all objects that has the img prefix in their names and has the tag a=1. For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).|
        |**Clear Policy**|**File Lifecycle**|Configure rules for objects to specify when the objects expire. You can set File Lifecycle to **Validity Period \(Days\)**, **Expiration Date**, or **Disabled**. If you select **Disabled**, the configurations of File Lifecycle do not take effect.|
        |**Transit to IA Storage Class**|Specify when objects that match the rule expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle**. The storage class of the objects is converted to IA after the objects expire.         -   **Validity Period \(Days\)**: specifies the number of days within which objects can be retained after they are last modified. The storage class of the objects is converted to IA one day after they expire. For example, if you set Validity Period \(Days\) to 30, the storage class of the objects that are last modified on January 1, 2021 expires on January 30, 2021 is converted to IA on February 1, 2021.
        -   **Expiration Date**: specifies an expiration date. All objects that are last modified before this date expire and the storage class of these objects is converted to IA. For example, if you set Expiration to January 1, 2021, the storage class of the objects that were last modified before January 1, 2021 is converted to IA. |
        |**Transit to Archive Storage Class**|Specify when objects that match the rule expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle**Specify when objects that match the rule expire. The storage class of the objects is converted to Archive after the objects expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Transit to Cold Archive Storage Class**|Specify when objects that match the rule expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle**Specify when objects that match the rule expire. The storage class of the objects is converted to Cold Archive after the objects expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete**|Specify when objects that match the rule expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle**Specify when objects that match the rule expire. The objects are deleted after they expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete Parts**|**Part Lifecycle**|Specify the operations to perform on expired parts. You can set Part Lifecycle to **Validity Period \(Days\)**, **Expiration Data**, or **Disabled**. If you select **Disabled**, the configurations of Part Lifecycle do not take effect. **Note:**

        -   You must configure at least one of **File Lifecycle** and **Part Lifecycle**.
        -   If you select **Tagging**, **Part Lifecycle** is unavailable. |
        |**Delete**|Specify when parts that match the rule expire based on **Validity Period \(Days\)** or **Expiration Data** that you set for **Part Lifecycle**. Expired parts are deleted. The configuration method is the same as that for **Transit to IA Storage Class**.|

        **Note:** For more information about the conversion between storage classes, see [Convert storage classes](/intl.en-US/Developer Guide/Storage classes/Convert storage classes.md). For more information about the pricing after the storage classes of objects are converted, see [Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.mdsection_e1n_vlx_dgb).

    -   Parameters for versioned buckets

        Parameters in the **Basic Settings** and **Delete Parts** sections are configured in the same way as that used to configure the same parameters for unversioned buckets. The following table describes only the parameters that are different from the parameters configured for versioned buckets.

        |Category|Parameter|Description|
        |--------|---------|-----------|
        |**Current Version**|**Clean Up Delete Marker**|If you enable versioning for the bucket, you can configure the **Clean Up Delete Marker** parameter. Other parameters are the same as those that you can configure for unversioned buckets. After you select Clean Up Delete Marker, if an object has only one version and the version is a delete marker, OSS considers the delete marker expired and deletes the delete marker. If an object has multiple versions and the current version of the object is a delete marker, OSS retains the delete marker. For more information about delete markers, see [Delete marker](/intl.en-US/Developer Guide/Data security/Versioning/Delete marker.md). |
        |**Previous Versions**|**File Lifecycle**|Specify when previous versions expire. Valid values: **Validity Period \(Days\)** or **Disabled**. If you select **Disabled**, File Lifecycle does not take effect.|
        |**Transit to IA Storage Class**|Specify the number of days that objects can be retained as previous versions. The storage class of the previous versions is converted to IA one day after previous versions expire. For example, if you set Validity Period \(Days\) to 30, the storage class of the objects that become previous versions on January 1, 2021 expires on January 30, 2021 is converted to IA on February 1, 2021. **Note:** You can determine when an object becomes a previous version based on the time when the newer version is generated. |
        |**Transit to Archive Storage Class**|Specify the number of days that objects can be retained as previous versions. The storage class of the previous versions is converted to Archive one day after the previous versions expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Transit to Cold Archive Storage Class**|Specify the number of days that objects can be retained as previous versions. The storage class of the previous versions is converted to Cold Archive one day after previous versions expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete**|Specify the number of days that objects can be retained as previous versions. The previous versions are deleted one day after they expire. The configuration method is the same as that for **Transit to IA Storage Class**. **Note:** If the current version of an object is deleted based on a lifecycle rule, OSS does not delete the current version but converts the current version to a previous version and adds a delete marker to the object as the new current version. If a previous version of an object is deleted based on a lifecycle rule, OSS deletes the previous version. If you configure a lifecycle rule to delete previous versions, delete markers that are stored as previous versions are also deleted. |

5.  Click **OK**.

    After a lifecycle rule is saved, you can view the rule in the lifecycle rule list. You can also click **Edit** or **Delete** in the Actions column that correspond to the lifecycle rule to edit or delete the lifecycle rule.


