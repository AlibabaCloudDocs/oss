# HTTP 411 status code

This topic describes the types of error messages returned with HTTP status code 411, and the common causes and solutions to these errors.

## MissingContentLength

-   Error message: You must provide the Content-Length HTTP header.
-   Cause: The request does not use chunked encoding or does not contain the `Content-Length` header.
-   Solution: Use [chunked encoding](https://tools.ietf.org/html/rfc2616#section-3.6.1) to encode request headers or include the [Content-Length](/intl.en-US/API Reference/Common HTTP headers.md) header in the request.

## ObjectNotAppendable

-   Error message: The object is not appendable.
-   Cause: You have performed the AppendObject operation on an object that is not uploaded by using append upload.
-   Solution: Check the type of the object on which you want to perform the AppendObject operation. OSS objects can be categorized into three types based on the upload method: normal objects, append objects, and multipart objects. The AppendObject operation can be performed only on append objects. You can call the [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md) operation to obtain the type of an object.

