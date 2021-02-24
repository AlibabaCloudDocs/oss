# Errors returned with HTTP status code 203

This topic describes the causes of errors returned with HTTP status code 203 and the solutions to these errors.

## CallbackFailed

-   Error message: Get image info failed.

    Cause: OSS failed to obtain the information about the image. The image may fail to be uploaded or is deleted.

    Solution:

    -   If the image failed to be uploaded, call the [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md) operation to upload the image again.
    -   Check whether the image is automatically deleted based on lifecycle rules or manually deleted by other authorized users.
-   Error message: Too many callback requests.

    Cause: Too many callback requests are being processed by OSS.

    Solution: Send the callback request later.

-   Error message: Cost too long time.

    Cause: OSS determines that the request times out because the callback server takes more than 5 seconds to process the request.

    Solution: To ensure that the callback server can process a request and return the result to OSS within 5 seconds, we recommend that you change the processing logic of the callback server to asynchronous.

-   Error message: Response body is not valid json format.

    Cause: The message body of the response returned by the callback server is not in the JSON format.

    Solution: See [Upload callback common errors and troubleshooting](https://www.alibabacloud.com/help/doc-detail/50092.htm) to handle the problem.

-   Error message: Error Status : 400.User server return too long content-length value.

    Cause: The response returned by the application server to OSS does not contain the Content-Length header. The size of the response body is larger than 1 MB.

    Solution: Make sure that the response returned by the application server to OSS contain the Content-Length header and the size of the response body does not exceed 1 MB.

    For example, the following sample response contains the Content-Length header. The body of the response is `{"a":"b"}`, which does not exceed 1 MB in size.

    ```
    HTTP/1.0 200 OK
    Server: BaseHTTP/0.3 Python/2.7.6
    Date: Mon, 14 Sep 2015 12:37:27 GMT
    Content-Type: application/json
    Content-Length: 9
    {"a":"b"}
    ```

-   Error message: Error Status : -1.OSS can not connect to your callbackUrl, please check it.

    Cause: OSS cannot access your application server.

    Solution: Check whether your application server runs and communicates with OSS normally.

-   Error message: Error Status : 400.User server missing content-length.

    Cause: The response returned by the application server does not contain the Content-Length header.

    Solution: Make sure that the response returned by the application server contain the Content-Length header.

-   Error message: Error Status : 400.User server return invalid conent-length value.

    Cause: The response returned by the application server does not contain the Content-Length header, or the value of Content-Length is not a positive integer.

    Solution: Ensure that the response returned by the application server contain the Content-Length header and the value of Content-Length is a positive integer.

    For example, the following sample response contains the Content-Length header, and the value of Content-Length is `9`, which is a positive integer.

    ```
    HTTP/1.1 200 OK
    Date: Mon, 14 Sep 2015 12:37:27 GMT
    Content-Type: application/json
    Content-Length: 9
    Connection: keep-alive
    ETag: "D8E8FCA2DC0F896FD7CB4CB0031B****"
    Server: AliyunOSS
    x-oss-bucket-version: 1442231779
    x-oss-request-id: 55F6BF87207FB30F2640****
    {"a":"b"}
    ```


