# HTTP 416 status code

This topic describes the types of error messages returned with HTTP status code 416, and the common causes and solutions to these errors.

## InvalidRange

-   Error message: The requested range cannot be satisfied.
-   Cause: The Range header specified in the request to download part of a large object is invalid.
-   Solution: Specify only one range in the request. OSS does not support GetObject requests in which multiple ranges are specified. ByteRange specifies the range of data that you request in bytes. The valid values of ByteRange range from 0 to `object size - 1`. The following example describes the download behavior when you set different ranges to download part of an object that is 2,000 bytes in size:
    -   `Range: bytes=0-499`: Data that ranges from byte 1 to byte 500 is downloaded.
    -   `Range: bytes=-500`: Data ranges from byte 1501 to byte 2000 is downloaded.
    -   `Range: bytes=500-`: Data ranges from byte 501 to byte 2000 is downloaded.
    -   `Range: bytes=0-`: Data ranges from byte 1 to byte 2000 is downloaded.

