# Error handling

OSS SDK for Java exceptions are classified into two types: OSSException and ClientException. Both are subclasses of RuntimeException.

## Example of handling exceptions

The following code provides an example on how to handle exceptions:

```
try {
    // Perform operations on OSS such as object upload.
    ossClient.putObject(...);
} catch (OSSException oe) {
    System.out.println("Caught an OSSException, which means your request made it to OSS, "
            + "but was rejected with an error response for some reason.") ;
    System.out.println("Error Message: " + oe.getErrorMessage());
    System.out.println("Error Code:       " + oe.getErrorCode());
    System.out.println("Request ID:      " + oe.getRequestId());
    System.out.println("Host ID:           " + oe.getHostId());
} catch (ClientException ce) {
    System.out.println("Caught an ClientException, which means the client encountered "
            + "a serious internal problem while trying to communicate with OSS, "
            + "such as not being able to access the network.") ;
    System.out.println("Error Message: " + ce.getMessage());
} finally {
    if (ossClient ! = null) {
        ossClient.shutdown();
    }
}
			
```

## ClientException

A ClientException indicates an exception that occurs when a client sends a request or transmit data to OSS. For example, a ClientException is returned when a request is sent under poor network conditions. A ClientException is also returned when an I/O exception occurs during object upload.

## OSSException

OTSException: indicates a server exception that arises from the resolution of a server error message. A ServiceException includes the error code and message returned by OSS so that you can identify and resolve the error.

OSSException involves the following error information:

|Parameter|Description|
|:--------|:----------|
|Code|The error code returned by OSS.|
|Message|The detailed error message returned by OSS.|
|RequestId|The UUID used to uniquely identify the request. If the problem persists, you can provide the request ID of a problem to the OSS development engineers for help.|
|HostId|The ID of the host in the accessed OSS cluster, which is the same as the host ID specified in the request.|

## Common OSS error codes

|Error code|Description|HTTP status code|
|:---------|:----------|:---------------|
|AccessDenied|The error message returned because access is denied.|403|
|BucketAlreadyExists|The error message returned when the bucket already exists.|409|
|BucketNotEmpty|The error message returned when the bucket is not empty.|409|
|EntityTooLarge|The error message returned because the entity is too large.|400|
|EntityTooSmall|The error message returned because the entity is too small.|400|
|FileGroupTooLarge|The error message returned because the object group is too large.|400|
|FilePartNotExist|The error message returned when the part does not exist.|400|
|FilePartStale|The error message returned when the part is expired.|400|
|InvalidArgument|The error message returned when the parameter format is invalid.|400|
|InvalidAccessKeyId|The error message returned because AccessKey ID does not exist.|403|
|InvalidBucketName|The error message returned when the bucket name is invalid.|400|
|InvalidDigest|The error message returned when the digest is invalid.|400|
|InvalidObjectName|The error message returned when the object name is invalid.|400|
|InvalidPart|The error message returned when the part is invalid.|400|
|InvalidPartOrder|The error message returned when the part order is invalid.|400|
|InvalidTargetBucketForLogging|The error message returned because the target bucket specified for logging is invalid.|400|
|InternalError|The error message returned when an internal OSS error occurs.|500|
|MalformedXML|The error message returned when the XML format is invalid.|400|
|MethodNotAllowed|The error message returned when the method is not supported.|405|
|MissingArgument|The error message returned when a parameter is missing.|411|
|MissingContentLength|The error message returned when the content length is missing.|411|
|NoSuchBucket|The error message returned when no such bucket exists.|404|
|NoSuchKey|The error message returned because no such object exists.|404|
|NoSuchUpload|The error message returned when no such multipart upload ID exists.|404|
|NotImplemented|The error message returned when the method cannot be implemented.|501|
|PreconditionFailed|The error message returned when a precondition error occurs.|412|
|RequestTimeTooSkewed|The error message returned when the time deviation of the OSS client and OSS server exceeds 15 minutes.|403|
|RequestTimeout|The error message returned because the request has timed out.|400|
|SignatureDoesNotMatch|The error message returned when a signature error occurs.|403|
|InvalidEncryptionAlgorithmError|The error message returned because the specified encryption algorithm based on entropy encoding is invalid.|400|

