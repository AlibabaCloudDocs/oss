Cross-region replication in specific scenarios 
===================================================================

This topic describes how cross-region replication works when it is used with versioning, lifecycle rules, server-side encryption, and retention policies.

Use cross-region replication with versioning 
-----------------------------------------------------------------

Note the following limits when you use cross-region replication with versioning:

* You can enable cross-region replication only between two buckets that are both versioned or unversioned. The versioning state of the source bucket and the destination bucket cannot be changed.

  

* Versioning cannot be suspended for the source bucket and destination bucket during data replication.To suspend versioning for the source bucket and destination bucket, you can delete the cross-region replication rule configured for the buckets first.

  




The following table describes the operations performed by OSS in cross-region replication when an object is deleted from the versioned source bucket.


|                            Request method                             | Synchronization policy |                                                    Result                                                     |
|-----------------------------------------------------------------------|------------------------|---------------------------------------------------------------------------------------------------------------|
| Send a DeleteObject request in which the version ID is not specified. | Add/Change             | A delete marker is created for the object in the source bucket and is synchronized to the destination bucket. |
| Send a DeleteObject request in which the version ID is not specified. | Add/Delete/Change      | A delete marker is created for the object in the source bucket and is synchronized to the destination bucket. |
| Send a DeleteObject request in which the version ID is specified.     | Add/Change             | The deletion is not synchronized to the destination bucket.                                                   |
| Send a DeleteObject request in which the version ID is specified.     | Add/Delete/Change      | The deletion is synchronized to the destination bucket.                                                       |



For more information about how to configure data synchronization policy for versioned buckets, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

Use cross-region replication with lifecycle rules 
----------------------------------------------------------------------

When you use cross-region replication with versioning, multiple previous object versions are synchronized to the destination bucket and incur additional storage costs. To reduce the costs, we recommend that you configure lifecycle rules for buckets to control storage costs and retain required data. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md).

Note the following items when you use cross-region replication with lifecycle rules:

* In cross-region replication, only the operations performed based on the lifecycle rules but not the lifecycle rules are synchronized to the destination bucket. To apply the same lifecyle rules as the source bucket on the objects in the destination buckets, configure the same lifecycle rules for the destination bucket.

  

* If a lifecycle rule is configured for the source bucket, the created time of an object replicated to the destination bucket is the time when the object is created in the source bucket but not the time when it is replicated to the destination bucket.

  

* If an object is deleted from the source bucket based on the lifecycle rule when it is replicating to the destination bucket, the replication may continue and the object replicated to the destination bucket is retained.

  




Use cross-region replication with server-side encryption 
-----------------------------------------------------------------------------

Cross-region replication supports unencrypted objects and objects encrypted by using SSE-KMS and SSE-OSS. For more information, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

The following table describes the encryption status of the destination object when cross-region replication is used with server-side encryption.


|      Encryption status of the source object      |           Encryption status of the destination bucket            |    Whether SSE-KMS is used    |              Encryption status of the destination object               |
|--------------------------------------------------|------------------------------------------------------------------|-------------------------------|------------------------------------------------------------------------|
|   Unencrypted    | Unencrypted                                                      | Not impacted                  | Unencrypted                                                            |
|   Unencrypted    | Unencrypted                                                      | Not impacted                  | Unencrypted                                                            |
|   Unencrypted    |  SSE-OSS                                         |  Not impacted |  SSE-OSS                               |
|   Unencrypted    |  SSE-OSS                                         |  Not impacted |  SSE-OSS                               |
|   Unencrypted    | SSE-KMS without a specified CMK ID                               | Not impacted                  | SSE-KMS without a specified CMK ID                                     |
|   Unencrypted    |  SSE-KMS with a specified CMK ID | Yes                           | SSE-KMS with a specified CMK ID                                        |
|   Unencrypted    |  SSE-KMS with a specified CMK ID | No                            | N/A. The source object cannot be replicated to the destination bucket. |
| SSE-OSS                                          | Not limited                                                      | Not impacted                  | SSE-OSS                                                                |
| SSE-KMS without a specified CMK ID               | Not limited                                                      | Not impacted                  | SSE-KMS without a specified CMK ID                                     |
|  SSE-KMS with a specified CMK ID |  Not limited                                     |  Yes          | SSE-KMS with a specified CMK ID                        |
|  SSE-KMS with a specified CMK ID |  Not limited                                     |  No           | N/A. The source object cannot be replicated to the destination bucket. |



Use cross-region replication with retention policies 
-------------------------------------------------------------------------

After a retention policy configured for a bucket is locked, you can read objects from or upload objects to the bucket. However, the objects in the bucket cannot be overwritten or deleted within the retention period.

For more information about retention policies, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

The following table describes whether the source object can be synchronized to the destination bucket when cross-region replication is used with retention policies.


| Whether the source object is in the retention period  | Allowed operation in the source bucket | Whether the destination object in the retention period | Whether the source object is synchronized to the destination bucket |
|-------------------------------------------------------|----------------------------------------|--------------------------------------------------------|---------------------------------------------------------------------|
|    No | Create an object                       | Yes                                                    | No                                                                  |
|    No | Overwrite an object                    | Yes                                                    | No                                                                  |
|    No | Delete an object                       | Yes                                                    | No                                                                  |
| No                                                    | Create an object                       | No                                                     | Yes                                                                 |
| No                                                    | Overwrite an object                    | No                                                     | Yes                                                                 |
| No                                                    | Delete an object                       | No                                                     | Yes                                                                 |
| Yes                                                   | Create an object                       | Not impacted                                           | Yes                                                                 |







