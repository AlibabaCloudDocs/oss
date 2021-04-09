List of supported API operations 
=====================================================

This topic describes the supported API operations when the hierarchical namespace feature is disabled or enabled for a bucket. 
**Note**

In the following table, a check (√) sign indicates that the solution is supported. A cross (×) sign indicates that the solution is not supported.

Service operations 
---------------------------------------



|                                                                 API                                                                  | Hierarchical namespace disabled | Hierarchical namespace enabled |
|--------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|--------------------------------|
| [GetService (ListBuckets)](/intl.en-US/API Reference/Service operations/GetService (ListBuckets).md) | √                               | √                              |





Bucket operations 
--------------------------------------




|                                                                                              API                                                                                               || Hierarchical namespace disabled | Hierarchical namespace enabled |
|--------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|--------------------------------|
| Basic operations                     | [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md)                                                   | √                               | √                              |
| Basic operations                     | [DeleteBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/DeleteBucket.md)                                             | √                               | √                              |
| Basic operations                     | [GetBucket (ListObjects)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md)                       | √                               | √                              |
| Basic operations                     | [GetBucketV2 (ListObjectsV2)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md)               | √                               | √                              |
| Basic operations                     | [GetBucketInfo](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketInfo.md)                                           | √                               | √                              |
| Basic operations                     | [GetBucketLocation](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketLocation.md)                                   | √                               | √                              |
| Retention policy                     | [InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md)                                 | √                               | ×                              |
| Retention policy                     | [AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md)                                       | √                               | ×                              |
| Retention policy                     | [CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md)                                 | √                               | ×                              |
| Retention policy                     | [ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md)                                     | √                               | ×                              |
| Retention policy                     | [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md)                                           | √                               | ×                              |
| Access control list (ACL)            | [PutBucketAcl](/intl.en-US/API Reference/Bucket operations/ACL/PutBucketAcl.md)                                                          | √                               | √                              |
| Access control list (ACL)            | [GetBucketAcl](/intl.en-US/API Reference/Bucket operations/ACL/GetBucketAcl.md)                                                          | √                               | √                              |
| Lifecycle                            | [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md)                                        | √                               | ×                              |
| Lifecycle                            | [GetBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/GetBucketLifecycle.md)                                        | √                               | ×                              |
| Lifecycle                            | [DeleteBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/DeleteBucketLifecycle.md)                                  | √                               | ×                              |
| Versioning                           | [PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md)                                     | √                               | ×                              |
| Versioning                           | [GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md)                                     | √                               | ×                              |
| Versioning                           | [GetBucketVersions(ListObjectVersions)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md) | √                               | ×                              |
| Cross-region replication (CRR)       | [PutBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region replication/PutBucketReplication.md)                     | √                               | ×                              |
| Cross-region replication (CRR)       | [GetBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplication.md)                     | √                               | ×                              |
| Cross-region replication (CRR)       | [GetBucketReplicationLocation](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplicationLocation.md)     | √                               | ×                              |
| Cross-region replication (CRR)       | [GetBucketReplicationProgress](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplicationProgress.md)     | √                               | ×                              |
| Cross-region replication (CRR)       | [DeleteBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region replication/DeleteBucketReplication.md)               | √                               | ×                              |
| Authorization policy                 | [PutBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/PutBucketPolicy.md)                                   | √                               | √                              |
| Authorization policy                 | [GetBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/GetBucketPolicy.md)                                   | √                               | √                              |
| Authorization policy                 | [DeleteBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/DeleteBucketPolicy.md)                             | √                               | √                              |
| Inventory                            | [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md)                                        | √                               | ×                              |
| Inventory                            | [GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md)                                        | √                               | ×                              |
| Inventory                            | [ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md)                                      | √                               | ×                              |
| Inventory                            | [DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md)                                  | √                               | ×                              |
| Logging                              | [PutBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/PutBucketLogging.md)                                              | √                               | √                              |
| Logging                              | [GetBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/GetBucketLogging.md)                                              | √                               | √                              |
| Logging                              | [DeleteBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/DeleteBucketLogging.md)                                        | √                               | √                              |
| Static website hosting               | [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md)                                      | √                               | ×                              |
| Static website hosting               | [GetBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/GetBucketWebsite.md)                                      | √                               | ×                              |
| Static website hosting               | [DeleteBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/DeleteBucketWebsite.md)                                | √                               | ×                              |
| Hotlink protection                   | [PutBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/PutBucketReferer.md)                                   | √                               | √                              |
| Hotlink protection                   | [GetBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/GetBucketReferer.md)                                   | √                               | √                              |
| Tag                                  | [PutBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/PutBucketTags.md)                                                    | √                               | √                              |
| Tag                                  | [GetObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/GetObjectTagging.md)                                              | √                               | √                              |
| Tag                                  | [DeleteBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/DeleteBucketTags.md)                                              | √                               | √                              |
| Encryption                           | [PutBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/PutBucketEncryption.md)                                     | √                               | √                              |
| Encryption                           | [GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md)                                     | √                               | √                              |
| Encryption                           | [DeleteBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/DeleteBucketEncryption.md)                               | √                               | √                              |
| Pay-by-requester                     | [PutBucketRequestPayment](/intl.en-US/API Reference/Bucket operations/Pay-by-requester/PutBucketRequestPayment.md)                       | √                               | √                              |
| Pay-by-requester                     | [GetBucketRequestPayment](/intl.en-US/API Reference/Bucket operations/Pay-by-requester/GetBucketRequestPayment.md)                       | √                               | √                              |
| Cross-origin resource sharing (CORS) | [PutBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md)                                                       | √                               | ×                              |
| Cross-origin resource sharing (CORS) | [GetBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/GetBucketCors.md)                                                       | √                               | ×                              |
| Cross-origin resource sharing (CORS) | [DeleteBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/DeleteBucketCors.md)                                                 | √                               | ×                              |
| Cross-origin resource sharing (CORS) | [OptionObject](/intl.en-US/API Reference/Bucket operations/CORS/OptionObject.md)                                                         | √                               | ×                              |



