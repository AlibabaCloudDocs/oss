# Errors returned with HTTP status code 403

This topic describes the causes of errors returned with HTTP status code 403 and the solutions to these errors.

## AccessDenied

-   Error message: AccessDenied
-   The error message returned because you do not have access permissions.
-   Solution: See [OSS permission]() to find detailed solutions.

## InvalidAccessKeyId

-   Error message
    -   The OSS Access Key Id you provided is disabled.
    -   The OSS Access Key Id you provided does not exist in our records.
    -   The OSS Access Key Id contains non-acceptable characters, which accepts only alphanumeric characters\[0-9a-zA-Z\] and several special characters\[. \_=\]
-   Cause: The error message returned because the AccessKey ID does not exist or is invalid.
-   Solution: Log on to the Alibaba Cloud Management Console. Move the pointer over your profile picture, and then click [AccessKey](https://ak-console.aliyun.com). Confirm that the AccessKey ID that you use to access OSS exists and is enabled.

    If your AccessKey ID is disabled, enable it. If you do not have an AccessKey ID, create an AccessKey ID and use it to access OSS.


## SignatureDoesNotMatch

-   Error message: The request signature we calculated does not match the signature you provided.
-   Cause: The error message returned because a signature error has occurred.
-   Solution: See [FAQ](/intl.en-US/Developer Guide/Data security/Signature/FAQ.md) to handle the problem.

## RequestTimeTooSkewed

-   Error message: The difference between the request time and the current time is too large.
-   Cause: The error message returned because the time when the request is initiated is at least 15 minutes before the current time of the OSS server.
-   Solution: Check the system time of the device used to send the request, and adjust the system time based on your time zone.

    You can adjust the system time of the device used to send the request in the following method:

    -   OSS uses Greenwich Mean Time \(GMT\) as the system time. Therefore, the system time of your device must be adjusted to GMT or to a zone time corresponding to GMT. GMT is the zone time of GMT+00:00, that is, the Universal Time.
        -   To check the time zone in which your device is located in Windows, click **Control Panel** \> **Clock, Language, and Region** \> **Set Date and Time**.

            For example, +08:00 displayed in the Time Zone column indicates that your device is located in the time zone GMT+08:00.

        -   To check the time zone in which your device is located in Linux or Unix, run the data-R command.

            In the following example figure, +0800 indicates that the time zone of the device is GMT+08:00

            ![+0800](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3106793161/p171537.png)

    -   You can use OSS in multiple regions. OSS uses GMT as the system time in all regions. Therefore the system time of your device used to send requests must also be GMT.

## ImageDamage

-   Error message: The image file may be damaged.
-   Cause: The error returned because the image is missing data or damaged such that the image cannot be recognized or processed.
-   Solution: Ensure that the image is not damaged. If the image is damaged, reupload the image.

## UserDisable

-   Error message: UserDisable
-   Cause: The error message returned because the account is deactivated due to overdue payment or security reasons.
-   Solution: Check whether you have any overdue payments or contact technical support to perform a security check.

## AccessForbidden

-   Error message: CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.
-   Cause: The error message returned because CORS is not configured or the CORS configuration is incorrect.
-   Solution: See [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md) to handle the problem.

## InvalidObjectState

-   Error message: The operation is not valid for the object's state.
-   Cause: When you download an Archive object, the state of the object is invalid in the following two scenarios:
    -   The RestoreObject request for the object timed out or was not initiated.
    -   The RestoreObject request for the object was initiated but the object was not restored.
-   Solution: See [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md) to handle the problem.

