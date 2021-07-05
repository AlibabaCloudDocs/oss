# Error responses

If an error occurs when you access Object Storage Service \(OSS\), OSS returns an HTTP status code and a response that contains a message body in the application/xml format. You can identify and rectify the error based on the returned HTTP status code and response.

## Response message body

The following example shows the message body of an error response:

```
<?xml version="1.0" ?>
<Error xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <Code>
        AccessDenied
    </Code>
    <Message>
        Query-string authentication requires the Signature, Expires and OSSAccessKeyId parameters
    </Message>
    <RequestId>
        1D842BC54255****
    </RequestId>
    <HostId>
        oss-cn-hangzhou.aliyuncs.com
    </HostId>
</Error>
```

The message body of an error response contains the following elements:

-   `Code`: the error code returned by OSS.
-   `Message`: the detailed error message provided by OSS.
-   `RequestId`: the UUID that uniquely identifies the error request. You can provide technical support the request ID to locate and resolve your problems. For more information about how to obtain the ID of a request, see [Obtain request IDs](/intl.en-US/Troubleshooting/Obtain request IDs.md).
-   `HostId`: the ID of the host in the accessed OSS cluster, which is the same as the host ID specified in the request.

## Sample error request and response

If parameters that are not supported by OSS are added to requests to call operations supported by OSS \(for example, the If-Modified-Since parameter is added to the PUT operation request\), OSS returns `400 Bad Request`.

Sample error requests

```
PUT /abc.zip HTTP/1.1
Host: bucketname.oss-cn-shanghai.aliyuncs.com
Accept: */*
Date: Thu, 11 Aug 2019 01:44:50 GMT
If-Modified-Since: Thu, 11 Aug 2019 01:43:51 GMT
Content-Length: 363
```

Sample responses

```
HTTP/1.1 400 Bad Request
Server: AliyunOSS
Date: Thu, 11 Aug 2019 01:44:54 GMT
Content-Type: application/xml
Content-Length: 322
Connection: keep-alive
x-oss-request-id: 57ABD896CCB80C366955****
x-oss-server-time: 0
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>NotImplemented</Code>
  <Message>A header you provided implies functionality that is not implemented.</Message>
  <RequestId>57ABD896CCB80C366955****</RequestId>
  <HostId>bucketname.oss-cn-shanghai.aliyuncs.com</HostId>
  <Header>If-Modified-Since</Header>
</Error>
```

For more information about OSS error codes, visit [OSS error center](https://error-center.alibabacloud.com/status/product/Oss).

## Common errors and troubleshooting

-   [HTTP 203 status code](/intl.en-US/Troubleshooting/HTTP 203 status code.md)
-   [HTTP 400 status code](/intl.en-US/Troubleshooting/HTTP 400 status code.md)
-   [HTTP 403 status code](/intl.en-US/Troubleshooting/HTTP 403 status code.md)
-   [HTTP 404 status code](/intl.en-US/Troubleshooting/HTTP 404 status code.md)
-   [HTTP 405 status code](/intl.en-US/Troubleshooting/HTTP 405 status code.md)
-   [HTTP 409 status code](/intl.en-US/Troubleshooting/HTTP 409 status code.md)
-   [HTTP 411 status code](/intl.en-US/Troubleshooting/HTTP 411 status code.md)
-   [HTTP 412 status code](/intl.en-US/Troubleshooting/HTTP 412 status code.md)
-   [HTTP 416 status code](/intl.en-US/Troubleshooting/HTTP 416 status code.md)
-   [HTTP 424 status code](/intl.en-US/Troubleshooting/HTTP 424 status code.md)
-   [HTTP 500 status code](/intl.en-US/Troubleshooting/HTTP 500 status code.md)
-   [HTTP 503 status code](/intl.en-US/Troubleshooting/HTTP 503 status code.md)

## Reference

For more information about troubleshooting, see the FAQ topics for OSS features and tools. You can view the following FAQ topics for OSS SDKs for different programming languages and OSS tools for troubleshooting:

-   [Java](/intl.en-US/SDK Reference/Java/FAQ.md)
-   [Python](/intl.en-US/SDK Reference/Python/FAQ.md)
-   [Go](/intl.en-US/SDK Reference/Go/Error handling.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Exception handling.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/FAQ.md)
-   [ossbrowser](/intl.en-US/Tools/ossbrowser/Troubleshooting.md)
-   [ossutil](/intl.en-US/Tools/ossutil/FAQ.md)
-   [ossfs](/intl.en-US/Tools/ossfs/FAQ.md)
-   [ossftp](/intl.en-US/Tools/ossftp/FAQ.md)

