# HTTP 404 status code

This topic describes the types of error messages returned with HTTP status code 404, and the common causes and solutions to these errors.

## KeyNotFound

-   Error message: The specified parameter KMS keyId is not found.
-   Cause: The specified CMK is not found.
-   Solution: Check whether KMS is activated and specify a valid CMK ID in the `9468da86-3509-4f8d-a61e-6eab1eac***` format. For more information, see [Configure server-side encryption](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure server-side encryption.md).

## AliasNotFound

-   Error message: The specified Alias is not found.
-   Cause: The specified CMK alias is not found.
-   Solution: Check whether KMS is activated and specify a valid CMK alias. The alias of a CMK must start with alias. Example: `alias/example`.

## NoSuchServerSideEncryptionRule

-   Error message: The server side encryption configuration was not found.
-   Cause: Server-side encryption is not enabled for the bucket.
-   Solution: Enable server-side encryption for the bucket. For more information, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## NoSuchWebsiteConfiguration

-   Error message: The specified bucket does not have a website configuration.
-   Cause: Static website hosting is not configured for the bucket.
-   Solution: Configure static website hosting for the bucket. For more information, see [Static website hosting](/intl.en-US/Developer Guide/Static website hosting/Static website hosting.md).

## NoSuchBucketObjectTagging

-   Error message: The specified bucket does not have a object tagging.
-   Cause: None of the objects in the bucket have tags.
-   Solution: Add tags to the objects in the bucket. A tag is a key-value pair used to identify an object. Object tags must comply with the following rules:
    -   You can add up to 10 tags to an object. The tags added to an object must have unique keys.
    -   A tag key can be up to 128 bytes in length. A tag value can be up to 256 bytes in length.
    -   Tag keys and tag values are case-sensitive.
    -   The key and value of a tag can contain letters, digits, spaces, and the following special characters:

        + - =.\_:/

        If the tags of the HTTP header contain the preceding characters, you must perform URL encoding on the keys and values of the tags.


## NoSuchCORSConfiguration

-   Error message: The CORS Configuration does not exist.
-   Cause: No cross-origin resource sharing \(CORS\) rules are not configured for the bucket.
-   Solution: Configure a CORS rule for the bucket to allow or deny specific cross-region requests. For more information, see [Configure CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md).

## NoSuchWORMConfiguration

-   Error message: The WORM Configuration does not exist.

    Cause: No retention policies are configured for the bucket.

    Solution: Configure a retention policy for the bucket to protect your data from being deleted or modified. For more information, see [Configure retention policies](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure retention policies.md).

-   Error message: The specified WORM ID does not exist.

    Cause: The specified retention policy ID does not exist.

    Solution: Specify a valid policy ID when you want to lock a retention policy or extend the retention period of the retention policy. You can call the [GetBucketWorm](/intl.en-US/API Reference/Bucket operations/Retention policy/GetBucketWorm.md) operation to obtain the ID of a retention policy.


## SymlinkTargetNotExist

-   Error message: The symlink target object does not exist.
-   Cause:
    -   The object name is invalid.
    -   The object to which the symbolic link points does not exist.
-   Solution:
    -   Check the object name and make sure the name complies with naming conventions.

        Object names must comply with the following conventions:

        -   The name cannot start with a forward slash \(/\) or a backslash \(\\\).
        -   The name can contain only UTF-8 characters.
        -   The name must be 1 to 1,023 bytes in length.
    -   If the object specified in the request is a symbolic link, ensure that the object to which the symbolic link points exists.

## NoSuchUser

-   Error message: User not found.
-   Cause: The specified user does not exist.
-   Solution: Check whether the Alibaba Cloud account that you use is canceled.

## NoSuchRegion

-   Error message: NoSuchRegion
-   Cause: The specified region does not exist.
-   Solution: Check the information about the regions of OSS. For mroe information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## NoSuchLifecycle

-   Error message: No Row found in Lifecycle Table.
-   Cause: No lifecycle rules are configured for the bucket.
-   Solution: Configure lifecycle rules for the bucket. This reduces storage costs by deleting expired objects and parts or converting the storage classes of objects to IA, Archive, or Cold Archive to save storage costs. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).

## NoSuchInventory

-   Error message: No Row found in Inventory Table.
-   Cause: No inventories are configured for the bucket.
-   Solution: Configure inventories for the bucket to obtain the information about specific objects in the bucket, such as the numbers, sizes, storage classes, and encryption status of the objects. For more information, see [PutBucketInventory](/intl.en-US/API Reference/Bucket operations/Inventory/PutBucketInventory.md).

## NoSuchBucket

-   Error message: The specified bucket does not exist.
-   Cause: The bucket name does not comply with naming conventions.
-   Solution: Check the bucket name and make sure that the bucket name complies with naming conventions.

    Bucket names must comply with the following conventions:

    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length.

