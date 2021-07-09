# Rename

You can call this operation to rename a directory or an object. This operation supports only buckets for which the hierarchical namespace feature is enabled.

## Usage notes

When you call the Rename operation to rename the source directory or object to the destination directory or object, take note of the following items:

-   You must have the DeleteObject permission on the source directory or object and the PutObject permission on the destination directory or object.
-   The parent directory included in the names of the source directory or object as well as the destination directory or object must exist.
-   The name that you use to rename the source directory or object cannot be the same as that of an existing directory or object in the parent directory included in the name of the destination directory or object.

## Request structure

```
POST /dstObjectName?x-oss-rename HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-rename-source:srcPathName
```

## Request headers

|Header|Type|Required|Description|
|------|----|--------|-----------|
|x-oss-rename-source|String|Yes|The absolute path of the source directory or object. Example: desktop/oss/a. The path must exist in the source bucket.|

For more information about the common request headers contained in Rename requests such as Host and Date, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a Rename request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

Sample requests

The following sample request is sent to rename an object named a in the desktop/osstest/ directory to b.

```
POST /desktop/osstest/b?x-oss-rename HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 29 Apr 2021 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
x-oss-rename-source: desktop/oss/a
```

Sample responses

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Thu, 29 Apr 2021 05:21:12 GMT
Connection: keep-alive
Server: AliyunOSS
```

## SDK

[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Manage directories.md)

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|403|Possible causes:-   You do not have permissions to access the bucket specified in the request.
-   You do not have permissions to access the directory or object that you want to rename. |
|NoSuchKey|404|Possible causes:-   The source directory or object you want to rename does not exist.
-   The parent directory included in the name of the source directory or object does not exist. |
|FileAlreadyExists|409|The error message returned because the name that you use to rename the source directory or object is the same as that of an existing directory or object.|

