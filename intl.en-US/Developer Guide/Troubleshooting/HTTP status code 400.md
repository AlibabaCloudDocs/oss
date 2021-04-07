# HTTP status code 400

This topic describes the causes of errors returned with HTTP status code 400 and the solutions to these errors.

## InvalidBucketName

-   Error message: The specified bucket is not valid.
-   Cause: The bucket name is invalid.
-   Solution: Check the bucket name and make sure that the name complies with naming conventions.

    Bucket names must comply with the following conventions:

    -   The name can contain only lowercase letters, digits, and hyphens \(-\).
    -   The name must start and end with a lowercase letter or digit.
    -   The name must be 3 to 63 bytes in length.

## InvalidObjectName

-   Error message:
    -   The Object name can not be empty.
    -   The Length of Object name must be less than 1024.
    -   The specified object is not valid.
-   Cause: The object name is invalid.
-   Solution: Check the object name and make sure that the name complies with naming conventions.

    Object names must comply with the following conventions:

    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 1,023 bytes in length.
    -   The name cannot start with a forward slash \(/\) or a backslash \(\\\).

## TooManyBuckets

-   Error message: You have attempted to create more buckets than allowed.
-   Cause: The Alibaba Cloud account has reached the maximum number of buckets \(100\) it can create within a region.
-   Solution: Submit a ticket to increase the maximum number of buckets that are allocated to an Alibaba Cloud account within a region.

## RequestIsNotMultiPartContent

-   Error message: Bucket POST must be of the enclosure-type multipart/form-data.
-   Cause: The value of the Content-Type header in the PostObject request is not in the `multipart/form-data` format.
-   The value of the Content-Type header submitted by the PostObject operation must be in the `multipart/form-data` format. The `Content-Type` header must be in the `multipart/form-data;boundary=xxxxxx` format, where `boundary` is a boundary string. For more information, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).

## NotImplemented

-   Error message: A header you provided implies functionality that is not implemented.
-   Cause: The request contains invalid parameters.
-   Solution: Make sure that the parameters are valid before you call the API operation. For more information, see [API overview](/intl.en-US/API Reference/API overview.md).

## MissingArgument

-   Error message: Missing Some Required Arguments.
-   Cause: The request for the API operation does not contain all the required parameters.
-   Solution: Check if you have specified all the required parameters when you call the API operation For more information, see [API overview](/intl.en-US/API Reference/API overview.md).

## InvalidArgument

-   Error message: Invalid Argument. Parameter is invalid.
    -   Cause: The parameter format of the request is not valid.
    -   Solution: Make sure that the parameter format is valid before you call the API operation. For more information, see [API overview](/intl.en-US/API Reference/API overview.md).
-   Error message: The callback signature version is invalid.
    -   Cause: The version of the signature is not supported.
    -   Solution: Make sure that the version of the current signature is 1.0 or 2.0.
-   Error message: Copy Source must mention the source bucket and key: /sourcebucket/sourcekey.
    -   Cause: The source bucket and destination object are not specified.
    -   Solution: When you copy an object, you must specify the source object in the x-oss-copy-source parameter in the following format: `/sourcebucket/sourcekey`. For example, if you want to copy an object named exampleobject from the bucket named examplebucket to another bucket, set x-oss-copy-source to `/examplebucket/exampleobject`.
-   Error message: KMSMasterKeyID is not applicable if the default sse algorithm is not KMS.
    -   Cause: KMSMasterKeyID is specified when SSEAlgorithm is set to AES256.
    -   Solution: Specify KMSMasterKeyID only when you set SSEAlgorighm to KMS and want to use a specified CMK to encrypt objects.
-   Error message: KMSMasterKeyID is not applicable if user is not in white list. or SM4 is not applicable if user is not in white list.
    -   Cause: You not have the permission to perform this operation.
    -   Solution: Only the bucket owner or authorized RAM users can configure encryption rules for the bucket. For more information, see [PutBucketEncryption](/intl.en-US/API Reference/Bucket operations/Encryption/PutBucketEncryption.md).
-   Error message: No such bucket storage class exists.
    -   Cause: The bucket storage class in the specified in the request is invalid.
    -   Solution: OSS supports only Standard, Infrequent Access \(IA\), Archive, and Cold Archive. For more information, see [PutBucket](/intl.en-US/API Reference/Bucket operations/Basic operations/PutBucket.md).
-   Error message: Invalid version id specified.
    -   Cause: The object version ID specified in the request is invalid.
    -   Solution: Use the GetBucketVersions \(ListObjectVersions\) operation to confirm whether versioning is enabled for the bucket and whether the specified object version ID is valid.
-   Error message: Version id can not be the empty string.
    -   Cause: The VersionId field is not specified.
    -   Solution: Set the VersionId field to specify the object version ID. For more information, see [GetBucketVersions\(ListObjectVersions\)](/intl.en-US/API Reference/Bucket operations/Versioning/GetBucketVersions(ListObjectVersions).md).
-   Error message:

    Authorization header is invalid.

    Authorization header is invalid, bad num of Items in Authorization header.

    AccessKeyId or Signature is missing in Authorization header value.

    Authorization header value is empty.

    Unknown parameter in Authorization header.

    -   Cause: The parameters used to calculate the Authorization header are invalid.
    -   Solution: The following code provides an example on how to calculate the Authorization header. Specify the parameters correctly based on the description in the following table.

        ```
        Authorization = "OSS " + AccessKeyId + ":" + Signature
        Signature = base64(hmac-sha1(AccessKeySecret,
                    VERB + "\n"
                    + Content-MD5 + "\n" 
                    + Content-Type + "\n" 
                    + Date + "\n" 
                    + CanonicalizedOSSHeaders
                    + CanonicalizedResource))
        ```

        |Parameter|Description|
        |---------|-----------|
        |AccessKeySecret|The key used to encrypt the signature string.|
        |VERB|The method of the HTTP request, such as PUT, GET, POST, HEAD, and DELETE.|
        |\\n|The line feed.|
        |Content-MD5|The MD5 hash of the request content. This parameter is used to check the validity of the request. For example, you can use this parameter to check whether the content of the received request is the same as that of the sent request.For more information about how to calculate the value of Content-MD5, see [Calculate the Content-MD5 value](/intl.en-US/API Reference/Access control/Add signatures to headers.md). |
        |Content-Type|The type of the request content. Example: `application/octet-stream` or empty.|
        |Date|The time when this operation is performed. The value of this parameter must be in GMT. Example: `Sun, 22 Nov 2015 08:16:38 GMT`.|
        |CanonicalizedOSSHeaders|HTTP headers that contains the `x-oss-` prefix.|
        |CanonicalizedResource|The OSS resources accessed by the user.|

