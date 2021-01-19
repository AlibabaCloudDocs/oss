# PostObject

You can call this operation to upload an object to a specified bucket by using an HTML form.

## Usage notes

When you call the PostObject operation, take note of the following items:

-   To initiate a PostObject request to a bucket, you must have write permissions on the bucket. If the ACL of the bucket is public read/write, you do not need to upload the signature information. If the ACL of the bucket is not public read/write, signature verification is required for this operation.
-   Unlike PutObject, PostObject uses an AccessKey secret to calculate the signature for policy. The calculated signature string is used as the value of the Signature form field. OSS checks this value to verify the validity of the signature.
-   The URL of the submitted form can be the domain name of the bucket. You do not need to specify the object in the URL. In other words, the request line is in the format of `POST / HTTP/1.1` instead of `POST /ObjectName HTTP/1.1`.
-   OSS does not check the signature included in headers or URLs in PostObject requests.

## Versioning

If you initiate a PostObject request to a versioned bucket, OSS generates a unique version ID for the uploaded object and returns the version ID as the x-oss-version-id header in the response.

If you initiate a PostObject request to a bucket with versioning suspended, OSS generates a version ID null for the uploaded object and returns the version ID as the x-oss-version-id header in the response. An object can have only one version whose version ID is null.

## Request structure

```
POST / HTTP/1.1 
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
User-Agent: browser_data
Content-Length: ContentLength
Content-Type: multipart/form-data; boundary=9431149156168
--9431149156168
Content-Disposition: form-data; name="key"
key
--9431149156168
Content-Disposition: form-data; name="success_action_redirect"
success_redirect
--9431149156168
Content-Disposition: form-data; name="Content-Disposition"
attachment;filename=oss_download.jpg
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-uuid"
myuuid
--9431149156168
Content-Disposition: form-data; name="x-oss-meta-tag"
mytag
--9431149156168
Content-Disposition: form-data; name="OSSAccessKeyId"
access-key-id
--9431149156168
Content-Disposition: form-data; name="policy"
encoded_policy
--9431149156168
Content-Disposition: form-data; name="Signature"
signature
--9431149156168
Content-Disposition: form-data; name="file"; filename="MyFilename.jpg"
Content-Type: image/jpeg
file_content
--9431149156168
Content-Disposition: form-data; name="submit"
Upload to OSS
--9431149156168--
```

## Request headers

**Note:** The message body of a PostObject request is encoded in the multipart/form-data format. In PostObject operations, parameters are passed as form fields in the request message body, whereas parameters are passed as HTTP request headers in PutObject operations.

|Header|Type|Required|Description|
|:-----|:---|--------|:----------|
|OSSAccessKeyId|String|Conditional|The AccessKey ID of the bucket owner. Default value: null

Constraint: This form field is required when the bucket ACL is not public read/write or when the policy or Signature form field is provided. |
|policy|String|Conditional|Specifies the validity of the form fields in the request. A request that does not contain the policy form field is considered as an anonymous request, and can only be used to access buckets whose ACLs are public read/write. Default value: null

Constraint: This form field is required when the bucket ACL is not public read/write or when the OSSAccessKeyId or Signature form field is provided.

**Note:** The form and the policy form field must be encoded in UTF-8. |
|Signature|String|Conditional|Specifies the signature information that is calculated based on the AccessKey secret and the policy form field. OSS checks the signature information to verify the validity of the PostObject request. For more information, see [Appendix: Signature](#section_wny_mww_wdb). Default value: null

Constraint: This form field is required when the bucket ACL is not public read/write or when the OSSAccessKeyId or policy form field is provided.

**Note:** Form fields are case-insensitive, but their values are case-sensitive. |
|Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|String|No|HTTP request headers. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md). Default value: null

