# API operation calling fees

Operations in OSS are implemented by calling OSS API operations. Fees are calculated based on the count of API operation calls.

OSS charges API operation calling fees based on the count of API operations you call to send PUT requests and GET requests. For more information about prices, see [Object Storage Service Pricing](https://www.alibabacloud.com/zh/product/oss/pricing).

|Billing item|Description|Billing method|
|------------|-----------|--------------|
|Count of PUT requests|Fees generated when you call OSS operations to send PUT requests. The counts of failed and successful requests are calculated.|-   Pay-as-you-go: Calling fees = Actual calls × Unit price per 10,000 calls/10000.

**Note:** You can call OSS API operations 50,000 times free of charge to perform operations on buckets located in the China \(Beijing\) and China \(Shenzhen\) regions. Additional calls incur fees.

-   Subscription: none. |
|Count of GET requests|Fees generated when you call OSS operations to send GET requests. The counts of failed and successful requests are calculated.|

**Note:** Operations in the OSS console are implemented by calling OSS API operations. For example, GetService \(ListBuckets\) is called when you view the list of buckets. GetBucket \(ListObjects\) is called when you access the Files page on the OSS console. Therefore, OSS also charges API operation calling fees for operations in the OSS console.

The following table lists the details of PUT and GET requests.

-   **PUT Request**

    |API request|Operation|
    |-----------|---------|
    |PutBucket|Creates a bucket.|
    |GetService\(ListBuckets\)|Lists all buckets.|
    |GetBucket\(ListObject\)|Lists all objects.|
    |PutBucketACL|Configures bucket ACL.|
    |PutBucketLogging|Configures bucket logging.|
    |PutBucketWebsite|Configures static page hosting.|
    |PutBucketReferer|Configures hotlink protection.|
    |PutBucketLifecycle|Configures lifecycle rules.|
    |DeleteBucket|Deletes a bucket.|
    |DeleteBucketLogging|Disables bucket logging.|
    |DeleteBucketWebsite|Deletes static website hosting configurations.|
    |DeleteBucketLifecycle|Deletes lifecycle rules.|
    |PutObject|Uploads an object.|
    |CopyObject|Copies an object.|
    |AppendObject|Appends an object.|
    |DeleteObject|Deletes an object.|
    |DeleteMultipleObjects|Deletes multiple objects at a time.|
    |PutObjectACL|Configures object ACL.|
    |PostObject|Uploads objects by using POST requests.|
    |PutSymlink|Creates a symbolic link.|
    |RestoreObject|Restores an Archive object.|
    |UploadPart|Uploads parts.|
    |AbortMultipartUpload|Cancels a multipart upload task.|
    |UploadPartCopy|Copies objects by part.|
    |DeleteBucketcors|Deletes cross-origin resource sharing \(CORS\) configurations for a bucket.|
    |PutBucketcors|Adds CORS configurations for a bucket.|
    |CompleteMultipartUpload|Completes a multipart upload task.|
    |InitiateBucketWorm|Creates a retention policy.|
    |AbortBucketWorm|Deletes an unlocked retention policy.|
    |CompleteBucketWorm|Locks a retention policy.|
    |ExtendBucketWorm|Extends the retention period \(days\) of objects in a bucket for which a retention policy is locked.|
    |PutBucketVersioning|Specifies the versioning status of a bucket.|
    |PutBucketPolicy|Configures bucket policies.|
    |DeleteBucketPolicy|Deletes bucket policies.|
    |PutBucketTags|Adds tags for a bucket or modifies the tags of a bucket.|
    |DeleteBucketTags|Deletes the tags of a bucket.|
    |PutBucketEncryption|Configures encryption rules for a bucket.|
    |DeleteBucketEncryption|Deletes encryption rules for a bucket.|
    |PutBucketRequestPayment|Configures the pay-by-requester mode for a bucket.|
    |PutObjectTagging|Configures or updates object tags.|
    |DeleteObjectTagging|Deletes the tags of a specified object.|
    |PutLiveChannel|Creates a LiveChannel.|
    |DeleteLiveChannel|Deletes a specified LiveChannel.|
    |PutLiveChannelStatus|Switches the status of a LiveChannel.|
    |PostVodPlaylist|Generates a playlist used for broadcasting for a LiveChannel.|

-   **Get Request**

    |API request|Operation|
    |-----------|---------|
    |GetBucketAcl|Queries the bucket ACL.|
    |GetBucketLocation|Queries the data center where a bucket is located.|
    |GetBucketInfo|Queries bucket information.|
    |GetBucketLogging|Queries bucket logging configurations.|
    |GetBucketWebsite|Queries the static website hosting configurations of a bucket.|
    |GetBucketReferer|Queries the hotlink protection configurations of a bucket.|
    |GetBucketLifecycle|Queries the lifecycle rules configured for a bucket.|
    |GetObject|Downloads an object.|
    |CopyObject|Copies an object.|
    |HeadObject|Queries object metadata.|
    |GetObjectMeta|Queries basic object metadata.|
    |GetObjectACL|Queries the object ACL.|
    |GetSymlink|Queries a symbolic link.|
    |ListMultipartUploads|Lists all ongoing multipart upload tasks.|
    |ListParts|Lists the uploaded parts.|
    |UploadPartCopy|Copies objects by part.|
    |GetBucketcors|Queries the CORS rules configured for a bucket.|
    |GetBucketWorm|Queries the retention policies configured for a bucket.|
    |GetBucketVersioning|Queries the versioning status of a specified bucket.|
    |GetBucketVersions\(ListObjectVersions\)|Displays the version information of all objects in a bucket.|
    |GetBucketPolicy|Queries bucket policy configurations.|
    |GetBucketReferer|Queries the hotlink protection configurations of a bucket.|
    |GetBucketTags|Queries the tags of a bucket.|
    |GetBucketEncryption|Queries the encryption rules of a bucket.|
    |GetBucketRequestPayment|Queries the pay-by-requester configurations of a bucket.|
    |SelectObject|Scans an object.|
    |GetObjectTagging|Queries the tags of an object.|
    |ListLiveChannel|Lists a specified LiveChannel.|
    |GetLiveChannelInfo|Queries the configurations of a specified LiveChannel.|
    |GetLiveChannelStat|Queries the ingestion status of a specified LiveChannel.|
    |GetLiveChannelHistory|Queries the ingestion history of a specified LiveChannel.|
    |GetVodPlaylist|Queries the playlist generated during a specified period of time for a specified LiveChannel.|