-   Error message: Post request accessKeyId is empty.
    -   Cause: The OSSAccessKeyId form field is not specified in the headers of the POST request.
    -   Solution: Check the bucket ACL. The OSSAccessKeyId form field is required when the bucket ACL is not public read/write or when the policy or Signature form field is contained in the request.
-   Error message: Post request signature is empty.
    -   Cause: The Signature form field is not specified in the headers of the POST request.
    -   Solution: Check the bucket ACL. The Signature form field is required when the bucket ACL is not public read/write or when the policy or Signature form field is contained in the request.

        You can perform the following steps to calculate the value of the Signature form field:

        1.  Create a UTF-8-encoded policy.
        2.  Encode the policy in Base64. The result is the value of the policy form field, and this value is used as the string to sign.
        3.  Use AccessKeySecret to sign the string by using the following method: `Signature = base64(hmac-sha1(base64(policy), AccessKeySecret))`.
-   Error message: OSS authentication requires a valid Date.
    -   Cause: The Date field in the Authorization header is invalid.
    -   Solution: Check whether the value in the Date field is in GMT. The Date field specifies the time when this operation is performed. Example: `Sun, 22 Nov 2015 08:16:38 GMT`.
-   Error message: The bucket POST must contain the specified field name.
    -   Cause: The form of the POST request contains parameters that are invalid or are in the incorrect format.
    -   Solution: Specify valid parameters. For more information, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).
-   Error message: The bucket POST contains unrecognized field name.
    -   Cause: The line after the `filename` field contains custom parameters.
    -   Solution: Make sure that the line after the `filename` field is the `Content-Type` parameter. The following figure shows an example of the Content-Type parameter in the line follwing the filename field.

        ![file-name](../images/p224019.png)

-   Error message: Post body size must be less than 5G.
    -   Cause: The total size of the POST request body exceeds 5 GB.
    -   Solution: Make sure that the body of POST requests do not exceed 5 GB in size. For more information, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).
-   Error message: The callback configuration is not base64 encoded.
    -   Cause: The callback parameter is not Base64-encoded.
    -   Solution: Set the callback parameter to a Base64-encoded JSON string. The string must contain the URL of the server to which callback requests are sent and the content of the callback information sent to the URL. For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).
-   Error message: The callback configuration is not json format.
    -   Cause: The callback parameter is not in the JSON format.
    -   Solution: Set the callback parameter to a Base64-encoded JSON string. The following code provides an example of the callback parameter:

        ```
        {
        "callbackUrl":"121.101.166.30/test.php",
        "callbackHost":"oss-cn-hangzhou.aliyuncs.com",
        "callbackBody":"{\"mimeType\":${mimeType},\"size\":${size}}",
        "callbackBodyType":"application/json"
        }
        ```

-   Error message: The callback url configuration is invalid.
    -   Cause: The callback parameter does not contain the callbackUrl field, which specifies the URL of the server to which the callback request is sent.
    -   Solution: Set the callback parameter to a Base64-encoded JSON string. The string must contain the URL of the server to which callback requests are sent and the content of the callback information sent to the URL.
-   Error message: The callback host configuration is invalid.
    -   Cause: The callbackHost parameter is invalid and cannot be parsed as a JSON object.
    -   Solution: Check the parameter value of callbackHost. The callbackHost parameter indicates the Host header in the callback request. Example: `oss-cn-hangzhou.aliyuncs.com`. If the callbackHost parameter is not specified, OSS parses the URL specified by the callbackUrl parameter and sets the callbackHost parameter to the host parsed from the URL.
-   Error message: The callback body type configuration is invalid. or The body type of callback is not supported.
    -   Cause: The callbackBodyType parameter is invalid and cannot be parsed as a JSON object.
    -   Solution: Check the parameter value of callbackBodyType. The callbackBodyType parameter specifies the Content-Type header in the callback request. The valid values of the parameter are `application/x-www-form-urlencoded` and `application/json`.
-   Error message: The number of callback url exceed max value.
    -   Cause: The callbackUrl parameter contains more than five URLs.
    -   Solution: Check if the callbackUrl parameter contains more than five URLs. You can configure up to five URLs in a request. Each URL must be separated with a semicolon \(;\).
-   Error message: The header value specified by persistent header is not utf-8 encoded.
    -   Cause: The value of the custom header is invalid.
    -   Solution: Encode special characters such as Chinese characters in the value of the custom header in UTF-8.
-   Error message: The header value specified by persistent header is not equal to request header value.
    -   Cause: The value of the custom header is different from that of a standard request header that has the same name.
    -   Solution: If a custom header has the same name as a standard request header, the values of both the headers must be the same.
-   Error message: The header value specified by persistent header contains CR or LF.
    -   Cause: The value of the custom header contains carriage returns \(`\r`\) or line feeds \(`\n`\).
    -   Solution: Delete the carriage returns \(`\r`\) or line feeds \(`\n`\) in the value of the custom header.
