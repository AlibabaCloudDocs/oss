# HTTP 412 status code

This topic describes the causes of errors returned with HTTP status code 412 and the solutions to these errors.

## PreconditionFailed

-   Error message: At least one of the pre-conditions you specified did not hold.
-   Cause: The error message returned because the object to download does not meet the specified conditions. You may encounter this error for the following reasons:
    -   The If-Unmodified-Since header is specified in the request, but the specified time is earlier than the modified time of the object to download.
    -   The If-Match header is specified in the request, but the ETag value provided in the request is different from the ETag value of the object to download.
-   Solution:
    -   If you specify the If-Unmodified-Since header in the request, ensure that the value of this header is equal to or later than the modified time of the object to download.
    -   If you specify the If-Match header in the request, ensure that the ETag value provided in the request is the same as the ETag value of the object to download.

