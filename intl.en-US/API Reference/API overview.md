# API overview

This topic describes the API operations provided by OSS and their usage.

## Service operations

|API|Description|
|---|-----------|
|[GetService \(ListBuckets\)](/intl.en-US/API Reference/Service operations/GetService (ListBuckets).md)|Lists all buckets owned by the requester.|

## Bucket operations

|API|Description|
|:--|:----------|
|[PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md)|Creates a bucket.|
|[DeleteBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/DeleteBucket.md)|Deletes a bucket.|
|[GetBucket\(ListObject\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md)|Lists the information about all objects in a bucket.|
|[GetBucketInfo](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketInfo.md)|Queries the information about a bucket.|
|[GetBucketLocation](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketLocation.md)|Queries the region of a bucket.|
|[PutBucketAcl](/intl.en-US/API Reference/Bucket operations/ACL/PutBucketAcl.md)|Configures the ACL of a bucket.|
|[GetBucketAcl](/intl.en-US/API Reference/Bucket operations/ACL/GetBucketAcl.md)|Queries the ACL of a bucket|
|[PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md)|Configures lifecycle rules for the objects in a bucket.|
|[GetBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/GetBucketLifecycle.md)|Queries the lifecycle rules configured for the objects in a bucket.|
|[DeleteBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/DeleteBucketLifecycle.md)|Deletes the lifecycle rules configured for the objects in a bucket.|
|[PutBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/PutBucketVersioning.md)|Configures the versioning status of a bucket.|
|[GetBucketVersioning](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersioning.md)|Queries the versioning status of a bucket.|
|[GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md)|Lists the versions of all objects in a bucket.|
|[PutBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region Replication/PutBucketReplication.md)|Configures cross-region replication \(CRR\) rules for a bucket.|
|[GetBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region Replication/GetBucketReplication.md)|Queries the CRR rules configured for a bucket.|
|[GetBucketReplicationLocation](/intl.en-US/API Reference/Bucket operations/Cross-region Replication/GetBucketReplicationLocation.md)|Queries the regions in which the destination bucket can be located.|
|[GetBucketReplicationProgress](/intl.en-US/API Reference/Bucket operations/Cross-region Replication/GetBucketReplicationProgress.md)|Queries the progress of a CRR task performed on a bucket.|
|[DeleteBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region Replication/DeleteBucketReplication.md)|Disables CRR for a bucket and deletes the CRR rule configured for the bucket.|
|[PutBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/PutBucketPolicy.md)|Configures policies for a bucket.|
|[GetBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/GetBucketPolicy.md)|Queries the policies configured for a bucket.|
|[DeleteBucketPolicy](/intl.en-US/API Reference/Bucket operations/Authorization policy/DeleteBucketPolicy.md)|Deletes the policies configured for a bucket.|
|[PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md)|Configures inventory rules for a bucket.|
|[GetBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/GetBucketInventory.md)|Queries a specified inventory task configured for a bucket.|
|[ListBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/ListBucketInventory.md)|Queries all inventory tasks configured for a bucket.|
|[DeleteBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/DeleteBucketInventory.md)|Deletes a specified inventory task configured for a bucket.|
|[InitiateBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/InitiateBucketWorm.md)|Creates a retention policy.|
|[AbortBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/AbortBucketWorm.md)|Deletes an unlocked retention policy.|
|[CompleteBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/CompleteBucketWorm.md)|Locks a retention policy.|
|[ExtendBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/ExtendBucketWorm.md)|Extends the retention duration \(days\) of objects in a bucket for which a retention policy is locked.|
|[GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md)|Queries the retention policy configured for a bucket.|
|[PutBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/PutBucketLogging.md)|Enables logging for a bucket.|
|[GetBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/GetBucketLogging.md)|Queries the logging configuration of a bucket.|
|[DeleteBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/DeleteBucketLogging.md)|Disables logging for a bucket.|
|[PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md)|Sets a bucket to the static website hosting mode.|
|[GetBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/GetBucketWebsite.md)|Queries the static website hosting status of a bucket.|
|[DeleteBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/DeleteBucketWebsite.md)|Disable the static website hosting mode for a bucket.|
|[PutBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/PutBucketReferer.md)|Configure hotlink protection rules for a bucket.|
|[GetBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/GetBucketReferer.md)|Queries the hotlink protection rules configured for a bucket.|
|[PutBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/PutBucketTags.md)|Adds tags to a bucket or modifies the tags of a bucket.|
|[GetBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/GetBucketTags.md)|Queries the tags of a bucket.|
|[DeleteBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/DeleteBucketTags.md)|Deletes the tags of a bucket.|
|[PutBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/PutBucketEncryption.md)|Configures encryption rules for a bucket.|
|[GetBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/GetBucketEncryption.md)|Queries the encryption rules configured for a bucket.|
|[DeleteBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/DeleteBucketEncryption.md)|Deletes the encryption rules configured for a bucket.|
|[PutBucketRequestPayment](/intl.en-US/API Reference/Bucket operations/Pay-by-requester/PutBucketRequestPayment.md)|Enables pay-by-requester for a bucket.|
|[GetBucketRequestPayment](/intl.en-US/API Reference/Bucket operations/Pay-by-requester/GetBucketRequestPayment.md)|Queries the pay-by-requester configurations of a bucket.|

## Object operations

|API|Description|
|:--|:----------|
|[PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md)|Uploads an object.|
|[CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md)|Copies an object to another object.|
|[GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md)|Obtains an object.|
|[AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md)|Uploads an object by appending the object to an existing object.|
|[DeleteObject](/intl.en-US/API Reference/Object operations/Basic operations/DeleteObject.md)|Deletes an object.|
|[DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md)|Deletes multiple objects.|
|[HeadObject](/intl.en-US/API Reference/Object operations/Basic operations/HeadObject.md)|Queries only the metadata of an object but not the object content.|
|[GetObjectMeta](/intl.en-US/API Reference/Object operations/Basic operations/GetObjectMeta.md)|Queries only the basic metadata of an object, including ETag, Size, and LastModified, but not the object content.|
|[PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md)|Uploads an object by using an HTML form.|
|[PutObjectACL](/intl.en-US/API Reference/Object operations/ACL/PutObjectACL.md)|Modifies the ACL of an object.|
|[GetObjectACL](/intl.en-US/API Reference/Object operations/ACL/GetObjectACL.md)|Queries the ACL of an object.|
|[Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md)|Enables callback in requests.|
|[PutSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/PutSymlink.md)|Creates a symbolic link.|
|[GetSymlink](/intl.en-US/API Reference/Object operations/Symbolic link/GetSymlink.md)|Queries a symbolic link.|
|[RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md)|Restores an object.|
|[SelectObject](/intl.en-US/API Reference/Object operations/Basic operations/SelectObject.md)|Queries objects by using SQL statements.|
|[PutObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/PutObjectTagging.md)|Configures or updates the tags of an object.|
|[GetObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/GetObjectTagging.md)|Queries the tags of an object.|
|[DeleteObjectTagging](/intl.en-US/API Reference/Object operations/Tagging/DeleteObjectTagging.md)|Delete specified tags of an object.|

## Multipart upload operations

|API|Description|
|:--|:----------|
|[InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md)|Initializes a multipart upload task.|
|[UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md)|Uploads an object by part.|
|[UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md)|Copies data from an existing object to upload a part.|
|[CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md)|Completes the multipart upload task of an object.|
|[AbortMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/AbortMultipartUpload.md)|Cancels a multipart upload tasks.|
|[ListMultipartUploads](/intl.en-US/API Reference/Object operations/Multipart upload/ListMultipartUploads.md)|Lists all ongoing multipart upload tasks|
|[ListParts](/intl.en-US/API Reference/Object operations/Multipart upload/ListParts.md)|Lists all parts that are uploaded in a multipart upload task that has a specified upload ID.|

## Cross-origin resource sharing \(CORS\) operations

|API|Description|
|:--|:----------|
|[PutBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md)|Configures CORS rules for a bucket.|
|[GetBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/GetBucketCors.md)|Queries the CORS rules configured for a bucket.|
|[DeleteBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/DeleteBucketCors.md)|Disables CORS for a bucket and deletes all CORS rules configured for the bucket.|
|[OptionObject](/intl.en-US/API Reference/Bucket operations/CORS/OptionObject.md)|Specifies preflight requests for cross-region access.|

## LiveChannel operations

|API|Description|
|---|-----------|
|[PutLiveChannelStatus](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannelStatus.md)|Switches the status of a LiveChannel.|
|[PutLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md)|Creates a LiveChannel.|
|[GetVodPlaylist](/intl.en-US/API Reference/LiveChannel-related operations/GetVodPlaylist.md)|Queries the playlist of a LiveChannel.|
|[PostVodPlaylist](/intl.en-US/API Reference/LiveChannel-related operations/PostVodPlaylist.md)|Creates a playlist for a LiveChannel.|
|[Get LiveChannelStat](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelStat.md)|Queries the ingestion status of a LiveChannel.|
|[GetLiveChannelInfo](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelInfo.md)|Queries the configuration information of a LiveChannel.|
|[GetLiveChannelHistory](/intl.en-US/API Reference/LiveChannel-related operations/GetLiveChannelHistory.md)|Queries the ingestion history of a LiveChannel.|
|[ListLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/ListLiveChannel.md)|Lists LiveChannels.|
|[DeleteLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/DeleteLiveChannel.md)|Deletes a LiveChannel.|

