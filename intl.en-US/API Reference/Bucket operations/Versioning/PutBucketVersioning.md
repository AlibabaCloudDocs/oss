# PutBucketVersioning

You can call this operation to configure the versioning state of a bucket.

## Usage notes

-   To configure versioning for a bucket, you must have the PutBucketVersioning permission.
-   A bucket can be in one of the following versioning state: disabled, enabled, and suspended. By default, versioning is disabled for a bucket.
-   If versioning is enabled for a bucket, an object that is added to the bucket have a unique version ID. In this case, multiple versions of an object can be stored in OSS at the same time.
-   If versioning is suspended for a bucket, the version ID of an object that is added to the bucket is null. OSS does not store new previous versions for objects that is deleted or overwritten.

For more information about versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## Request syntax

```
PUT /? versioning HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<VersioningConfiguration>
    <Status>Enabled</Status>
</VersioningConfiguration>
```

## Examples

-   Sample request for enabling versioning for a bucket

    ```
    PUT /? versioning HTTP/1.1
    Host: bucket-versioning.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:20:12 GMT
    Authorization: OSS e7thre3jj5mlvqk:12ztptkaR8a74gIGFzOaZZQe****
    <? xml version="1.0" encoding="UTF-8"? >
    <VersioningConfiguration>
        <Status>Enabled</Status>
    </VersioningConfiguration>
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC015CB7AEADE01700****
    Date: Tue, 09 Apr 2019 02:20:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Sample request for suspending versioning for a bucket

    ```
    PUT /? versioning HTTP/1.1
    Host: bucket-versioning.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:28:18 GMT
    Authorization: OSS m2qa99e9tpkaehr:DWAzr2EkqDwFJNke1Nuaogn7****
    <? xml version="1.0" encoding="UTF-8"? >
    <VersioningConfiguration>
        <Status>Suspended</Status>
    </VersioningConfiguration>
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC0342B7AEADE01700****
    Date: Tue, 09 Apr 2019 02:28:18 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use SDKs for the following programming languages to call the PutBucketVersioning operation:

-   [Java](/intl.en-US/SDK Reference/Java/Versioning/Versioning.md)
-   [Python](/intl.en-US/SDK Reference/Python/Versioning/Versioning.md)
-   [Go](/intl.en-US/SDK Reference/Go/Versioning/Versioning.md)
-   [C++](/intl.en-US/SDK Reference/C++/Versioning/Versioning.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Versioning/Versioning.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Versioning.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|403|This error message returned because you do not have permissions to configure the versioning state of the bucket.|
|InvalidArgument|400|This error message returned because the versioning state that you want to configure is invalid. You can set the versioning state of a bucket to only Enabled or Suspended.|

