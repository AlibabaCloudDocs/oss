# Common errors

If an error occurs when you access Image Processing \(IMG\), IMG returns an error code and an error message. This helps you locate and fix the error.

## Error response format

The following example shows an error response:

```
<Error>
  <Code>BadRequest</Code>
  <Message>Input is not base64 decoding.</Message>
  <RequestId>52B155D2D8BD99A15D0005FF</RequestId>
  <HostId>userdomain</HostId>
</Error>
```

The error response contains the following elements:

-   `Code`: the error code that IMG returns to the user.
-   `Message`: the detailed error information provided by IMG.
-   `RequestId`: the UUID used to identify an error request. If the problem persists, you can send this UUID to technical support to help locate the cause of the error.
-   `HostId`: the ID used to identify the accessed IMG cluster.

## Error codes

The following table describes the error codes contained in responses to IMG requests.

|Error code|Description|Solution|
|----------|-----------|--------|
|InvalidArgument|The error message returned because the parameter is invalid.|[HTTP status code 400](/intl.en-US/Developer Guide/Troubleshooting/HTTP status code 400.md)|
|BadRequest|The error message returned because a request error occurs.|
|MissingArgument|The error message returned because a required parameter is not specified.|
|ImageTooLarge|The error message returned because the image size exceeds the limit.|
|WatermarkError|The error message returned because a watermark error occurs.|
|NotImplemented|The error message returned because the access is denied.|
|AccessDenied|The error message returned because access is denied.|[HTTP 403 status code](/intl.en-US/Developer Guide/Troubleshooting/HTTP 403 status code.md)|
|SignatureDoesNotMatch|The error message returned because the signature calculated by OSS does not match the signature provided in the request.|
|NoSuchKey|The error message returned because the specified image does not exist.|[HTTP 404 status code](/intl.en-US/Developer Guide/Troubleshooting/HTTP 404 status code.md)|
|NoSuchStyle|The error message returned because the specified style does not exist.|
|InternalError|The error message returned because an internal error occurs.|[HTTP 500 status code](/intl.en-US/Developer Guide/Troubleshooting/HTTP 500 status code.md)|

## SDK demos

-   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/IMG.md)
-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/IMG.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/IMG.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/IMG.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/IMG.md)
-   [OSS SDK for C](/intl.en-US/SDK Reference/C/IMG.md)
-   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/IMG.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/IMG.md)
-   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Image processing.md)
-   [OSS SDK for Android](/intl.en-US/SDK Reference/Android/IMG.md)