## NoSuchKey

-   Error message: The specified key does not exist.
-   Cause:
    -   The object name is invalid.
    -   The object is deleted based on lifecycle rules.
    -   The object is deleted by an authorized user by using the OSS console, OSS clients, or API operations.
    -   Cross-region replication \(CRR\) is configured for the bucket. A delete operation performed on an object that has the same name in the source bucket is synchronized to the specified object.
-   Solution:
    -   Check the object name and make sure that the name complies with naming conventions. The object name must be UTF-8 encoded and 1 to 1023 bytes in length. The object name cannot start with a forward slash \(/\) or backslashes \(\\\).
    -   Check the lifecycle rules configured for the bucket and make sure that the object is not deleted based on the rules. For more information, see [Configure lifecycle rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure lifecycle rules.md).
    -   Make sure that the object is not deleted by another authorized user .
    -   Check the cross-region replication \(CRR\) rules configured for the bucket and make sure that the object is not deleted based on the CRR rules. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

## NoSuchUpload

-   Error message: The specified upload does not exist. The upload ID may be invalid, or the upload may have been aborted or completed.
-   Cause:
    -   The request ID is not returned to the client, which indicates that the object failed to be uploaded.
    -   Only some parts but not the entire object is uploaded in multipart upload or resumable upload.
-   Solution:
    -   If this error message is returned when you access an object that you upload, check the returned results and make sure that the request ID is included in the response.
    -   If this error message is returned during multipart or resumable uploads, check the returned results and make sure that HTTP status code 200 and the request ID is returned for the CompleteMultipartUpload operation. For more information, see [InitiateMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/InitiateMultipartUpload.md).

## NoSuchVersion

-   Error message: The specified version does not exist.
-   Cause: The specified version of the object does not exist.
-   Solution: Specify a valid version ID when you list, download, or delete a specific version of an object. You can call the [GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md) operation to obtain the IDs of all versions of an object.

## NoSuchLiveChannel

-   Error message: The specified live channel does not exist.
-   Cause: The specified LiveChannel does not exist.
-   Solution: Create a LiveChannel to ingest streams to OSS and obtain the ingest URL. For more information, see [RTMP-based stream ingest](/intl.en-US/Developer Guide/Objects/Upload files/RTMP-based stream ingest.md).

## NoSuchBucketPolicy

-   Error message: The specified bucket policy does not exist.
-   Cause: No authorization policies are configured for the bucket.
-   Solution: Configure authorization policies for the bucket to authorize other users to access your OSS resources. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

## NoSuchReplicationConfiguration

-   Error message: The bucket you specified does not have replication configuration.
-   Cause: No cross-region replication \(CRR\) rules are configured for the bucket.
-   Solution: Configure CRR rules for the bucket to perform automatic and asynchronous \(near real-time\) replication of objects across buckets in different regions. After you configure CRR rules for a bucket, operations in the source bucket such as the creation, overwriting, and deletion of objects are synchronized to the destination bucket. This implements geo-disaster recovery and data replication. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

## NoSuchReplicationRule

-   Error message: The BucketReplicationRule you specified does not exist.
-   Cause: The specified cross-region replication \(CRR\) rule does not exist.
-   Solution: Specify a valid CRR rule ID when you query the progress of a CRR task of a bucket or delete a CRR rule configured for a bucket. You can call the [GetBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplication.md) operation to obtain the IDs of the CRR rules configured for a bucket.

## NoSuchTransferAccelerationConfiguration

-   Error message: The bucket you specified does not have transfer acceleration configuration.
-   Cause: Transfer acceleration is not enabled for the bucket.
-   Solution: Enable transfer acceleration when you accelerate remote data transfer, accelerate the upload and download of objects of gigabytes or terabytes in size, and accelerate the download of non-static and non-hotspot data. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## NoSuchChannel

-   Error message: No Such Image Channel.
-   Cause: The specified image channel does not exist.
-   Solution: Use the latest version of the image processing \(IMG\) feature. Earlier versions of IMG are no longer supported. For more information about the latest version of IMG, see [Overview](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## NoSuchStyle

-   Error message: No Such Image Style.
-   Cause: The specified image style does not exist.
-   Solution: Create an image style that includes multiple IMG parameters to perform complex operations on image objects in batches. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

## NoSuchCacheControlConfiguration

-   Error message: The bucket you specified does not have cache control configuration.
-   Cause: No cache control policies are configured for the bucket.
-   Solution: Specify the `cache-control` header in HTTP requests and responses to configure different cache control policies. The `cache-control` header is supported by the [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md), [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md), and [GetObject](/intl.en-US/API Reference/Object operations/Basic operations/GetObject.md) operations.