-   Error message: Either the callback query string parameter or the x-oss-callback header should be specified, not both.
    -   Cause: The request URL and request header both contain the callback parameter.
    -   Solution: Remove the callback parameter from the request URL or the request header. You can only include the callback parameter in one of these fields.
-   Error message: Either the callback-var query string parameter or the x-oss-callback-var header should be specified, not both.
    -   Cause: The request URL and request header both contain the callback-var parameter.
    -   Solution: Remove the callback-var parameter from the request URL or the request header. You can only include the callback-var parameter in one of these fields.
-   Error message: x-oss-traffic-limit should be specified either in query string or header, not both.
    -   Cause: The request URL and request header both contain the `x-oss-traffic-limit` parameter, which is used to limit the transmission speed of a single link.
    -   Solution: Remove the `oss-traffic-limit` parameter from the request URL or the request header. You can only include the oss-traffic-limit parameter in one of these fields.
-   Error message: x-oss-traffic-limit is invalid, should be specified between 819200\(100KB/s\) and 838860800\(100MB/s\).
    -   Cause: The transmission speed limit for a single link is invalid.
    -   Solution: Set the transmission speed limit of a single link to a value from 819200 bit/s \(100 KB/s\) to 838860800 bit/s \(100 MB/s\).
-   Error message: The format of persistent header is invalid, should be \\"name:Base64Encode\(value\),name:Base64Encode\(value\)... \\". or The header value specified by persistent header is not base64 encoded.
    -   Cause: The format of the custom header is invalid.
    -   Solution: Specify the custom headers in the`x-oss-persistent-headers: key1:base64_encode(value1),key2:base64_encode(value2)...` format, where key1 and key2 indicates the custom headers, value1 and value2 indicates the values of the custom headers, and base64\_encode indicates that the values of the custom headers are Base64-encoded.

        For example, if you configure two custom headers named myheader1 and myheader2 and set their values to myvalue1 and myvalue2 respectively, the custom headers contained in the request is `x-oss-persistent-headers:myheader1:bXl2YWx1ZTE=,myheader2:bXl2YWx1ZTI=`.

-   Error message: The header 'x-oss-tagging' shall be encoded as UTF-8 then URLEncoded URL query parameters without tag name duplicates.
    -   Cause: The keys and values of tags are not URL-encoded and multiple tags have the same key.
    -   Solution: Check whether the tag is correct. A tag is a key-value pair used to identify an object. Object tags must comply with the following rules:
        -   You can add up to 10 tags to an object. The tags added to an object must have unique keys.
        -   A tag key can be up to 128 bytes in length. A tag value can be up to 256 bytes in length.
        -   Tag keys and values are case-sensitive.
        -   The key and value of a tag can contain letters, digits, spaces, and the following special characters:

            + - = . \_:/

            If the HTTP header contains tags that contain the preceding characters, you must perform URL encoding on the keys and values of the tags.

-   Error message: The header specified by persistent header is reserved.
    -   Cause: The custom headers in the request conflict with standard HTTP request headers.
    -   Solution: Do not specify standard HTTP request headers as custom headers, such as `Host`, `Content-MD5`, `Origin`, and `Range`.
-   Error message: The header specified by persistent header is not valid HTTP token.
    -   Cause: The custom headers in the request contain invalid characters.
    -   Solution: Check whether custom headers contain invalid characters that are specified in RFC 7230.
-   Error message: Can not get ip by this host.
    -   Cause: The Host header is invalid.
    -   Solution: The value of the Host field can be the domain name or IP address of a bucket. The domain name of a bucket is in the `BucketName.Endpoint` format. Example: `examplebucket.oss-cn-hangzhou.aliyuncs.com`. If the value of the Host field is an IP address, make sure that the IP address is valid.
-   Error message: Private address is forbidden to callback.
    -   Cause: OSS cannot send callback requests to an IP address that resides in the internal network.
    -   Solution: Set the URL of the server to which callback requests are sent to a public IP address.
-   Error message: The length of callback exceed max value.
    -   Cause: The size of the Base64-encoded callback parameter exceeds 5 KB.
    -   Solution: Make sure that the total size of the Base64-encoded callback parameter does not exceed 5 KB.
-   Error message: The callback body is empty.
    -   Cause: The callbackBody parameter is not specified.
    -   Solution: Set the callback parameter to a Base64-encoded JSON string. The string must contain the URL of the server to which callback requests are sent \(callbackUrl\) and the content of the callback information sent to the URL \(callbackBody\).
-   Error message: The callback body is invalid.
    -   Cause: The callbackBody parameter is invalid and cannot be parsed as a JSON object.
    -   Solution: Check the parameter value of callbackBody. The callbackBody parameter specifies the value of the callback request body. Example: `key=$(object)&etag=$(etag)&my_var=$(x:my_var)`. For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).
-   Error message: The length of callback var exceed max value.
    -   Cause: The total size of the Base64-encoded callback-var parameter exceeds 5 KB.
    -   Solution: Make sure that the total size of the Base64-encoded callback-var parameter does not exceed 5 KB.
-   Error message: The callback var is not expecten json.
    -   Cause: The callback-var parameter is not in the JSON format.
    -   Solution: Check whether the callback-var parameter is in the JSON format. The callback-var parameter is used to configure custom parameters, which are key-value pairs. The key of a custom parameter must be in lower-case and start with `x:`. For example, you want to configure two custom parameters whose keys are `x:var1` and `x:var2` and the values of the two parameters are value1 and value2 respectively. The callback-var parameter is in the following JSON format:

        ```
        {
        "x:var1":"value1",
        "x:var2":"value2"
        }
        ```

-   Error message: Invalid date. Must be later than 1970-01-01 00:00:00.
    -   Cause: The Date parameter is a UNIX timestamp that is earlier than 00:00:00, January 1, 1970.
    -   Solution: Set the value of the Date parameter to a UNIX timestamp that is later than 00:00:00, January 1, 1970.
