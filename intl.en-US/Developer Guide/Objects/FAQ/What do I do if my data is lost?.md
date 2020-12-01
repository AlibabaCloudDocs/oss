# What do I do if my data is lost?

OSS is a distributed storage service that ensures data durability based on automatic backup for redundancy. To prevent data loss, OSS keeps data secure and intact.

However, data may be deleted in the following situations:

-   Lifecycle rules

    If you have configured lifecycle rules to automatically delete objects, the system deletes data based on the lifecycle configurations. We recommend that you configure lifecycle rules based on actual requirements. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

-   Bucket ACL set to public read/write
    -   Bucket ACL set to public read/write: After you set the ACL of a bucket to public read/write, all users can read and write the objects in the bucket. We recommend that you do not set the bucket ACL to public read/write unless necessary. For more information, see [Set the ACL for a bucket](/intl.en-US/Developer Guide/Buckets/Set the ACL for a bucket.md).
    -   Bucket policy that allows all users to write and read objects in a bucket: After you set the bucket ACL to public read, all users can read and write the objects in the bucket. We recommend that you do not set the bucket ACL to public read unless necessary. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).
-   Leak of accounts that have the management permissions on buckets: After an account and password that have the management permissions on a bucket are leaked, users who have the account can perform operations on the objects in your bucket. We recommend that you use RAM users to access resources during routine management, and grant fine-grained permissions to the RAM users. If you find that the account is leaked, change the password of the RAM user and disable the AccessKey pair to minimize impacts. For more information, see [Overview of a RAM user](/intl.en-US/RAM User Management/Overview of a RAM user.md).
-   Accidental deletes by administrators: After objects in OSS are deleted, the deleted objects cannot be retrieved. To prevent data from being accidentally overwritten and deleted, we recommend that you use the following features:
    -   CRR: Back up your bucket data to another bucket. Objects in the source bucket is deleted. You can find the objects in the backup bucket. For more information, see [Cross-region replication](/intl.en-US/Developer Guide/Data security/Disaster recovery/Cross-region replication.md).
    -   Scheduled backup: After scheduled backup is enabled, OSS backs up your data to Hybrid Backup Recovery \(HBR\) on a regular basis. This way, objects can be recovered when they are lost. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md).
    -   Versioning: After you enable versioning, overwritten or deleted objects are saved as previous versions and can be recovered if necessary. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).
    -   Retention policy: You can enable retention policy for important data. Data within the retention period cannot be overwritten or deleted. For more information, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

