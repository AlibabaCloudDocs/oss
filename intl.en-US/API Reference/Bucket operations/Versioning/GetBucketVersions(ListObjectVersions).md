# GetBucketVersions\(ListObjectVersions\)

You can call this operation to list the versions of all objects and delete markers in a bucket.

## Usage notes

When you perform the GetBucketVersions\(ListObjectVersions\) and GetBucket\(ListObjects\) operations on a versioned bucket, the following different results are returned:

-   For the GetBucket\(ListObjects\) operation, only the current versions of objects but not delete markers in the bucket are returned.
-   For the GetBucketVersions\(ListObjectVersions\) operation, all versions of all objects in the bucket are returned.

    Different objects are returned in alphabetic order. The versions of each object are returned in the order in which they were created but not in the alphabetic order of their version IDs.


## Request headers

An GetBucketVersions\(ListObjectVersions\) request contains only common request headers, such as `Authorization` and `Host`. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request parameters

When you call the GetBucketVersions\(ListObjectVersions\) operation, you can specify the following parameters to filter the returned results: prefix, key-marker, version-id-marker, delimiter, and max-keys.

|Parameter|Type|Required|Example|Description|
|:--------|:---|:-------|-------|:----------|
|delimiter|String|No|/|The character used to group objects by name. If you specify the delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.If you set prefix to a folder name and delimiter to a forward slash \(/\), only objects in the folder are returned. The names of the subfolders within the folder are returned in CommonPrefixes. However, the objects and folders within the subfolders are not returned.

Default value: null |
|key-marker|String|The key-marker parameter must be specified if version-id-marker is specified.|example|If this parameter is specified, objects whose names are alphabetically greater than the value of key-marker are returned in alphabetic order. This parameter is specified together with version-id-marker.The value of key-marker must be smaller than 1,024 bytes in length.

Default value: null |
|version-id-marker|String|No|CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1\*\*\*\*|If this parameter is specified, versions of the key-marker object created after the version specified by version-id-marker are returned in an order of created time. By default, if this parameter is not specified, the results are returned from the latest version of the object whose name follows the object specified by key-marker in alphabetic order. Default value: null

Valid values: version IDs |
|max-keys|String|No|100|The maximum number of objects to return.If the number of returned objects exceeds the value of max-keys, the response contains the `NextKeyMarker` and `NextVersionIdMarker` elements as the markers for the next list operation. Objects specified by `NextKeyMarker` and `NextVersionIdMarker` are included in the returned result.

Valid values: 1 to 1000

Default value: 100 |
|prefix|String|No|fun|The prefix that the returned object names must contain.-   The value of prefix must be smaller than 1,024 bytes in length.
-   If you specify a prefix in the request, the names of the returned objects contain the prefix.

If you set prefix to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders within the folder.

Default value: null |
|encoding-type|String|No|URL|The encoding type of the object name in the response.Default value: null

Valid value: URL

**Note:** The values of delimiter, marker, prefix, NextMarker, and Key must be UTF-8 encoded. If delimiter, marker, prefix, NextMarker, and Key contain control characters that are not supported by the XML 1.0 standard, you can specify encoding-type to encode Delimiter, Marker, Prefix, NextMarker, and Key in the response. |

## Response elements

|Element|Type|Example|Description|
|:------|:---|-------|:----------|
|ListVersionsResult|Container|N/A|The container used to store the results of the GetBucketVersions request.Child nodes: Name, Prefix, Marker, MaxKeys, Delimiter, IsTruncated, NextMarker, Version, and DeleteMarker

Parent node: none |
|CommonPrefixes|String|N/A|If the delimiter parameter is specified in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.Parent node: ListVersionsResult |
|Delimiter|String|/|The character used to group objects by name. If you specify the delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes.Parent node: ListVersionsResult |
|EncodingType|String|URL|The encoding type of the object names in the response. If you specify encoding-type in the request, Delimiter, Marker, Prefix, NextMarker, and Key are encoded in the returned results.Parent node: ListVersionsResult |
|IsTruncated|String|true|Indicates whether the returned results are truncated. -   true: indicates that not all results are returned this time.
-   false: indicates that all results are returned this time.

Valid values: true and false

Parent node: ListVersionsResult |
|KeyMarker|String|example|Indicates the object from which the GetBucketVersions operation starts.Parent node: ListVersionsResult |
|VersionIdMarker|String|CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1\*\*\*\*|This parameter is returned with KeyMarker together to indicate the version from which the GetBucketVersions operation starts.Parent node: ListVersionsResult |
|NextKeyMarker|String|test|If not all results are returned for the request, the NextUploadMarker element is contained in the response to indicate the key-marker of the next request.Parent node: ListVersionsResult |
|NextVersionIdMarker|String|CAEQGBiBgIC\_jq7P9xYiIDRiZWJkNjY2Y2Q4NDQ5ZTI5ZGE5ODIxMTIyZThl\*\*\*\*|If not all results are returned for the request, the NextUploadMarker element is contained in the response to indicate the version-id-marker of the next request.Parent node: ListVersionsResult |
|MaxKeys|String|1000|The maximum number of returned objects in the response.Parent node: ListVersionsResult |
|Name|String|examplebucket-1250000000|The bucket name.Parent node: ListVersionsResult |
|Owner|Container|N/A|The container that stores the information about the bucket owner.Parent node: ListVersionsResult |
|Prefix|String|fun|The prefix that the names of returned objects must contain.Parent node: ListVersionsResult |
|Version|Container|N/A|The container that stores the versions of objects except for delete markers.Parent node: ListVersionsResult |
|DeleteMarker|Container|N/A|The container that stores delete markers.Parent node: ListVersionsResult |
|ETag|String|250F8A0AE989679A22926A875F0A2\*\*\*\*|The entity tag \(ETag\). An ETag is generated when an object is created to identify the content of the object. -   If an object is created by using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created by using other methods, the ETag value is the UUID of the object content.

