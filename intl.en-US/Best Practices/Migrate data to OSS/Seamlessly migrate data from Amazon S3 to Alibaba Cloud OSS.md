# Seamlessly migrate data from Amazon S3 to Alibaba Cloud OSS

OSS is compatible with the S3 API to allow you seamlessly migrate data from S3 to OSS.

After data is migrated to OSS, you can still use the S3 API to access OSS. You need only to configure your S3 client application by performing the following operations:

1.  Obtain the AccessKey ID and AccessKey secret of your OSS account and RAM user, and configure the obtained AccessKey ID and AccessKey secret in the client and SDKs that you use.
2.  Set the endpoint for client connection to an OSS endpoint. For more information about OSS endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## Migration tutorials

You can use [Alibaba Cloud Data Online Migration](https://mgw.console.aliyun.com/job?_k=r90z7u#/job?_k=xdgp8r) to migrate data from AWS S3 to Alibaba Cloud OSS. For more information, see [Background information]().

## Use S3 API operations to access OSS after migration

When you use S3 API operations to access OSS after data is migrated from S3 to OSS, take note of the following items:

-   Path style and virtual hosted style

    [Virtual hosted style](http://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html) supports access to OSS by adding the bucket to the host header. For security reasons, OSS supports only virtual hosted style access. Therefore, you must configure your application client after the migration from S3 to OSS. By default, some S3 tools use path style access, which also requires proper configurations. Otherwise, OSS may report errors and prohibit access.

-   Definition of ACL permissions

    Definition of ACL permissions in OSS are not the same as they are in S3. You can adjust the permissions after the migration. The following table describes the differences between OSS and S3.

    |Item|AWS S3 permission|AWS S3|Alibaba Cloud OSS|
    |:---|:----------------|:-----|:----------------|
    |Bucket|READ|You have the list permission on the bucket.|If no object permissions is set for an object when the object is in the bucket, you can read the object.|
    |WRITE|You can write or overwrite objects in the bucket.|    -   You can write objects that do not exist in the bucket.
    -   If no object permissions is set for an object when the object exists in the bucket, you can overwrite the object.
    -   You can use InitiateMultipartUpload to upload objects. |
    |READ\_ACP|You can read bucket ACLs.|Only the bucket owner and RAM user have permissions to read bucket ACLs.|
    |WRITE\_ACP|You can configure ACLs for buckets.|Only the bucket owner and RAM user have permissions to configure bucket ACLs.|
    |Object|READ|You can read objects.|You can read objects.|
    |WRITE|N/A|You can overwrite objects.|
    |READ\_ACP|You can read object ACLs.|Only the bucket owner and RAM user have permissions to read bucket ACLs.|
    |WRITE\_ACP|You can configure object ACLs.|Only the bucket owner and RAM user have permissions to configure bucket ACLs.|

    **Note:**

    -   For more information about the differences, see [OSS request process](/intl.en-US/Developer Guide/Data security/Signature/OSS request process.md).
    -   OSS supports only three ACL modes in S3: private, public-read, and public read/write.
-   Storage class

    OSS supports three storage classes: Standard, Infrequent Access \(IA\), and Archive, which correspond to STANDARD, STANDARD\_IA, and GLACIER in AWSmazon S3. You can convert the storage class of OSS objects.

    To read an Archive object in OSS, you must use the Restore request to restore it. Different with S3, OSS ignores the lifetime to restore objects configured in the S3 API. By default, the restored state lasts for one day, and can be prolonged to seven days at most. Then, the object enters the frozen state again.

-   ETag
    -   If objects are uploaded by using the PUT method, the ETag of an OSS object and that of an Amazon S3 object differ in case sensitivity. The ETag is in uppercase for an OSS object and in lowercase for an S3 object. If your client has ETag validation, ignore the case sensitivity.
    -   If objects are uploaded by using the multipart upload method, OSS uses an ETag calculation method different from that used by S3.

## Compatible S3 operations

-   Bucket operations:
    -   Delete Bucket
    -   Get Bucket \(list objects\)
    -   Get Bucket ACL
    -   Get Bucket lifecycle
    -   Get Bucket location
    -   Get Bucket logging
    -   Head Bucket
    -   Put Bucket
    -   Put Bucket ACL
    -   Put Bucket lifecycle
    -   Put Bucket logging
-   Object operations:
    -   Delete Object
    -   Delete Objects
    -   Get Object
    -   Get Object ACL
    -   Head Object
    -   Post Object
    -   Put Object
    -   Put Object Copy
    -   Put Object ACL
-   Multipart operations:
    -   Abort Multipart Upload
    -   Complete Multipart Upload
    -   Initiate Multipart Upload
    -   List Parts
    -   Upload Part
    -   Upload Part Copy