The form submitted by the PostObject operation must be encoded in the `multipart/form-data` format. For example, the Content-Type header must be in the `multipart/form-data;boundary=xxxxxx` format, where boundary is a boundary string. |
|x-oss-content-type|String|No|You can add the x-oss-content-type header in the message body of a PostObject request to specify the Content-Type of the object to upload. The Content-Type value specified by the x-oss-content-type header has the highest priority.The content type of the object is determined by three headers in the following priority: x-oss-content-type \> Content-Type in the form field \> Content-Type.

Default value: null |
|key|String|Yes|The name of the object to upload. If the object name includes a path such as `destfolder/example.jpg`, OSS automatically creates corresponding folders. Default value: null |
|success\_action\_redirect|String|No|The URL to which the client is redirected after the object is uploaded. If this form field is not specified, the returned result is specified by success\_action\_status. If the upload fails, OSS returns an error code, and the client is not redirected to a URL. Default value: null |
|success\_action\_status|String|No|The status code returned to the client when the success\_action\_redirect form field is not specified and the object is uploaded. Default value: 204

Valid values: 200, 201, and 204.

-   If the value of this form field is set to 200 or 204, OSS returns an empty file and the 200 or 204 status code.
-   If the value of this form field is set to 201, OSS returns an XML file and the 201 status code.
-   If the value of this form field is not specified or set to an invalid value, OSS returns an empty file and the 204 status code. |
|x-oss-meta-\*|String|No|The metadata customized by the user. Default value: null

If the request contains a form field prefixed with x-oss-meta-, the form field is considered to be the user metadata. Example: `x-oss-meta-location`.

**Note:** An object may have multiple similar parameters prefixed with x-oss-meta-, but the total size of the user metadata cannot exceed 8 KB. |
|x-oss-server-side-encryption|String|No|The server-side encryption algorithm that is used when OSS creates the object. Valid values: AES256 and KMS

If you specify this parameter, it is returned in the response header and the uploaded object is encrypted and stored. When you download the encrypted object, the x-oss-server-side-encryption field is included in the response header and its value is set to the algorithm used to encrypt the object. |
|x-oss-server-side-encryption-key-id|String|No|The ID of the customer master key \(CMK\) hosted in KMS. This header is valid only when x-oss-server-side-encryption is set to KMS.|
|x-oss-object-acl|String|No|The ACL of the object when the object is created. Valid values: public-read, private, and public-read-write |
|x-oss-security-token|String|No|If an STS temporary security credential is used for this access, you must set this field to the SecurityToken value and set OSSAccessKeyId to the value of the paired temporary AccessKey ID. The method for calculating a signature based on a temporary AccessKey ID is the same as that based on a typical AccessKey ID. Default value: null |
|x-oss-forbid-overwrite|String|No|Specifies whether the PostObject operation overwrites objects of the same name. When the versioning status of the requested bucket is enabled or suspended, the x-oss-forbid-overwrite request header is invalid. In this case, the PostObject operation overwrites objects of the same name.-   By default, if x-oss-forbid-overwrite is not specified, the object that has the same name is overwritten.
-   If the value of x-oss-forbid-overwrite is set to true, objects of the same name cannot be overwritten. If the value of x-oss-forbid-overwrite is set to false, objects of the same name can be overwritten.

If you specify the x-oss-forbid-overwrite request header, the queries per second \(QPS\) performance of OSS may be degraded. If you want to use the x-oss-forbid-overwrite request header to perform a large number of operations \(QPS greater than 1000\), submit a ticket. |
|file|String|Yes|The file or text content. Browsers automatically set the Content-Type header based on the file type and overwrite the user setting. Only one file can be uploaded to OSS at a time. Default value: null

**Note:** This header must be the last field in the form. |

## Response headers