**Note:** The ETag value of an object can be used to check whether the object content changes. However, we recommend that you do not use the ETag value of an object as the MD5 hash of the object content to verify data integrity.

Parent node: ListVersionsResult.Version |
|Key|String|example|The name of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|LastModified|Time|2019-04-09T07:27:28.000Z|The last modified time of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|VersionId|String|CAEQMxiBgMDNoP2D0BYiIDE3MWUxNzgxZDQxNTRiODI5OGYwZGMwNGY3MzZjN\*\*\*\*|The version ID of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|IsLatest|String|true|Indicates whether the version is the current version.Valid values:

-   true: the version is the current version.
-   false: the version is a previous version.

Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|Size|String|93731|The size of the returned object. Unit: bytes.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|StorageClass|String|Standard|The storage class of the object.Parent nodes: ListVersionsResult.Version and ListVersionsResult.DeleteMarker |
|DisplayName|String|12345125285864390|The name of the object owner.Parent nodes: ListVersionsResult.Version.Owner and ListVersionsResult.DeleteMarker.Owner |
|ID|String|1234512528586\*\*\*\*|The user ID of the bucket owner.Parent nodes: ListVersionsResult.Version.Owner and ListVersionsResult.DeleteMarker.Owner |

For more information about the common response headers included in the response to a GetBucketVersions request, such as `x-oss-request-id` and `Content-Type`, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Perform GetBucketVersions\(ListObjectVersions\) on an unversioned bucket

    Sample requests

    ```
    GET /? versions HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76ov9cu:WFx4****+e7Rc0jawCsh7hlk****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Content-Type: application/xml
    Content-Length: 1262
    Connection: keep-alive
    Date: Thu, Tue, 09 Apr 2019 07:27:48 GMT
    Server: AliyunOSS
    x-cos-request-id: 534B371674E88A4D8906****
    
    <ListVersionsResult>
        <Name>examplebucket-1250000000</Name>
        <Prefix/>
        <KeyMarker/>
        <VersionIdMarker/>
        <MaxKeys>1000</MaxKeys>
        <IsTruncated>false</IsTruncated>
        <Version>
            <Key>example-object-1.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-5T12:03:10.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B669CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-2.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-9T12:03:09.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B002CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-3.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-10T12:03:08.000Z</LastModified>
            <ETag>4B3F1A2E053D763E1B002CC607C5AGTRF****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```

-   Perform GetBucketVersions\(ListObjectVersions\) on a versioned bucket

    In this example, two objects named example and pic.jpg are stored in a bucket named oss example. The object named example has the following three versions in the order of the created time: 111222, 000123, and 222333, in which 000123 is a delete marker. The object named pic.jpg has only one version whose ID is 232323.

    If you set key-marker to example and version-id-marker to 111222, the following three versions are returned in sequence: 000123 of example, 222333 of example, and 232323 of pic.jpg.

    Sample requests

    ```
    GET /? versions&key-marker=example&version-id-marker=CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1**** HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76o****:WFx4kLpx+e7Rc0jawCsh7hlk****
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC4974B7AEADE01700****
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListVersionsResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <KeyMarker>example</KeyMarker>
        <VersionIdMarker>CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1****</VersionIdMarker>
        <MaxKeys>100</MaxKeys>
        <Delimiter></Delimiter>
        <IsTruncated>false</IsTruncated>
        <DeleteMarker>
            <Key>example</Key>
            <VersionId>CAEQMxiBgICAof2D0BYiIDJhMGE3N2M1YTI1NDQzOGY5NTkyNTI3MGYyMzJm****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </DeleteMarker>
        <Version>
            <Key>example</Key>
            <VersionId>CAEQMxiBgMDNoP2D0BYiIDE3MWUxNzgxZDQxNTRiODI5OGYwZGMwNGY3MzZjN****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"250F8A0AE989679A22926A875F0A2****"</ETag>
            <Type>Normal</Type>
            <Size>93731</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>pic.jpg</Key>
            <VersionId>CAEQMxiBgMCZov2D0BYiIDY4MDllOTc2YmY5MjQxMzdiOGI3OTlhNTU0ODIx****</VersionId>
            <IsLatest>true</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"3663F7B0B9D3153F884C821E7CF4****"</ETag>
            <Type>Normal</Type>
            <Size>574768</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetBucketVersions operation:

-   [Java](/intl.en-US/SDK Reference/Java/Versioning/Versioning.md)
-   [Python](/intl.en-US/SDK Reference/Python/Versioning/Versioning.md)
-   [Go](/intl.en-US/SDK Reference/Go/Versioning/Versioning.md)
-   [C++](/intl.en-US/SDK Reference/C++/Versioning/Versioning.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Versioning/Versioning.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Configure versioning/Versioning.md)

## Errors codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the bucket that you attempt to access does not exist. This error message also returned when the bucket that you access cannot be created because the bucket name does not comply with the naming conventions.|
|AccessDenied|403|The error message returned because you are not authorized to access the bucket.|
|InvalidArgument|400|-   The error message returned because the value of max-keys in the request is smaller than 0 or greater than 1000.
-   The error message returned because the length of the value of prefix, key-marker, or delimiter is invalid.
-   The error message returned because the value of version-id-marker is invalid. |