-   Error message: Invalid date. Cannot be later than the current time.
    -   Cause: The value of the Date parameter is a UNIX timestamp that is later than the current time.
    -   Solution: Set the value of the Date parameter to a UNIX timestamp that is earlier than the current time.
-   Error message: Bad date format.
    -   Cause: The Date parameter is invalid.
    -   Solution: Set the Date parameter to a valid value that is specified in RFC 1123. Example: `Sun, 06 Nov 1994 08:49:37 GMT`.
-   Error message: The Versioning element must be specified.
    -   Cause: The versioning status of the bucket is not specified in the <VersioningConfiguration\> field.
    -   Solution: Specify the versioning status of the bucket in the <VersioningConfiguration\> field. The following code provides an sample request in which the versioning status of the bucket is specified in the <VersioningConfiguration\> field:

        ```
        PUT /? versioning HTTP/1.1
        Host: bucket-versioning.oss-cn-hangzhou.aliyuncs.com
        Date: Tue, 09 Apr 2019 02:20:12 GMT
        Authorization: OSS e7thre3jj5mlvqk:12ztptkaR8a74gIGFzOaZZQe****
        <? xml version="1.0" encoding="UTF-8"? >
        <VersioningConfiguration>
            <Status>Enabled</Status>
        <VersioningConfiguration>
        ```


## RepeatedTags

-   Error message: Tag keys must be unique.
-   Cause: The specified tag key already exists.
-   Solution: Call [GetBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/GetBucketTags.md) to obtain the information about the tags configured for the bucket, and then configure a new tag for the bucket. For more information, see [PutBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/PutBucketTags.md).

## InvalidHostPutBucket

-   Error message: Your host is invalid. Please put bucket use Open Storage Service standard host.
-   Cause: The Host parameter is invalid.
-   Solution: Set the Host parameter to a standard domain name. Make sure that the domain name can accept requests. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

## InvalidEncryptionAlgorithmError

-   Error message: The encryption algorithm specified is not valid. or The Encryption request you specified is not valid. Supported value: AES256/SM4/KMS.
-   Cause: The value of `x-oss-server-side-encryption` is invalid.
-   Solution: Set `x-oss-server-side-encryption` to one of the following valid values: AES256 or KMS. For more information about server-side encryption, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## InvalidDataEncryptionAlgorithmError

-   Error message: The KMS Data Encryption request you specified is not valid. Supported value:SM4
-   Cause: The encryption algorithm for objects specified in the request is invalid.
-   Solution: Check whether the encryption algorithm specified in the request is supported. If you set x-oss-server-side-encryption to KMS, only the SM4 encryption algorithm is supported. For more information, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

## InvalidParameter

-   Error message: The specified parameter KMS keyId is not valid.
-   Cause: The CMK ID is invalid.
-   Solution: Check the CMK ID specified in the request. If you set x-oss-server-side-encryption to KMS and want to use a CMK to encrypt objects, you must specify the CMK ID in the request. Example: `9468da86-3509-4f8d-a61e-6eab1eac****`.

## InvalidWORMConfiguration

-   Error message: Invalid WORM Configuration.
-   Cause: The specified retention policy is invalid.
-   Solution: Call InitiateBucketWorm to create a retention policy for a bucket. The following code provides a sample request:

    ```
    POST /? worm HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Length: 556
    Content-Type: application/xml
    Host: BucketName.oss.aliyuncs.com
    Authorization: OSS nxj7dtlhcyl5hp****:COS3OQkfQPnKmYZTEHYv2****
    
    <InitiateWormConfiguration>
      <RetentionPeriodInDays>365</RetentionPeriodInDays>
    </InitiateWormConfiguration>
    ```


## InventoryExceedLimit

-   Error message: You are not allowed to create more inventory than limit.
-   Cause: You have configured the maximum number of inventories \(1,000\) for the bucket.
-   Solution: Submit a ticket to increase the maximum number of inventories that can be configured for a bucket.

## DailyInventoryExceedLimit