|Header|Type|Description|
|:-----|:---|:----------|
|x-oss-server-side-encryption|String|If x-oss-server-side-encryption is specified in the request, the response contains this header to indicate the encryption algorithm used to encrypt the object at the server side.|

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|PostResponse|Container|Indicates the container that stores the result of the PostObject request. Child nodes: Bucket, ETag, Key, and Location |
|Bucket|String|Indicates the bucket name. Parent node: PostResponse |
|ETag|String|Indicates the entity tag \(ETag\) that is created when an object is generated. For an object that is created by using the PostObject operation, the ETag value is the object UUID, which can be used to check whether the object content is changed. Parent node: PostResponse |
|Location|String|Indicates the URL of the new object that is created. Parent node: PostResponse |

## Examples

-   Sample requests

    ```
    POST / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 344606
    Content-Type: multipart/form-data; boundary=9431149156168
    --9431149156168
    Content-Disposition: form-data; name="key"
    /user/a/objectName.txt
    --9431149156168
    Content-Disposition: form-data; name="success_action_status"
    200
    --9431149156168
    Content-Disposition: form-data; name="Content-Disposition"
    content_disposition
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-uuid"
    uuid
    --9431149156168
    Content-Disposition: form-data; name="x-oss-meta-tag"
    metadata
    --9431149156168
    Content-Disposition: form-data; name="OSSAccessKeyId"
    44CF9590006BF252F707
    --9431149156168
    Content-Disposition: form-data; name="policy"
    eyJleHBpcmF0aW9uIjoiMjAxMy0xMi0wMVQxMjowMDowMFoiLCJjb25kaXRpb25zIjpbWyJjb250ZW50LWxlbmd0aC1yYW5nZSIsIDAsIDEwNDg1NzYwXSx7ImJ1Y2tldCI6ImFoYWhhIn0sIHsiQSI6ICJhIn0seyJrZXkiOiAiQUJDIn1dfQ==
    --9431149156168
    Content-Disposition: form-data; name="Signature"
    kZoYNv66bsmc10+dcGKw5x2PRrk=
    --9431149156168
    Content-Disposition: form-data; name="file"; filename="MyFilename.txt"
    Content-Type: text/plain
    abcdefg
    --9431149156168
    Content-Disposition: form-data; name="submit"
    Upload to OSS
    --9431149156168--
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 61d2042d-1b68-6708-5906-33d81921362e 
    Date: Fri, 24 Feb 2014 06:03:28 GMT
    ETag: "5B3C1A2E053D763E1B002CC607C5****"
    Connection: keep-alive
    Content-Length: 0  
    Server: AliyunOSS
    ```


## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|InvalidArgument|400|The error message returned because one of the OSSAccessKeyId, policy, and Signature form fields is specified and the remaining two form fields are not specified. Regardless of whether the bucket ACL is public read/write, if one of the OSSAccessKeyId, policy, and Signature form fields is specified, the remaining two form fields must be specified.|
|InvalidDigest|400|The error message returned because the Content-MD5 value of the request body that is calculated by OSS is not the same as the value of the Content-MD5 header field in the request.|
|EntityTooLarge|400|The error message returned because the total size of the PostObject request body is greater than 5 GB.|
|InvalidEncryptionAlgorithmError|400|The error message returned because the x-oss-server-side-encryption header field is set to a valueother than AES256 or KMS .|
|IncorrectNumberOfFilesInPOSTRequest|400|The error message returned because the key form field is not specified in the request.|
|FileAlreadyExists|409|The error message returned because the request contains the x-oss-forbid-overwrite=true header and an object of the same name already exists.|
|KmsServiceNotEnabled|403|The error message returned because x-oss-server-side-encryption is set to KMS but KMS is not activated in advance.|
|FileImmutable|409|The error message returned the object you want to delete or modify is protected by a retention policy.|

## Appendix: Policy

The policy form field in a PostObject request is used to verify the validity of the request. The value of the policy form field is a piece of JSON text encoded in UTF-8 and Base64. This value declares the conditions that a PostObject request must meet. The policy form field is optional when you upload objects to a bucket whose ACL is public read/write. However, we recommend that you specify this field to limit PostObject requests.

Example

