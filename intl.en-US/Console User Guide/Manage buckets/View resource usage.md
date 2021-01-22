# View resource usage

This topic describes how to view OSS resource usage in the OSS console.

You can view the usage of the following resources in the OSS console:

-   **Basic Statistics**: includes **Buckets**, **Traffic Usage**, and **Requests per Hour**.
-   **Ranking Statistic**: includes **PV/UV** and **Rankings**.
-   **Region and Operator Statistics**: includes **Source Distribution** and **ISP**.
-   **API Statistics**: includes **HTTP Method Statistics** and **Status Codes**.
-   **Object Access Statistics**: includes the statistics for object access.

**Note:** **Ranking Statistic**, **Region and Operator Statistics**, **API Statistics**, and **Object Access Statistics** are supported only in regions in China, US \(Virginia\), US \(Silicon Valley\), and Singapore.

This topic uses **Basic Statistics** as an example to show you how to view resource usage.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Data Statistics** \> **Basic Statistics**.

    On the Basic Statistics tab, charts of **Buckets**, **Traffic Usage**, and **Requests per Hour** are displayed.

    -   **Buckets**

        Records the storage usage of the bucket for each period.

        |Basic data item|Description|
        |---------------|-----------|
        |Standard-Actual Storage Usage|The total storage of the Standard bucket.|
        |Storage Usage|The total storage of the bucket.|
        |Billed Storage Usage|The billed storage of the bucket.|

    -   **Traffic Usage**

        Records the traffic usage of the bucket for each period.

        |Basic data item|Description|
        |---------------|-----------|
        |CDN Inbound|The traffic that is generated when you use CDN to upload data to OSS.|
        |CDN Outbound|The traffic that is generated when you use CDN to download or browse data from OSS.|
        |Internet Inbound|The traffic that is generated when you upload data to OSS over the Internet.|
        |Internet Outbound|The traffic that is generated when OSS data is accessed or downloaded over the Internet.|
        |Intranet Inbound|The traffic that is generated when you upload data to OSS over the internal network.|
        |Intranet Outbound|The traffic that is generated when OSS data is accessed or downloaded over the internal network.|
        |Cross-region Replication|The traffic that is generated when you use cross-region replication \(CRR\) to copy data from the source bucket to the destination bucket.|

    -   **Requests per Hour**

        Records the number of requests initiated per hour to access the bucket. The statistics for **5XX Error**, **PUT Request**, and **GET Request** are collected.

        -   **5XX Error**

            Records statistics for 5xx responses returned when the client accesses OSS. Examples of 5xx HTTP status codes: 501, 502, and 503.

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

4.  Configure a time range to view resource usage.

5.  View statistics related to the basic data items based on different charts.

    The storage usage chart used in this example describes how to view statistics related to the basic data items.

    -   The display statuses of basic data items are shown in the lower-right corner of the chart. If a filled circle is displayed in front of a basic data item, related statistics are displayed in the chart. If an empty circle is displayed in front of a basic data item, the related statistics are not displayed in the chart.

        **Note:** By default, statistics for all basic data items are displayed.

    -   To specify whether to show statistics for a basic data item, click the circle in front of the basic data item.
    -   To specify whether to show statistics only for a basic data item or all basic data items, double-click the circle in front of the basic data item.

