# PutBucketCors

You can call this operation to configure cross-origin resource sharing \(CORS\) rules for a specific bucket.

## Usage notes

Take note of the following items when you call PutBucketCors:

-   Disabled-by-default design

    By default, CORS is disabled for a bucket. All cross-origin requests are not allowed.

-   Overwriting mechanism

    When you call PutBucketCors to configure a new CORS rule for a bucket for which a CORS rule is already configured, the existing rule is overwritten.

-   Usage in applications

    When you use CORS in applications, you must call PutBucketCors to configure a CORS rule to enable CORS.

    For example, if you want to use the `XMLHttpRequest` function of your browser to access OSS from the domain name `www.a.com`, you must call PutBucketCORS to configure a CORS rule that is described by an XML message body.

-   CORS rule matching

    When OSS receives a cross-origin request or an OPTIONS request sent for a bucket, OSS reads the CORS rules configured for the bucket and checks the permission of the request. OSS matches the rules one by one. When OSS finds the first match, OSS returns a corresponding header. If no match is found, OSS does not include any CORS header in the response.

    The following conditions must be met before OSS determines that a CORS rule matches the request:

    -   The origin from which the request is initiated must match the value of one `AllowedOrigin` element in the CORS rule.
    -   The method of the request such as GET or PUT or the method corresponding to the `Access-Control-Request-Method` header in an OPTIONS request must match the value of one `AllowedMethod` element in the CORS rule.
    -   Each header included in `Access-Control-Request-Headers` in an OPTIONS request must match the value of one `AllowedHeader` element in the CORS rule.

## Request structure

```
PUT /? cors HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
<? xml version="1.0" encoding="UTF-8"? >
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>the origin you want allow CORS request from</AllowedOrigin>
      <AllowedOrigin>… </AllowedOrigin>
      <AllowedMethod>HTTP method</AllowedMethod>
      <AllowedMethod>… </AllowedMethod>
        <AllowedHeader> headers that allowed browser to send</AllowedHeader>
          <AllowedHeader>… </AllowedHeader>
          <ExposeHeader> headers in response that can access from client app</ExposeHeader>
          <ExposeHeader>… </ExposeHeader>
          <MaxAgeSeconds>time to cache pre-fight response</MaxAgeSeconds>
    </CORSRule>
    <CORSRule>
      …
    </CORSRule>
…
</CORSConfiguration >
```

## Request elements

|Element|Type|Required|Example|Description|
|:------|:---|:-------|-------|:----------|
|CORSRule|Container|Yes|N/A|The container that stores CORS rules.Up to 10 CORS rules can be configured for a bucket. The XML message body in a request can be up to 16 KB in size.

Parent nodes: CORSConfiguration |
|AllowedOrigin|String|Yes|\*|The origins from which you want to allow cross-origin requests. -   You can use multiple elements to specify multiple allowed origins.
-   Each rule can contain up to one asterisk \(\*\) as the wildcard. If AllowedOrigin is set to an asterisk \(\*\), all cross-origin requests are allowed.

Parent nodes: CORSRule |
|AllowedMethod|Enumeration|Yes|GET|The cross-origin request methods that are allowed.Valid values: GET, PUT, DELETE, POST, and HEAD

Parent nodes: CORSRule |
|AllowedHeader|String|No|Authorization|Specifies whether the headers specified by `Access-Control-Request-Headers` in the OPTIONS preflight request are allowed.Each header specified by `Access-Control-Request-Headers` must match the value of an AllowedHeader element.

**Note:** Each rule can contain only one asterisk \(\*\) as the wildcard.

Parent nodes: CORSRule |
|ExposeHeader|String|No|x-oss-test|The response headers for allowed access requests from applications, such as a JavaScript XMLHttpRequest object.**Note:** Asterisks \(\*\) cannot be included as wildcards in the value of this parameter.

Parent nodes: CORSRule |
|MaxAgeSeconds|Integer|No|100|Specifies the period of time within which the browser can cache the response for an OPTIONS preflight request to specific resources. Unit: seconds. You can specify only one MaxAgeSeconds parameter in a CORS rule.

Parent nodes: CORSRule |
|CORSConfiguration|Container|Yes|N/A|The container that stores the CORS rule that you want to configure for the bucket.Parent nodes: none |
|ResponseVary|Boolean|No|false|Specifies whether to return the Vary: Origin header. Default value: false. Valid values:

-   true: The Vary: Origin header is returned no matter whether the request is a cross-origin request or whether the cross-origin request is successful.
-   false: The Vary: Origin header is not returned.

**Note:** This element is valid only when at least one CORS rule is configured. |

For more information about the common request headers included in a PutBucketCors request, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response to a PutBucketCors request contains only common response headers. For more information, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    ```
    PUT /? cors HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 186
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT*****
    <? xml version="1.0" encoding="UTF-8"? >
    <CORSConfiguration>
        <CORSRule>
          <AllowedOrigin>*</AllowedOrigin>
          <AllowedMethod>PUT</AllowedMethod>
          <AllowedMethod>GET</AllowedMethod>
          <AllowedHeader>Authorization</AllowedHeader>
        </CORSRule>
        <CORSRule>
          <AllowedOrigin>http://www.a.com</AllowedOrigin>
          <AllowedOrigin>http://www.b.com</AllowedOrigin>
          <AllowedMethod>GET</AllowedMethod>
          <AllowedHeader> Authorization</AllowedHeader>
          <ExposeHeader>x-oss-test</ExposeHeader>
          <ExposeHeader>x-oss-test1</ExposeHeader>
          <MaxAgeSeconds>100</MaxAgeSeconds>
        </CORSRule>
        <ResponseVary>false</ResponseVary>
    </CORSConfiguration >
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 50519080C4689A033D0*****
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

You can use OSS SDKs for the following programming languages to call the PutBucketCors operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/CORS.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/CORS.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/CORS.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/CORS.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/CORS.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/CORS.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/CORS.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/CORS.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/CORS.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidDigest|400|The error message returned because the Content-MD5 value of the message body is inconsistent with the Content-MD5 value in the request header.|

