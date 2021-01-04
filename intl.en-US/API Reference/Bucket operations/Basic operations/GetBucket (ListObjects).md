# GetBucket \(ListObjects\)

You can call this operation to query the information about all objects in a bucket.

## Usage notes

When you call the GetBucket \(ListObjects\) operation, take note of the following items:

-   The GetBucket \(ListObjects\) operation has been updated to GetBucketV2 \(ListObjectsV2\). We recommend that you use GetBucketV2 \(ListObjectsV2\) when you develop your applications. To provide backward compatibility, OSS continues to support the GetBucket \(ListObjects\) operation. For more information about GetBucketV2 \(ListObjectsV2\), see [GetBucketV2 \(ListObjectsV2\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md).
-   The user metadata of objects is not returned for the GetBucket \(ListObjects\) request.

## Request structure

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

A GetBucket \(ListObjects\) request contains only common request headers such as `Authorization` and `Host`. For more information, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request parameters

|Parameter|Type|Required|Example|Description|
|:--------|:---|:-------|-------|:----------|
|delimiter|String|No|/|The character used to group objects by name. If you specify the delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Default value: null |
|marker|String|No|test1.txt|The name of the object from which the list operation begins. If this parameter is specified, objects whose names are alphabetically greater than the marker parameter value are returned. The marker parameter is used to list the returned objects by page, and its value must be smaller than 1,024 bytes in length.

Even if the specified marker does not exist in the list during a conditional query, the list starts from the object whose name is alphabetically greater than the marker parameter value.

Default value: null |
|max-keys|String|No|200|The maximum number of objects that can be returned for a list operation. If the objects cannot be completely listed at one time because max-keys is specified, a NextMarker element is included in the response as the marker for the next list operation.Valid values: 1 to 1000

Default value: 100 |
|prefix|String|No|fun|The prefix that the returned object names must contain. -   The value of prefix must be smaller than 1,024 bytes in length.
-   If you specify a prefix to query objects, the names of the returned objects still contain the prefix.

If prefix is set to a folder name in the request, the objects whose names contain this prefix are listed, including all objects and subfolders in the folder.

If prefix is specified and delimiter is set to a forward slash \(/\), only the objects in the folder are listed. The subfolders are grouped together as a single result in CommonPrefixes. Objects and folders in the subfolders are not listed.

For example, a bucket contains the following objects: fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. If prefix is set to fun/, the three objects are all returned. If prefix is set to fun/ and delimiter is set to a forward slash \(/\), fun/test.jpg and fun/movie/are returned.

Default value: null |
|encoding-type|String|No|URL|The encoding type of the object name in the response. Default value: null

Valid value: URL

**Note:** The delimiter, marker, prefix, NextMarker, and Key values are UTF-8 encoded. If delimiter, marker, prefix, NextMarker, and Key contain control characters that are not supported by the XML 1.0 standard, you can specify encoding-type to encode Delimiter, Marker, Prefix, NextMarker, and Key in the response. |

## Response elements

|Element|Type|Example|Description|
|-------|----|-------|-----------|
|Contents|Container|N/A|The container that stores the returned object metadata. Parent node: ListBucketResult |
|CommonPrefixes|String|N/A|If the delimiter parameter is specified in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent node: ListBucketResult |
|Delimiter|String|/|The character used to group objects by name. If you specify the delimiter parameter in the request, the response contains the CommonPrefixes element. Objects whose names contain the same string that ranges from the prefix to the next occurrence of the delimiter are grouped as a single result element in CommonPrefixes. Parent node: ListBucketResult |
|EncodingType|String|N/A|The encoding type of the object name in the response. In addition, if you specify the encoding-type parameter, the following elements in the response are encoded: Delimiter, Marker, Prefix, NextMarker, and Key. Parent node: ListBucketResult |
|DisplayName|String|user\_example|The name of the object owner. Parent node: ListBucketResult.Contents.Owner |
|ETag|String|5B3C1A2E053D763E1B002CC607C5A0FE1\*\*\*\*|The entity tag \(ETag\). An ETag is created when an object is created to identify the content of the object. -   If an object is created by using a PutObject request, the ETag value is the MD5 hash of the object content.
-   If an object is created using other methods, the ETag value is the UUID of the object content.
-   The ETag value of an object can be used to check whether the object content is changed. However, we recommend that you do not use the ETag of an object as the MD5 hash of the object to verify data integrity.

Parent node: ListBucketResult.Contents |
|ID|String|0022012\*\*\*\*|The user ID of the bucket owner. Parent node: ListBucketResult.Contents.Owner |
|IsTruncated|Enumerated string|false|Indicates whether the returned results are truncated. Valid values: true and false

-   true: indicates that not all of the results are returned this time.
-   false: indicates that all of the results are returned this time.

Parent node: ListBucketResult |
|Key|String|fun/test.jpg|The key of the object. Parent node: ListBucketResult.Contents |
|LastModified|Time|2012-02-24T08:42:32.000Z|The last modified time of the object. Parent node: ListBucketResult.Contents |
|ListBucketResult|Container|N/A|The container that stores the result of the GetBucket request. Child nodes: Name, Prefix, Marker, MaxKeys, Delimiter, IsTruncated, NextMarker, and Contents

Parent node: none |
|Marker|String|test1.txt|The name of the object from which the list operation begins. Parent node: ListBucketResult |
|MaxKeys|String|100|The maximum number of returned objects in the response. Parent node: ListBucketResult |
|Name|String|oss-example|The bucket name. Parent node: ListBucketResult |
|Owner|Container|N/A|The container that stores the information about the bucket owner. Child nodes: DisplayName and ID

Parent node: ListBucketResult |
|Prefix|String|fun/|The prefix that the names of returned objects contain. Parent node: ListBucketResult |
|Size|String|344606|The size of the returned object. Unit: bytes. Parent node: ListBucketResult.Contents |
|StorageClass|String|Standard|The storage class of the object. Parent node: ListBucketResult.Contents |

For more information about other common response headers included in the response, such as `x-oss-request-id` and `Content-Type`, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample request for the simple GetBucket operation

    ```
    GET / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>oss.jpg</Key>
          <LastModified>2012-02-24T06:07:48.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   Sample request in which the prefix parameter is specified

    ```
    GET /? prefix=fun HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix>fun</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   Sample request in which the prefix and delimiter parameters are specified

    ```
    GET /? prefix=fun/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    Sample response

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Name>oss-example</Name>
    <Prefix>fun/</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <CommonPrefixes>
          <Prefix>fun/movie/</Prefix>
    </CommonPrefixes>
    </ListBucketResult>
    ```

-   Sample request in which the marker parameter is specified

    In this example, the value of max-keys is set to 2, which indicates that up to two objects can be returned for a list operation.

    ```
    GET /? max-keys=2&marker=test1.txt HTTP/1.1
    Host: test-bucket-xxx.oss-cn-shenzhen.aliyuncs.com
    Accept-Encoding: identity
    Accept: */*
    Connection: keep-alive
    User-Agent: aliyun-sdk-python/2.11.0(Darwin/18.2.0/x86_64;3.4.1)
    date: Tue, 26 May 2020 08:39:48 GMT
    authorization: OSS LTAIB1VW9xxxxxxx:MmY11jLlO8UlAqjqHK3Ckp****
    ```

    Sample response

    The value of NextMarker in the response is the marker for next list operation.

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 26 May 2020 08:39:48 GMT
    Content-Type: application/xml
    Content-Length: 1032
    Connection: keep-alive
    x-oss-request-id: 5ECCD5D4881816373582xxx
    x-oss-server-time: 3
    
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>test-bucket-xxx</Name>
      <Prefix></Prefix>
      <Marker>test1.txt</Marker>
      <MaxKeys>2</MaxKeys>
      <Delimiter></Delimiter>
      <EncodingType>url</EncodingType>
      <IsTruncated>true</IsTruncated>
      <NextMarker>test100.txt</NextMarker>
      <Contents>
        <Key>test10.txt</Key>
        <LastModified>2020-05-26T07:50:18.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
      <Contents>
        <Key>test100.txt</Key>
        <LastModified>2020-05-26T07:50:20.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
    </ListBucketResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the GetBucket \(ListObjects\) operation:

-   [Java](/intl.en-US/SDK Reference/Java/Manage objects/List objects.md)
-   [Python](/intl.en-US/SDK Reference/Python/Manage objects/List objects.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Manage objects/List objects.md)
-   [Go](/intl.en-US/SDK Reference/Go/Manage objects/List objects.md)
-   [C](/intl.en-US/SDK Reference/C/Manage objects/List objects.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Manage objects/List objects.md)
-   [Android](/intl.en-US/SDK Reference/Android/Manage objects/List objects.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Overview.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Manage objects.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Manage objects.md)

## Errors codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to access the bucket.|
|InvalidArgument|400|-   The error message returned because the value of max-keys is smaller than 0 or greater than 1000.
-   The error message returned because the length of the value of prefix, marker, or delimiter is invalid. |

