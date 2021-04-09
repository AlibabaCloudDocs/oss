# CompleteMultipartUpload

You can call this operation to complete the multipart upload task of an object.

## Usage notes

When you call the CompleteMultipartUpload operation, you must provide a complete list of all valid parts, which includes the part number and ETag of each part. After OSS receives the list of parts, OSS verifies the validity of each part one by one. After all parts are validated, OSS combines these parts into a complete object.

-   Confirm the size of each part

    When you perform the CompleteMultipartUpload operation, OSS checks whether each part except the last part is larger than or equal to 100 KB in size and verifies the part number and ETag of each part that are provided in the part list. Therefore, when a part is uploaded, the client must record not only the part number of the part but also the ETag value of the part returned from the server.

-   Handle requests

    It may take a while for OSS to process the CompleteMultipartUpload request. If the client is disconnected from OSS during this period, OSS continues to process the request.

-   PartNumber

    OSS verifies the value of PartNumber when you call the CompleteMultipartUpload operation.

    The value of PartNumber ranges from 1 to 10000. The part numbers listed in the request do not have to be consecutive but must be sorted in ascending order. For example, if the part number of the first part is 1, the part number of the second part can be 5.

-   UploadId

    An object can be uploaded by multiple upload tasks that have independent upload IDs. When one upload task is completed, the corresponding upload ID becomes invalid and the upload IDs of other upload tasks are not affected.

-   The x-oss-server-side-encryption request header

    If the x-oss-server-side-encryption header is specified in an InitiateMultipartUpload request, this header is returned in the response to the CompleteMultipartUpload request. The value of the x-oss-server-side-encryption header in the response indicates the method used to encrypt the object on the OSS server.


## Versioning

You can call CompleteMultipartUpload to complete the multipart upload task of an object when versioning is enabled for the bucket to which the object is uploaded. In this case, OSS generates a unique version ID for the object, and returns the version ID as the x-oss-version-id header in the response.

## Request structure

```
POST /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: Signature
<CompleteMultipartUpload>
<Part>
<PartNumber>PartNumber</PartNumber>
<ETag>ETag</ETag>
</Part>
...
</CompleteMultipartUpload>
```

## Request parameters

You can configure the encoding-type parameter in a CompleteMultipartUpload request. OSS uses the specified encoding type to encode the object name in the response.

|Parameter|Type|Description|
|:--------|:---|:----------|
|encoding-type|String|The encoding type of the object name in the response. Only URL encoding is supported. The object name can contain characters that are encoded in UTF-8. However, the XML 1.0 standard cannot be used to parse control characters such as characters with an ASCII value from 0 to 10. You can configure this parameter to encode the object names in the response.

Default value: null

Valid value: url |

## Request headers

|Header|Type|Required|Description|
|------|----|--------|-----------|
|x-oss-forbid-overwrite|String|No|Specifies whether the object with the same object name is overwritten when you perform the CompleteMultipartUpload operation. -   If the value of x-oss-forbid-overwrite is not specified or set to false, existing objects can be overwritten by objects that have the same names.
-   If the value of x-oss-forbid-overwrite is set to true, existing objects cannot be overwritten by objects that have the same names.

**Note:**

-   The x-oss-forbid-overwrite request header is invalid when the versioning is enabled or suspended for the bucket. In this case, the CompleteMultipartUpload operation overwrites the object that has the same name.
-   If you specify the x-oss-forbid-overwrite request header, the query per second \(QPS\) performance of OSS may be degraded. If you want to use the x-oss-forbid-overwrite request header for a large number of operations \(QPS greater than 1,000\), submit a ticket. |
|x-oss-complete-all:yes|String|No|Specifies whether to list all parts uploaded by using the current upload ID. -   If x-oss-complete-all:yes is specified in the request, OSS lists all parts that are uploaded by using the current upload ID, sorts the parts by part numbers, and then performs the CompleteMultipartUpload operation. OSS cannot detect parts that are not uploaded or being uploaded in the CompleteMultipartUpload operation. Before you perform the CompleteMultipartUpload operation, make sure that all parts are uploaded.
-   If x-oss-complete-all:yes is specified in the request, the request body cannot be specified. Otherwise, an error occurs.
-   The format of the response remains unchanged if x-oss-complete-all:yes is specified in the request. |

