# API operation calling fees

Operations in Object Storage Service \(OSS\) are implemented by calling OSS API operations. Fees are calculated based on the count of API operation calls.

## Billable items

OSS charges API operation calling fees based on the count of API operations that you call to send PUT requests and GET requests.

|Billable item|Description|Billing method|
|-------------|-----------|--------------|
|Count of PUT requests|Fees generated when you call OSS operations to send PUT requests. The counts of failed and successful requests are calculated.|-   Pay-as-you-go: Calling fees = Actual calls × Unit price per 10,000 calls/10000.

**Note:** You can call OSS API operations 50,000 times free of charge to perform operations on buckets located in the China \(Beijing\) and China \(Shenzhen\) regions. You are charged for additional calls.

For more information about the unit price of API operation calling fees, visit [OSS pricing page](https://www.alibabacloud.com/zh/product/oss/pricing).

-   Subscription: None. |
|Count of GET requests|Fees generated when you call OSS operations to send GET requests. The counts of failed and successful requests are calculated.|

**Note:**

-   Operations that you perform in the OSS console are implemented by calling OSS API operations. For example, GetService \(ListBuckets\) is called when you view the list of buckets. GetBucket \(ListObjects\) is called when you access the Files page on the OSS console. Therefore, OSS charges API operation calling fees for operations that you perform in the OSS console.
-   For more information about the operations used to sent PUT and GET requests, see [API overview](/intl.en-US/API Reference/API overview.md).

## API operations that initiate PUT requests

|API operation|Action|
|-------------|------|
|PutBucket|Creates a bucket.|
|GetService\(ListBuckets\)|Lists all buckets.|
|GetBucket \(ListObject\) and GetBucketV2 \(ListObjectsV2\)|Lists all objects.|
|PutBucketACL|Configures the access control list \(ACL\) of a bucket.|
|PutBucketInventory|Configure inventories for a bucket.|
|DeleteBucketInventory|Deletes a specific inventory task configured for a bucket.|
|PutBucketLogging|Enables logging for a bucket.|
|DeleteBucketLogging|Disables logging for a bucket.|
|PutBucketWebsite|Enables static website hosting for a bucket and configures redirection rules.|
|DeleteBucketWebsite|Disables static website hosting for a bucket.|
|PutBucketReferer|Configures the Referer whitelist and specifies whether an empty Referer field is allowed.|
|PutBucketLifecycle|Configures lifecycle rules for a bucket.|
|DeleteBucketLifecycle|Deletes lifecycle rules configured for a bucket.|
|DeleteBucket|Deletes a bucket.|
|PutObject|Uploads an object.|
|CopyObject|Copies objects to the same bucket or another bucket within the same region.|
|AppendObject|Uploads an object by appending the object to an existing object.|
|DeleteObject|Deletes a single object|
|DeleteMultipleObjects|Deletes multiple objects.|
|PutObjectACL|Configures the ACL of an object.|
|PostObject|Uploads an object by using an HTML form.|
|PutSymlink|Creates a symbolic link.|
|RestoreObject|Restores an Archive object.|
|InitiateMultipartUpload|Initiates a multipart upload task.|
|UploadPart|Uploads an object by part based on the specified object name and upload ID.|
|AbortMultipartUpload|Cancels a multipart upload task and deletes uploaded parts.|
|UploadPartCopy|Copies objects by part.|
|PutBucketReplication|Configures cross-region replication \(CRR\) rules for a bucket.|
|DeleteBucketReplication|Disables CRR for a bucket and deletes the CRR rules configured for the bucket.|
|PutBucketCors|Adds cross-origin resource sharing \(CORS\) configurations for a bucket.|
|DeleteBucketCors|Deletes the CORS configurations of a bucket.|
|CompleteMultipartUpload|Completes a multipart upload task.|
|InitiateBucketWorm|Configures a retention policy for a bucket.|
|AbortBucketWorm|Deletes an unlocked retention policy.|
|CompleteBucketWorm|Locks a retention policy.|
|ExtendBucketWorm|Extends the retention period of objects in a bucket for which a retention policy is locked.|
|PutBucketVersioning|Enables versioning for a bucket.|
|PutBucketPolicy|Configures a bucket policy.|
|DeleteBucketPolicy|Deletes a bucket policy.|
|PutBucketTags|Adds tags to a bucket or modifies the tags of a bucket.|
|DeleteBucketTags|Deletes the tags of a bucket.|
|PutBucketEncryption|Configure a data encryption rule for a bucket.|
|DeleteBucketEncryption|Deletes a data encryption rule configured for a bucket.|
|PutBucketRequestPayment|Enables pay-by-requester for the bucket.|
|PutObjectTagging|Adds tags to an object or modifies the tags of an object.|
|DeleteObjectTagging|Deletes the tags of an object.|
|PutLiveChannel|Creates a LiveChannel.|
|DeleteLiveChannel|Deletes the specified LiveChannel.|
|PutLiveChannelStatus|Switches the status of a LiveChannel.|
|PostVodPlaylist|Generates a playlist used for broadcasting for a LiveChannel.|

## API operations that initiate GET requests

|API operation|Action|
|-------------|------|
|GetBucketAcl|Queries the ACL of a bucket.|
|GetBucketLocation|Queries the data center where a bucket is located.|
|GetBucketInfo|Queries the information about a bucket.|
|GetBucketLogging|Queries the logging configurations of a bucket.|
|GetBucketWebsite|Queries the static website hosting configurations of a bucket.|
|GetBucketReferer|Queries the hotlink protection configurations of a bucket.|
|GetBucketLifecycle|Queries the lifecycle rules configured for a bucket.|
|GetBucketReplication|Queries the CRR rules configured for a bucket.|
|GetBucketReplicationLocation|Queries the regions in which the destination bucket can be located.|
|GetBucketReplicationProgress|Queries the progress of a CRR task that is performed for a bucket.|
|GetBucketInventory|Queries the specified inventory task configured for a bucket.|
|ListBucketInventory|Queries all inventory tasks configured for a bucket.|
|GetObject|Downloads an object.|
|CopyObject|Copies an object.|
|HeadObject|Queries all metadata of an object.|
|GetObjectMeta|Queries part of metadata of an object.|
|GetObjectACL|Queries the ACL of an object.|
|GetSymlink|Queries a symbolic link.|
|ListMultipartUploads|Lists all ongoing multipart upload tasks.|
|ListParts|Lists the uploaded parts.|
|UploadPartCopy|Copies objects by part.|
|GetBucketcors|Queries the CORS rules configured for a bucket.|
|GetBucketWorm|Queries the retention policies configured for a bucket.|
|GetBucketVersioning|Queries the versioning status of a bucket.|
|GetBucketVersions\(ListObjectVersions\)|Lists the versions of all objects in a bucket.|
|GetBucketPolicy|Queries the policies configured for a bucket.|
|GetBucketReferer|Queries the hotlink protection configurations of a bucket.|
|GetBucketTags|Queries the tags of a bucket.|
|GetBucketEncryption|Queries the encryption configurations of a bucket.|
|GetBucketRequestPayment|Queries the pay-by-requester configurations of a bucket.|
|SelectObject|Scans an object.|
|GetObjectTagging|Queries the tags of an object.|
|ListLiveChannel|Queries all LiveChannels that belongs to a user.|
|GetLiveChannelInfo|Queries the configurations of the specified LiveChannel.|
|GetLiveChannelStat|Queries the ingestion status of the specified LiveChannel.|
|GetLiveChannelHistory|Queries the ingestion history of the specified LiveChannel.|
|GetVodPlaylist|Queries the playlist generated during a specified period of time for the specified LiveChannel.|