```
{ "expiration": "2014-12-01T12:00:00.000Z",
  "conditions": [
    {"bucket": "johnsmith" },
    ["starts-with", "$key", "user/eric/"]
  ]
}
```

The policy form field must contain the expiration and conditions parameters.

-   Expiration

    The expiration parameter specifies the expiration time of the request. The time follows the ISO 8601 standard and must be in GMT. For example, `2014-12-01T12:00:00.000Z` indicates that the PostObject request must be sent before 12:00 on December 1, 2014.

-   Conditions

    The conditions parameter is a list that specifies the valid values of form fields in the PostObject request.

    **Note:** The value of a form field is extended after OSS checks the policy. Therefore, the valid value of the form field set in policy is equivalent to the value of the form field before extension.

    The following table lists the conditions supported by the policy.

    |Condition|Description|
    |:--------|:----------|
    |content-length-range|The minimum and maximum sizes of the object to upload. This condition supports the content-length-range matching mode.|
    |Cache-Control, Content-Type, Content-Disposition, Content-Encoding, Expires|HTTP request headers. This condition supports the exact matching and starts-with matching modes. **Note:** To prevent malicious modification to the Content-Type header during form upload, we recommend that you include the Content-Type parameter in the policy form field. |
    |key|The name of the object to upload. This condition supports the exact matching and starts-with matching modes.|
    |success\_action\_redirect|The URL to which the client is redirected after the object is uploaded. This condition supports the exact matching and starts-with matching modes.|
    |success\_action\_status|The status code to return after the object is uploaded if success\_action\_redirect is not specified. This condition supports the exact matching and starts-with matching modes.|
    |x-oss-meta-\*|The metadata customized by the user. This condition supports the exact matching and starts-with matching modes.|

    If the PostObject request contains additional form fields, OSS adds these form fields to the conditions of the policy and checks their validity.

-   Condition matching modes

    |Matching mode|Description|
    |:------------|:----------|
    |Exact match|The value of a form field must be exactly the same as the value that is declared in conditions. For example, if the value of the key form field must be a, the condition can be \{"key": "a"\} or \["eq", "$key", "a"\].|
    |Starts With|The value of a form field must start with the specified value. For example, if the value of the key form field must start with /user/user1, the condition must be \["starts-with", "$key", "/user/user1"\].|
    |Specified object size|Specifies the range of acceptable object sizes. For example, if the acceptable object size is 1 to 10 bytes, the condition must be \["content-length-range", 1, 10\].|


In the policy form field of a PostObject request, the dollar sign \($\) is used to indicate a variable. To describe the dollar sign \($\), you must use the following escape character: \\$. The following table describes the escape characters used in the JSON string of the policy form field in a PostObject request.

|Escape character|Description|
|:---------------|:----------|
|\\/|Forward slash|
|\\\\|Backslash|
|\\"|Double quotation marks|
|\\$|Dollar sign|
|\\b|Space|
|\\f|Form feed|
|\\n|Line feed|
|\\r|Carriage return|
|\\t|Horizontal tab|
|\\uxxxx|Unicode character|

## Appendix: Signature

To verify a PostObject request, you must include the policy and Signature form fields in the HTML form. The policy form field specifies the values that are acceptable in the request.

You can perform the following steps to calculate the value of the Signature form field:

1.  Create a UTF-8 encoded policy.
2.  Encode the policy in Base64. The encoding result is the value of the policy form field, and this value is used as the string-to-sign.
3.  Use the AccessKey secret to sign the string in the following format: `Signature = base64(hmac-sha1(base64(policy), AccessKeySecret))`.

## References

-   For the demo of transferring data from the web client to OSS by using form upload, see [Add signatures on the client by using JavaScript and upload data to OSS](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the client by using JavaScript and upload data to OSS.md).
-   For demos of how to use OSS SDK for Java to call the PostObject operation, see [Form upload](/intl.en-US/SDK Reference/Java/Upload objects/Form upload.md).