-   Error message: daily inventory object count exceed limit.
-   Cause: The number of objects in the bucket exceed the daily export limit for inventory lists.
-   Solution: Make sure that the number of objects in the bucket is less than 1 billion. You can call [GetBucketV2 \(ListObjectsV2\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md) to obtain the number of objects in the bucket.

## WeeklyInventoryExceedLimit

-   Error message: weekly inventory object count exceed limit.
-   Cause: The number of objects in the bucket exceed the weekly export limit for inventory lists.
-   Solution: Make sure that the number of objects in the bucket is less than 10 billion. You can call [GetBucketV2 \(ListObjectsV2\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketV2 (ListObjectsV2).md) to obtain the number of objects in the bucket.

## BadRequest

-   Error message: Insufficient information. Origin request header needed.

    Cause: The request does not contain the Origin header .

    Solution: Include the Origin header in the request. Before the browser sends a cross-origin request, the browser sends a preflight \(OPTIONS\) request that contains the Origin header to identify the domain name from which the request is sent. The following figure shows an example of a preflight request that contains the Origin header:

    ![origin](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5527877161/p225158.png)

-   Error message: Insufficient information. Request Access-Control-Request-Method header needed.

    Cause: The request does not contain the Access-Control-Request-Method header.

    Solution: Include the Access-Control-Request-Method header in the request. Before the browser sends a cross-origin request, the browser sends a preflight \(OPTIONS\) request that contains the Access-Control-Request-Method header to identify the method used by the actual request. The following figure shows an example of a preflight request that contains the Access-Control-Request-Method header:

    ![method](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5527877161/p225159.jpg)


## RequestTimeout

-   Error message: Your socket connection to the server was not read from or written to within the timeout period. Idle connections will be closed.
-   Cause: The request times out due to network environments or configurations.
-   Solution: See [Network connection timeout troubleshooting]().

## InvalidTaggingKey

-   Error message: The tagging key you provided is invalid.
-   Cause: The key of the tag specified in the request is invalid.
-   Solution: Check whether the tag is correct. A tag is a key-value pair used to identify an object. Object tags must comply with the following rules:
    -   A tag key can be up to 128 bytes in length. A tag value can be up to 256 bytes in length.
    -   Tag keys and values are case-sensitive.
    -   The key and value of a tag can contain letters, digits, spaces, and the following special characters:

        + - = . \_:/

        If the HTTP header contains tags that contain the preceding characters, you must perform URL encoding on the keys and values of the tags.


## TooManyCname

-   Error message: You have attempted to create more buckets than allowed.
-   Cause: You have bound the maximum number of domain names \(100\) to the bucket.
-   Solution: Submit a ticket to increase the maximum number of domain names that can be bound to a bucket.

## InvalidChannelName

-   Error message:

    The Length of channel name must be less than 64.

    ChannelName must not be empty.

    The characters encoding must be utf-8.

    ChannelName must not contain a slash.

-   Cause: The name of the Channel does not comply with naming conventions.
-   Solution: Check the Channel name and make sure that the name complies with the naming conventions.

    The name of a Channel must comply with the following conventions:

    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 64 bytes in length.
    -   The name cannot contain forward slashes \(/\) or backslashes \(\\\).

## InvalidPart

-   Error message: One or more of the specified parts could not be found or the specified entity.
-   Cause: The PartNumber or ETag of a part uploaded by using CompleteMultipartUpload is invalid.
-   Solution: Check whether the parameter values of PartNumber and ETag are correct. OSS verifies the values of PartNumber and ETag when you call the CompleteMultipartUpload operation.
    -   The valid values of PartNumber ranges from 1 to 10000. The part numbers listed in the request do not have to be consecutive but must be sorted in ascending order. For example, if the part number of the first part is 1, the part number of the second part can be 5.
    -   The ETag value of an object created by CompleteMultipartUpload is the UUID of the object. The ETag value of an object can be used to check whether the object content is changed.

## InvalidPartOrder

-   Error message: The list of parts was not in ascending order.

    Cause: The part numbers of the parts uploaded by using CompleteMultipartUpload are not in ascending order.

    Solution: Make sure that the part numbers of the parts uploaded using CompleteMultipartUpload are in ascending order. For example, if the part number of the first part is 1, the part number of the second part can be 5.

-   Error message: Part number must be an integer between 1 and 10000, inclusive.

    Cause: The value of PartNumber is invalid.

    Solution: Make sure that the value of PartNumber ranges from 1 to 10000.


## FilePartNotExist

-   Error message: The Part you read had been deleted.
-   Cause: The part specified in the request does not exist.
-   Solution: Check whether all parts are uploaded. For more information, see [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

## InvalidXMLFormat

-   Error message: The XML you provided was not well-formed.
-   Cause: The parameter is not in a valid XML format.
-   Solution: Check whether the parameter is in a valid XML format.

## InvalidRequest

-   Error message: Request specific response headers cannot be used for anonymous GET requests.

    Cause: The request does not contain all the required parameters.

    Solution: Check whether all required parameters of the API operation are contained in the request. For more information, see [API overview](/intl.en-US/API Reference/API overview.md).

-   Error message: Security-token must be provided by query string parameter.

    Cause: The request does not contain security-token.

    Solution: Make sure that the requests contains security-token when a temporary user uses a signed URL to access OSS resources. Temporary users can use a signed URL in the following format to access OSS resources: `http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936****&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv****&security-token=SecurityToken`. For more information, see [Generate a signed URL](/intl.en-US/API Reference/Access control/Generate a signed URL.md).

-   Error message: Size of playlist is too big.

    Cause: The size of the playlist file exceeds the limit.

    Solution: Make sure that the size of the playlist file does not exceed 1 MB.

-   Error message: No ts found in the playlist.

    Cause: No TS files are found.

    Solution: Ingest a stream to upload audio and video objects. For more information, see [PutLiveChannel](/intl.en-US/API Reference/LiveChannel-related operations/PutLiveChannel.md).

-   Error message: Playlist name must ends with \\".m3u8\\".

    Cause: The playlist file does not end with `.m3u8`.

    Solution: Check the the uploaded data. If the uploaded data uses the HLS protocol, configure the name of the generated playlist file to end with .m3u8. The name of the generated playlist file must end with `.m3u8` and range from 6 to 128 characters in size. Example: `playlist.m3u8`.

-   Error message: Master playlist has ts file.

    Cause: The master playlist file contains both M3U8 files and TS files.

    Solution: Delete the TS files from the master playlist. Playlist files are in the M3U8 format and used to record the information about the video files in the TS format. For more information, see [Create HLS streams based on OSS](/intl.en-US/Best Practices/Create HLS streams based on OSS.md).


## MaxPOSTPreDataLengthExceededError

-   Error message: Your POST request fields preceding the upload file were too large.
-   Cause: The size of the object that you want to upload by using PostObject is larger than 5 GB.
-   Solution: See [PostObject errors and troubleshooting]().

## TooManyPipes

-   Error message: Maximal number of pipes supported is.
-   Cause: The number of operations performed on the image exceeds the limit.
-   Solution: Make sure that the number of operations that you perform on the image does not exceed the limit.

## FieldItemTooLong

-   Error message: Your name of form field is too long.
-   Cause: The total length of the form fields in the POST request exceeds the limit.
-   Solution: Make sure that the form fields in the request excluding the file form field do not exceed 4 KB in size. For more information, see [PostObject errors and troubleshooting]().

## MalformedPOSTRequest

-   Error message: The body of your POST request is not well-formed multipart/form-data.
-   Cause: The format of the form field is invalid.
-   Solution: Make sure that the format of the form field is valid.

    Form fields must be in the following format:

    ```
    Content-Disposition: form-data;
            name="{key}"\r\n\r\n{value}\r\n--{boundary}
    ```

    A PostObject request must be in the following format:

    ```
    POST / HTTP/1.1
    User-Agent: Mozilla/5.0 (Windows; U; Windows NT 6.1; zh-CN; rv:1.9.2.6)
    Content-Type: multipart/form-data; boundary=9431149156168
    Host: xxxx-hz.oss-cn-hangzhou.aliyuncs.com
    Accept: text/html, image/gif, image/jpeg, *; q=.2, */*; q=.2
    Connection: keep-alive
    Content-Length: 5052
    --9431149156168
    Content-Disposition: form-data; name="key"
    test-key
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    attachment;filename=D:\img\example.png
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    2NeL********j2Eb
    ```

    A PostObject request must meet the following requirements:

    -   The request must contain the Content-Type: multipart/form-data; boundary=\{boundary\} header.
    -   The request header and body are separated by `\r\n--{boundary}`, in which `\r\n` is displayed as a line feed.
    -   The names of form fields are case-sensitive, such as `policy`, `key`, `file`, `OSSAccessKeyId`, `OSSAccessKeyId`, and `Content-Disposition`.
    -   The last form field must be `file`.
    -   When the ACL of the bucket is `public-read-write`, you do not have to specify the following form fields: `OSSAccessKeyId`, `policy`, and `Signature`. If you specify one of these three fields, you must specify the other two fields regardless of whether the ACL of the bucket is `public-read-write`.
    **Note:** The preceding example is a part of a PostObject request. For the complete request sample, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).

    For more information about the PostObject request, see the sample code used by OSS SDKs for the following programming languages:

    -   [C\#](https://github.com/aliyun/aliyun-oss-csharp-sdk/blob/master/samples/Samples/PostPolicySample.cs)
    -   [Java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/PostObjectSample.java)
    -   [JavaScript](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the client by using JavaScript and upload data to OSS.md)

## InvalidPolicyDocument

-   Invalid Simple-Condition
    -   Error message: Invalid Policy: Invalid Simple-Condition: Simple-Conditions must have exactly one property specified.
    -   Cause: The policy header in the request does not contain the `conditions` field.
    -   Solution: Make sure that the policy field contains at least one `conditions` field.
-   Invalid JSON: unknown char e
    -   Error message: Invalid Policy: Invalid JSON: unknown char e.
    -   Cause: The format of the policy header in the request is invalid.
    -   Solution: Check whether double quotation marks \(`"`\) are missing in the policy header and whether a backslash \(`\`\) is added before each escape character.
-   Invalid JSON: , or \] expected
    -   Error message: Invalid Policy: Invalid JSON: , or \] expected.
    -   Cause: The format of the policy header in the request is invalid.
    -   Solution: Check whether commas \(`,`\) or right brackets \(`]`\) are missing in the policy header.

## IncorrectNumberOfFilesInPOSTRequest

-   Error message: POST requires exactly one file upload per request.
-   Cause: The number of files contained in the PostObject request is invalid. A PostObject request can contain only one `file` form field.
-   Solution: Make sure that the PostObject request contains only one `file` form field.

## EntityTooSmall

-   Error message: Your proposed upload smaller than the minimum allowed size.
-   Cause: The size of the object to upload is smaller than the minimum value.
-   Solution: When you call PostObject to upload files, configure the Post policy to specify the valid values of form fields. You can configure the `content-length-range` condition to specify the maximum and minimum sizes of the object to upload.

## EntityTooLarge

-   Error message: Your proposed upload exceeds the maximum allowed size. or Source object Length exceeds the maximum allowed size.
-   Cause: The size of the object to upload is larger than the maximum value.
-   Solution: When you call PostObject to upload files, configure the Post policy field to specify the valid values of form fields. You can configure the `content-length-range` condition to specify the maximum and minimum sizes of the object to upload.

## MalformedXML

-   Error message: The XML you provided was not well-formed or did not validate against our published schema.
-   Cause: The format of the XML message body of the request is invalid.
-   Solution: Troubleshoot the error based on your request.

    For more information about how to handle errors for different requests, see the following topics:

    -   [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md)
    -   [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md)
    -   [PutBucketLogging](/intl.en-US/API Reference/Bucket operations/Logging/PutBucketLogging.md)
    -   [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md)
    -   [PutBucketLifecycle](/intl.en-US/API Reference/Bucket operations/Lifecycle/PutBucketLifecycle.md)
    -   [PutBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/PutBucketReferer.md)
    -   [PutBucketCORS](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md)

## NoReplicationRule

-   Error message: No replication rule specified.
-   Cause: No cross-region replication \(CRR\) rules are configured for the bucket specified in the request.
-   Solution: Configure CRR rules for the bucket. For more information, see [PutBucketReplication](/intl.en-US/API Reference/Bucket operations/Cross-region replication/PutBucketReplication.md).

## TooManyReplicationRules

-   Error message: OSS only support one replication rule now.
-   Cause: The number of CRR rules configured for a single bucket exceeds the limit.
-   Solution: You can configure only one CRR rule for each bucket.

## InvalidBucketStatus

-   Error message: The src and dest bucket must be versioning enabled when drs tagging rule exist.
-   Cause: The versioning states of the source bucket and destination bucket in CRR are different.
-   Solution: Make sure that the source bucket and destination bucket in CRR are in the same versioning state.

## InvalidTargetBucket

-   Error message: The target bucket is invalid for bucket replication.
-   Cause: The destination configured in the CRR rule is invalid.
-   Solution: Call [GetBucketReplicationLocation](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplicationLocation.md) to query the region in which the destination bucket can be located. You can determine the region of the destination bucket based on the response returned for the operation.

## InvalidTargetLocation

-   Error message: The target bucket you specified does not locate in the target location.
-   Cause: The specified destination bucket does not exist in the destination region.
-   Solution: Check whether the region specified in the request is correct.

## BadReplicationLocation

-   Error message: The replication location you choose is invalid.
-   Cause: The destination region specified in the CRR rule does not exist.
-   Solution: Check whether the region specified in the request is correct.

## BucketAlreadyHasReplicationConfiguration

-   Error message: The bucket you specified already has replication configuration.
-   Cause: A CRR rule is configured for the specified bucket.
-   Solution: CRR can be configured only between two buckets that are not synchronized with other buckets. CRR can be configured bidirectional between the two buckets. To synchronize data from a source bucket that has an existing CRR rule to a destination bucket that is not specified in the CRR rule, you must first delete the existing CRR rule.

## NoReplicationLocation

-   Error message: The bucket does not have corresponding replication location.
-   Cause: The region of the source bucket does not have a matching region for which CRR can be configured.
-   Solution: See [GetBucketReplicationLocation](/intl.en-US/API Reference/Bucket operations/Cross-region replication/GetBucketReplicationLocation.md).

## BucketReplicationAlreadyExist

-   Error message: Bucket replication already exists.
-   Cause: The bucket specified in the request has an existing CRR rule with another bucket.
-   Solution: Do not enable CRR for buckets that already have CRR enabled.

## CORSRuleBeyondLimit

-   Error message: The CORS Rules num is beyond limit.
-   Cause: The number of cross-origin resource sharing \(CORS\) rules configured for the bucket exceeds the limit.
-   Solution: Check the number of CORS rules configured for the bucket. You can configure up to 10 CORS rules for a bucket. For more information, see [PutBucketCors](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md).

## TooManyTags

-   Error message: The bucket tags num is beyond limit.
-   Cause: The number of tags configured for the bucket exceeds the limit.
-   Solution: Check the number of tags configured for the bucket. You can configure up to 20 tags for a bucket. For more information, see [PutBucketTags](/intl.en-US/API Reference/Bucket operations/Tagging/PutBucketTags.md).

## TooManyPrefixes

-   Error message: The bucket replication rule's prefixes number is beyond limit.
-   Cause: The number of prefixes specified in the CRR rule exceeds the limit.
-   Solution: Check whether the number of prefixes specified in the CRR rule exceeds 10. You can specify up to 10 prefixes in a CRR rule to synchronize only objects whose names contain the specified prefixes to the destination bucket.

## TooManyFilterObjectTags

-   Error message: The bucket replication rule's filter object tags number is beyond limit.
-   Cause: The number of tags specified in the CRR rule exceeds the limit.
-   Solution: Check whether the number of tags specified in the CRR rule exceeds 10. You can specify up to 10 tags in a CRR rule to synchronize only objects that have the specified tags to the destination bucket.

## InvalidDigest

-   Error message: The Content-MD5 you specified was invalid.
-   Cause: The value of the Content-MD5 request header is different from the Content-MD5 value of the message body calculated by OSS.
-   Solution: See [Calculate the Content-MD5 value](/intl.en-US/API Reference/Access control/Add signatures to headers.md).

## InvalidCRC64

-   Error message: The x-oss-hash-crc64ecma you specified does not match what we calculated.
-   Cause: The CRC64 value returned by the server is different from the CRC64 value calculated by the local client.
-   Solution: See [Check data transmission integrity by using CRC-64](/intl.en-US/Best Practices/Check data transmission integrity by using CRC-64.md).

## InlineDataTooLarge

-   Error message: Inline data exceeds the maximum allowed size.
-   Cause: The size of the object to upload exceeds the limit.
-   Solution: Make sure that the size of the object does not exceed the limit for the methods used to upload the object.
    -   Objects uploaded by using [Simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md) cannot be larger than 5 GB in size.
    -   Objects uploaded by using [Form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md) cannot be larger than 5 GB in size.
    -   Objects uploaded by using [Append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md) cannot be larger than 5 GB in size.
    -   Objects uploaded by using [Multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) cannot be larger than 48.8 TB in size.

## ImageTooLarge

-   Error message: Maximal size of image supported is.
-   Cause: The size of the source image exceeds the limit.
-   Solution: Make sure that the source image does not exceed the following limits:
    -   The size of the source image cannot exceed 20 MB.
    -   The width or height of the source image cannot exceed 4,096 pixels when you rotate images.
    -   Each side of the source image cannot exceed 30,000 pixels.

## IncompleteBody

-   Error message: You did not provide the number of bytes specified by the Content-Length HTTP header.
-   Cause: The value of the Content-Length request header is different from the size of the data sent by the request in bytes.
-   Solution: Set the value of the Content-Length header to the actual size of the data that you want to send. Specify this value in bytes.

## KmsServiceNotEnabled

-   Error message: This user does not turn on KMS service.
-   Cause: Key Management Service \(KMS\) is not activated when KMS is specified as the encryption method for server-side encryption.
-   Solution: Make sure that KMS is activated before you set the encryption method of server-side encryption to KMS. For more information, see [Activate KMS](/intl.en-US/Quick Start/Activate KMS.md).

## OperationNotSupported

-   Error message: The operation is not supported for this resource.
-   Cause: The specified operation is not supported by the resources.
-   Solution: This error tends to occur when you attempt to restore an object whose storage class is not Archive or Cold Archive. For more information about the operations supported by different resources, see [API overview](/intl.en-US/API Reference/API overview.md).

## NotSymlink

-   Error message: The object is not symlink.
-   Cause: The object on which the operation is performed is not a symbolic link.
-   Solution: Make sure that the PutSymlink or GetSymlink operation is performed only on symbolic links.

## InvalidTag

-   Error message: The TagKey you have provided is invalid. or The TagValue you have provided is invalid.
-   Cause: The key or value of the tag configured for the bucket is invalid.
-   Solution: Make sure that the key and value of the tag configured for the bucket comply with the following conventions:
    -   The key and value of the tag must be UTF-8-encoded.
    -   The key of a tag can be up to 64 bytes in length. It cannot be null and cannot start with `http://`, `https://`, or `Aliyun`.
    -   The value of a tag can be up to 128 bytes in length and can be null.

## InvalidEncryptionRequest

-   Error message: The parameters of client encryption are allowed to be set once.

    Cause: You attempted to use the CopyObject operation to modify object metadata related protected by client-side encryption.

    Solution: Do not use CopyObject to modify object metadata. After you upload objects encrypted on the client, object metadata related to client-side encryption is protected. In this case, CopyObject cannot be used to modify object metadata. For more information, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

-   Error message: Miss some necessary client encryption meta parameters.

    Cause: The request does not contain all the required parameters related to client-side encryption.

    Solution: Make sure that all the required parameters described in the following table are specified in the request

    |Parameter|Description|
    |:--------|:----------|
    |x-oss-meta-client-side-encryption-key|The encrypted data key, which is a string encrypted using a CMK and encoded in Base64.|
    |x-oss-meta-client-side-encryption-start|The initial value which is randomly generated for data encryption. The initial value is a string encrypted using a CMK and encoded in Base64.|
    |x-oss-meta-client-side-encryption-cek-alg|The algorithm used to encrypt data.|
    |x-oss-meta-client-side-encryption-wrap-alg|The algorithm used to encrypt the data key.|

-   Error message: The parts count calculated by client encryption meta is too large.

    Cause: The number of parts specified in object metadata related to client-side encryption exceeds the limit.

    Solution: Split the object into multiple parts. An object can be split into up to 10,000 parts.

-   Error message: The client encryption meta data\_size or part\_size is invalid.

    Cause: The total size or part size specified in object metadata for client-side encryption is invalid.

    Solution: Check whether the size is an integer multiple of 16. When you perform client-side encryption on an object that you want to upload by using multipart upload, you must configure the total size \(x-oss-meta-client-side-encryption-data-size\) and part size \(x-oss-meta-client-side-encryption-data-size\) of the object for init\_multipart. The part size must be an integer multiple of 16. For more information, see [Client-side encryption](/intl.en-US/SDK Reference/Java/Data encryption/Client-side encryption.md).

-   Error message: The client encryption part list is unexpected with init\_multipart setted.

    Cause: The number of parts uploaded to OSS and the number of parts specified when the multipart upload task is initialized are different.

    Solution: Make sure that the specified number of parts is the same as the number of parts that are uploaded to OSS.

-   Error message: The partId must less or equal to expectedPartNumber.

    Cause: The ID of the part is larger than the value of PartNumber, which indicates the total number of parts.

    Solution: Make sure that the ID of each part is equal to or smaller than the total number of parts. For more information, see [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md).

-   Error message: The partSize must same with init\_multipart setted except last part.

    Cause: The size of uploaded parts is different from the part size specified when the multipart upload task is initialized.

    Solution: Make sure that the size of all parts except for the last part is the same as the part size specified when the multipart upload task is initialized. For more information, see [UploadPart](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPart.md).

-   Error message: The last partSize must same with init\_multipart setted.

    Cause: The total size of the uploaded parts is different from the total size specified when the multipart upload task is initialized.

    Solution: After the last part is uploaded, make sure that the total size of the uploaded parts is the same as the total size specified when the multipart upload task is initialized. For more information, see [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

-   Error message: The client encryption meta is inconsistent with init\_multipart setted.

    Cause: The encryption information about multipart upload configured on the client is different from the encryption information configured when the multipart upload task is initialized.

    Solution: Make sure that the encryption information about multipart upload configured on the client is the same as the encryption information configured when the multipart upload task is initialized. For more information, see [Client-side encryption](/intl.en-US/SDK Reference/Java/Data encryption/Client-side encryption.md).

-   Error message: Client encryption doesn't support upload part copy.

    Cause: UploadPartCopy cannot be used to upload a part from an object that is encrypted on the client.

    Solution: You can use UploadPartCopy to upload a part by copying data only from an existing object that is not encrypted on the client. For more information, see [UploadPartCopy](/intl.en-US/API Reference/Object operations/Multipart upload/UploadPartCopy.md).


## UserKeyMustBeSpecified

-   Error message: User key must be specified.
-   Cause: The request does not contain the name of the object that you want to delete.
-   Solution: Specify the name of the object that you want to delete in the request. For more information, see [DeleteMultipleObjects](/intl.en-US/API Reference/Object operations/Basic operations/DeleteMultipleObjects.md).

## InvalidTargetBucketForLogging

-   Error message: Put bucket log requester is not target bucket owner.
-   Cause: The destination bucket specified to store logs does not exist.
-   Solution: Specify a valid destination bucket.

## InvalidPart

-   Error message: One or more of the specified parts could not be found or the specified entity tag might not have matched the part's entity tag.
-   Cause: A part uploaded by CompleteMultipartUpload has an invalid PartNumber or ETag.
-   Solution: See [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

## InvalidPartOrder

-   Invalid partNumber order
    -   Error message: The list of parts was not in ascending order.
    -   Cause: The partNumbers of the parts submitted by CompleteMultipartUpload are not in ascending order.
    -   Solution: See [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).
-   Invalid partNumber value
    -   Error message: Part number must be an integer between 1 and 10000, inclusive.
    -   Cause: The value of partNumber is invalid. The valid values of partNumber ranges from 1 to 10000.
    -   Solution: See [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md).

## InvalidEncryptionAlgorithmError

-   Error message: The encryption algorithm specified is not valid.
-   Cause: The value of `x-oss-server-side-encryption` is invalid. Valid values: `AES256` and `KMS`.
-   Solution: See [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## InvalidTargetType

-   Error message: The symbolic's target file type is invalid.
-   Cause: The object is a symbolic link and the object to which the link points is also a symbolic link.
-   Solution: Make sure that the object to which the symbolic link points is not a symbolic link.