Object operations 
--------------------------------------




|                                                                              API                                                                              || Hierarchical namespace disabled | Hierarchical namespace enabled |
|---------------------------|------------------------------------------------------------------------------------------------------------------------------------|---------------------------------|--------------------------------|
| Basic operations          | [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md)                             | √                               | √                              |
| Basic operations          | [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md)                             | √                               | √                              |
| Basic operations          | [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md)                           | √                               | √                              |
| Basic operations          | [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md)                       | √                               | ×                              |
| Basic operations          | [DeleteObject](/intl.en-US/API Reference/Object operations/Basic operations/DeleteObject.md)                       | √                               | √                              |
| Basic operations          | [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md)     | √                               | ×                              |
| Basic operations          | [HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md)                           | √                               | √                              |
| Basic operations          | [GetObjectMeta](/intl.en-US/API Reference/Object operations/Basic operations/GetObjectMeta.md)                     | √                               | √                              |
| Basic operations          | [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md)                           | √                               | √                              |
| Basic operations          | [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md)                               | √                               | ×                              |
| Basic operations          | [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md)                     | √                               | ×                              |
| Basic operations          | [SelectObject](/intl.en-US/API Reference/Object operations/Basic operations/SelectObject.md)                       | √                               | ×                              |
| Directory management      | [CreateDirectory](/intl.en-US/API Reference/Object operations/Directory management/CreateDirectory.md)             | ×                               | √                              |
| Directory management      | [Rename](/intl.en-US/API Reference/Object operations/Directory management/Rename.md)                               | ×                               | √                              |
| Directory management      | [DeleteDirectory](/intl.en-US/API Reference/Object operations/Directory management/DeleteDirectory.md)             | ×                               | √                              |
| Multipart upload          | [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md) | √                               | √                              |
| Multipart upload          | [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md)                           | √                               | √                              |
| Multipart upload          | [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md)                   | √                               | √                              |
| Multipart upload          | [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md) | √                               | √                              |
| Multipart upload          | [AbortMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/AbortMultipartUpload.md)       | √                               | √                              |
| Multipart upload          | [ListMultipartUploads](/intl.en-US/API Reference/Object operations/Multipart upload/ListMultipartUploads.md)       | √                               | √                              |
| Multipart upload          | [ListParts](/intl.en-US/API Reference/Object operations/Multipart upload/ListParts.md)                             | √                               | √                              |
| Access control list (ACL) | [PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md)                                    | √                               | √                              |
| Access control list (ACL) | [GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md)                                    | √                               | √                              |
| Symbolic link             | [PutSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/PutSymlink.md)                              | √                               | ×                              |
| Symbolic link             | [GetSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/GetSymlink.md)                              | √                               | ×                              |
| Tagging                   | [PutObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/PutObjectTagging.md)                        | √                               | √                              |
| Tagging                   | [GetObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/GetObjectTagging.md)                        | √                               | √                              |
| Tagging                   | [DeleteObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/DeleteObjectTagging.md)                  | √                               | √                              |



LiveChanne operations 
------------------------------------------



|                                                            API                                                             | Hierarchical namespace disabled | Hierarchical namespace enabled |
|----------------------------------------------------------------------------------------------------------------------------|---------------------------------|--------------------------------|
| [PutLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md)               | √                               | ×                              |
| [ListLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/ListLiveChannel.md)             | √                               | ×                              |
| [DeleteLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/DeleteLiveChannel.md)         | √                               | ×                              |
| [PutLiveChannelStatus](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannelStatus.md)   | √                               | ×                              |
| [GetLiveChannelInfo](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelInfo.md)       | √                               | ×                              |
| [GetLiveChannelStat](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelStat.md)       | √                               | ×                              |
| [GetLiveChannelHistory](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelHistory.md) | √                               | ×                              |
| [PostVodPlaylist](/intl.en-US/API Reference/LiveChannel-related operations/PostVodPlaylist.md)             | √                               | ×                              |
| [GetVodPlaylist](/intl.en-US/API Reference/LiveChannel-related operations/GetVodPlaylist.md)               | √                               | ×                              |



For more information about the hierarchical namespace feature, see [Hierarchical namespace](/intl.en-US/Developer Guide/Objects/Hierarchical namespace.md).

