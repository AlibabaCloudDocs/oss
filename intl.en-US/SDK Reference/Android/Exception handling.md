# Exception handling

OSS SDK for Android has two types of exceptions: ClientException and ServiceException.

## ClientException

A ClientException indicates an exception that occurs when a client sends a request or transmit data to OSS. For example, a ClientException is returned when a request is sent under poor network conditions. A ClientException is also returned when an I/O exception occurs during object upload.

## ServiceException

A ServiceException indicates a server error that is resolved from a server error message. A ServiceException includes the error code and message returned by OSS so that you can identify and resolve the error.

The following table describes the information included in a ServiceException.

|Parameter|Description|
|:--------|:----------|
|Code|The error code returned by OSS.|
|Message|The detailed error message returned by OSS.|
|RequestId|The UUID used to uniquely identify the request. If you need help from OSS development engineers to handle an exception, provide them with the RequestId value.|
|HostId|The ID of the host in the accessed OSS cluster, which is the same as the host ID specified in the request.|
|rawMessage|The raw text of the message body in the HTTP response.|

## Common error codes

For more information about the common error codes of OSS, see [Error responses](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).

