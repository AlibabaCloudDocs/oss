# Configure lifecycle rules

You can configure lifecycle rules to convert the storage class of objects in a bucket or delete specified objects and parts from a bucket.

-   After a lifecycle rule is configured, it is loaded within 24 hours and takes effect within 24 hours after it is loaded. Check the configurations of a rule before you save the rule.
-   Objects that are deleted based on lifecycle rules cannot be recovered. Configure lifecycle rules based on your needs.
-   You can configure up to 100 lifecycle rules in the OSS console. To configure more than 100 rules, use ossutil or OSS SDKs. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Lifecycle**. In the **Lifecycle** section, click **Configure**.

4.  Click **Create Rule**. In the Create Rule panel, configure the following parameters.

    -   Parameters for unversioned buckets

        |Parameter|Description|
        |---------|-----------|
        |**Basic Settings**|
        |**Status**|Specify the status of the lifecycle rule. Valid values: **Enabled** and **Disabled**.|
        |**Applied To**|Select policies used to match objects with the rule. You can select **Files with Specified Prefix** or **Whole Bucket**. Files with Specified Prefix indicates that this rule applies to objects whose names contain a specified prefix. Whole Bucket indicates that this rule applies to all objects in the bucket. **Note:** If you select **Files with Specified Prefix**, you can configure multiple lifecycle rules for objects whose names contain different prefixes. If you select **Whole Bucket**, only one lifecycle rule can be configured for the bucket. |
        |**Prefix**|If you set **Applied To** to **Files with Specified Prefix**, you must specify the prefix of the objects to which the rule applies. For example, if you want the rule applies to objects whose names start with img, enter img.|
        |**Tagging**|Select tagging and configure tags. The rule applies only to objects that have the specified tags. Example: Select **Files with Specified Prefix** and set Prefix to img, Key to a, and Value to 1. The rule applies to all objects whose names start with img and whose tagging configurations are a=1. For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).|
        |**Clear Policy**|
        |**File Lifecycle**|Configure object lifecycle to specify when objects expire. You can set File Lifecycle to **Validity Period \(Days\)**, **Expiration Date**, or **Disabled**. If you select **Disabled**, File Lifecycle does not take effect.|
        |**Transit to IA Storage Class**|Specify when objects expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle**. The storage class of the objects is converted to IA after they expire.         -   **Validity Period \(Days\)**: specifies the number of days within which objects can be retained after they are last modified. The next day after they expire, the storage class of the objects is converted to IA. For example, if you set Validity Period \(Days\) to 30, the storage class of the objects that are last modified on January 1, 2019 is converted to IA on February 1, 2019.
        -   **Expiration Date**: specifies a date based on which all objects that are last modified before this date expire and the storage class of these objects is converted to IA. For example, if you set Expiration Date to 2019-1-1, the storage class of objects that are last modified before January 1, 2019 is converted to IA.
**Note:**

        -   Only Standard objects can be converted to IA objects based on lifecycle rules.
        -   For more information about the pricing after the storage classes of objects are converted, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.mdsection_e1n_vlx_dgb). |
        |**Transit to Archive Storage Class**|Specify when objects expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle** The storage class of the objects that match the rule is converted to Archive after they expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Transit to Cold Archive Storage Class**|Specify when objects expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle** The storage class of the objects that match the rule is converted to Cold Archive after they expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete**|Specify when objects expire based on **Validity Period \(Days\)** or **Expiration Date** that you set for **File Lifecycle** The objects that match the rule are deleted after they expire. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete Parts**|
        |**Part Lifecycle**|Specify the operations to perform on expired parts. You can set Part Lifecycle to **Validity Period \(Days\)**, **Expiration Data**, or **Disabled**. If you select **Disabled**, Part Lifecycle does not take effect. **Note:**

        -   Configure at least one of **File Lifecycle** and **Part Lifecycle**.
        -   If you select **Tagging**, **Part Lifecycle** is unavailable. |
        |**Delete**|Specify when parts expire based on **Validity Period \(Days\)** or **Expiration Data** that you set for **Part Lifecycle**. The parts are deleted after they expire. The configuration method is the same as that for **Transit to IA Storage Class**.|

    -   Parameters for versioned buckets

        After versioning is enabled for a bucket, you can manage previous versions of objects in the bucket. For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

        After versioning is enabled, the lifecycle configuration methods of the current versions and parts for versioned objects are the same as those for unversioned objects. The following table describes the parameters that you must configure for previous versions.

        |Parameter|Description|
        |---------|-----------|
        |**Current Version**|
        |**Clean Up Delete Marker**|After versioning is enabled, the **Clean Up Delete Marker** parameter is added. Other parameters are the same as those when the bucket is unversioned.Select **Clean Up Delete Marker**. If the object has only a current version and the current version is a delete marker, OSS deletes the expired delete marker. For more information about delete markers, see [Delete marker](/intl.en-US/Developer Guide/Data security/Versioning/Delete marker.md). |
        |**Previous Versions**|
        |**File Lifecycle**|Configure objects lifecycle to specify when previous versions expire. You can set File Lifecycle to **Validity Period \(Days\)** or **Disabled**. If you select **Disabled**, File Lifecycle does not take effect.|
        |**Transit to IA Storage Class**|Specify the number of days within which objects can be retained after they become previous versions. The next day after they expire, the storage class of the previous versions is converted to IA. For example, if you set Validity Period \(Days\) to 30, the storage class of the objects that become previous versions on January 1, 2019 is converted to IA on February 1, 2019. **Note:**

        -   Only Standard objects can be converted to IA objects based on lifecycle rules.
        -   You can determine the time when an object becomes a previous version based on the date the later version is last modified. |
        |**Transit to Archive Storage Class**|Specify the number of days within which objects can be retained after they become previous versions. The next day after they expire, the storage class of the previous versions is converted to Archive. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Transit to Cold Archive Storage Class**|Specify the number of days within which objects can be retained after they become previous versions. The day after they expire, the storage class of the previous versions is converted to Cold Archive. The configuration method is the same as that for **Transit to IA Storage Class**.|
        |**Delete**|Specify the number of days within which objects can be retained after they become previous versions. The day after they expire, the previous versions are deleted. The configuration method is the same as that for **Transit to IA Storage Class**. **Note:** The delete operation on previous versions also applies to delete markers. |

        If a lifecycle rule deletes a current version, OSS does not directly delete the current version. However, OSS changes the current version to a previous version and adds a delete marker. If a lifecycle rule deletes a previous version, the previous version is deleted.

5.  Click **OK**.

    After a lifecycle rule is saved, you can view the rule in lifecycle rule list. You can also click **Edit** or **Delete** in the Actions column corresponding to the lifecycle rule that you want to edit or delete.


