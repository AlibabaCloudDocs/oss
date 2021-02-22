# Can I recover an OSS object after the object is deleted or overwritten?

The redundancy mechanism of the OSS backend is used to recover data when server or hardware failures occur. Alibaba Cloud cannot recover OSS data that is manually deleted, overwritten, or automatically deleted by configuration rules.

## Data deletion and overwriting

OSSThe following items are the description about user data management in Service Terms and Security Center Service Level Agreement \(SLA\).

-   The following items are the description about user data management in Service Terms:
    -   You can delete, change, and manage your business data. If you manually release services or delete data, Alibaba Cloud does not retain the data.
    -   If the user business data is deleted, the data cannot be recovered. You assume the consequences and responsibilities that result from the deletion of such data on your own. You understand and agree that Alibaba Cloud has no obligation to continue to retain, export, or return the user business data.
-   The following content is the description about data destructibility in SLA:

    If you manually delete data or want to delete data when your services expire, Alibaba Cloud automatically deletes disk data and clears memory on the corresponding physical server. The data cannot be recovered.


## Operations that may cause data to be deleted or overwritten

Your data may be deleted or overwritten when you use the following methods. Proceed with caution.

-   Delete objects by using the OSS console, ossutil, ossbrowser, or SDKs. For more information, see [Delete objects](/intl.en-US/Developer Guide/Objects/Manage files/Delete objects.md).
-   Upload an object that has the same name as an existing object to OSS by using the OSS console, ossutil, ossbrowser, or SDKs. The existing object is overwritten by the uploaded object.
-   Configure lifecycle rules to delete objects on a regular basis. OSS automatically deletes objects based on the lifecycle rules. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
-   Configure cross-region replication \(CRR\) rules for your bucket and set **Add/Delete/Change** to synchronize data from the source bucket to your bucket. If objects in the source bucket are modified or deleted, the changes are synchronized to the destination bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).
-   Other users delete or overwrite your objects because access permissions on the bucket are improperly configured. For more information about access permissions, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

## How do I avoid accidental operations on my objects?

You can use one of the following methods to prevent your objects from being deleted or overwritten:

-   Enable versioning

    When you enable versioning for a bucket, the objects that are deleted or overwritten are stored as previous versions in the bucket. You can recover an object to a previous version at any time. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

-   Use CRR to back up objects

    You can configure CRR rules for your bucket and set **Add/Delete/Change** to synchronize data from your bucket to another bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

-   Configure scheduled backup

    You can use Hybrid Backup Recovery \(HBR\) to back up your objects. This allows you to recover your objects when the objects are lost. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md).

-   Configure appropriate access permissions

    Take note of the following principles to grant appropriate access permissions to other users who access your bucket:

    -   Do not use your Alibaba Cloud account to access OSS.
    -   Grant read and write permissions to different RAM users. Use a RAM user that has only read permissions or an Security Token Service \(STS\) temporary access credential to access read-only data.
    -   We recommend that you provide an STS temporary access credential to the users who need to only temporarily access your data.
    -   Grant the least but sufficient access permissions on OSS data for different businesses.
    -   Use a secure location to store credentials for data access such as the password of your Alibaba Cloud account and the access credentials of a RAM user.
    For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).