For more information about other common request headers included in CompleteMultipartUpload requests such as Host and Date, see [Common HTTP headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Request elements

|Element|Type|Description|
|:------|:---|:----------|
|CompleteMultipartUpload|Container|The container in which the content of the CompleteMultipartUpload request is stored. Child nodes: Part

Parent nodes: none |
|ETag|String|The ETag values that are returned by OSS after parts are uploaded. Parent nodes: Part |
|Part|Container|The container that stores the uploaded parts. Child nodes: ETag and PartNumber

Parent nodes: CompleteMultipartUpload |
|PartNumber|Integer|The number of parts. Parent nodes: Part |

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|Bucket|String|The bucket name. Parent nodes: CompleteMultipartUploadResult |
|CompleteMultipartUploadResult|Container|The container in which the result of the CompleteMultipartUpload request is stored. Child nodes: Bucket, Key, ETag, and Location

Parent nodes: none |
|ETag|String|The ETag that is generated when an object is created. ETags are used to identify the content of objects. The ETag value of an object created by CompleteMultipartUpload is the UUID of the object.

**Note:** The ETag value of an object can be used to check whether the object content is changed. To verify data integrity, we recommend that you do not use the ETag of an object as the MD5 hash of the object.

Parent nodes: CompleteMultipartUploadResult |
|Location|String|The URL used to access the uploaded object. Parent nodes: CompleteMultipartUploadResult |
|Key|String|The name of the uploaded object. Parent nodes: CompleteMultipartUploadResult |
|EncodingType|String|The encoding type of the object name in the response. If this parameter is specified in the request, the object name is encoded in the response. Parent nodes: InitiateMultipartUploadResult |

## Examples

-   Complete the multipart upload task of an object in an unversioned bucket.

    Sample requests

    ```
    POST /multipart.data?uploadId=0004B9B2D2F7815C432C9057C031****&encoding-type=url HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 1056
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:8VwFhFUWmVecK6jQlHlXMK/z****
    <CompleteMultipartUpload> 
        <Part> 
            <PartNumber>1</PartNumber>  
            <ETag>"3349DC700140D7F86A0784842780****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>5</PartNumber>  
            <ETag>"8EFDA8BE206636A695359836FE0A****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>8</PartNumber>  
            <ETag>"8C315065167132444177411FDA14****"</ETag> 
        </Part> 
    </CompleteMultipartUpload>
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Content-Length: 329
    Content-Type: Application/xml
    Connection: keep-alive
    x-oss-request-id: 594f0751-3b1e-168f-4501-4ac71d21****
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <CompleteMultipartUploadResult xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <EncodingType>url</EncodingType>
        <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com /multipart.data</Location>
        <Bucket>oss-example</Bucket>
        <Key>multipart.data</Key>
        <ETag>"B864DB6A936D376F9F8D3ED3BBE540****"</ETag>
    </CompleteMultipartUploadResult>
    ```

-   Complete the multipart upload task of an object in a versioned bucket.

    Sample requests

    ```
    POST /multipart.data?uploadId=63C06A5CFF6F4AE4A6BB3AD7F01C****  HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 223
    Date: Tue, 09 Apr 2019 07:01:56 GMT
    Authorization: OSS 6jftttm6x6s****:XljBrYBYxDnxKdFMj9WYI6qu****
    <CompleteMultipartUpload> 
        <Part> 
            <PartNumber>1</PartNumber>  
            <ETag>"25A9F4ABFCC05743DF6E2C886C56****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>5</PartNumber>  
            <ETag>"25A9F4ABFCC05743DF6E2C886C56****"</ETag> 
        </Part>  
    </CompleteMultipartUpload>
    ```

    Sample responses

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Content-Length: 314
    Content-Type: Application/xml
    Connection: keep-alive
    x-oss-version-id: CAEQMxiBgID6v86D0BYiIDc3ZDI0YTBjZGQzYjQ2Mjk4OWVjYWNiMDljYzhlN****
    x-oss-request-id: 5CAC4364B7AEADE017000662
    Date: Tue, 09 Apr 2019 07:01:56 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <CompleteMultipartUploadResult>
      <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com/multipart.data</Location>
      <Bucket>oss-example</Bucket>
      <Key>multipart.data</Key>
      <ETag>"097DE458AD02B5F89F9D0530231876****"</ETag>
    </CompleteMultipartUploadResult>
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the CompleteMultipartUpload operation:

-   [Java](/intl.en-US/SDK Reference/Java/Upload objects/Multipart upload.md)
-   [Python](/intl.en-US/SDK Reference/Python/Upload objects/Multipart upload.md)
-   [Go](/intl.en-US/SDK Reference/Go/Upload objects/Multipart upload.md)
-   [C++](/intl.en-US/SDK Reference/C++/Upload objects/Multipart upload.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Upload objects/Multipart upload.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Upload objects/Multipart upload.md)
-   [C](/intl.en-US/SDK Reference/C/Upload objects/Multipart upload.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Upload objects/Multipart upload.md)
-   [Android](/intl.en-US/SDK Reference/Android/Upload objects/Multipart upload.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidDigest|400|The error message returned because the Content-MD5 value in the request is inconsistent with the MD5 hash calculated by OSS. To ensure errors do not occur during data transmission over the network, you can include the Content-MD5 value in the request. OSS calculates the MD5 hash of the uploaded data and compares it with the Content-MD5 value.|
|FileAlreadyExists|409|Possible causes:-   An object that has the same name already exists when the header contains `x-oss-forbid-overwrite=true` that is used to prevent overwriting the object that has the same name.
-   The object on which you want to perform the CompleteMultipartUpload operation is a directory within a bucket for which the hierarchical namespace feature is enabled. |

