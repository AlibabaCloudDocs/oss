# HTTP 403 status code

This topic describes the causes of errors returned with HTTP status code 403 and the solutions to these errors.

## AccessDenied

-   Error message: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.

    Cause: The endpoint used to access the bucket is incorrect.

    Solution: Make sure that you use the correct endpoint to access the bucket. For example, if you want to access a bucket in the China \(Hangzhou\) region \(`oss-cn-hangzhou`\), use the public endpoint of the region: `oss-cn-hangzhou.aliyuncs.com`. For more information about endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).

-   Error message: This request is forbidden by kms.

    Cause: You do not have permissions to use Key Management Service \(KMS\).

    Solution: Make sure that you have permissions to use the specified CMK. For more information, see [Server-side encryption](/intl.en-US/Developer Guide/Data security/Data encryption/Server-side encryption.md).

-   Error message: AccessDenied.

    Cause: You do not have permissions to access the resources.

    Solution:

    -   Make sure that you use the correct AccessKey ID and AccessKey secret to access the resources. For more information, see [Create an AccessKey pair for a RAM user](/intl.en-US/Security Settings/AccessKey pairs/Create an AccessKey pair for a RAM user.md).
    -   Make sure that the RAM user has permissions to perform operations on the bucket or object that you want to manage.
-   Error message: You have no right to access this object.

    Cause: The RAM user that you use does not have permissions to access the object.

    Solution: Make sure that the RAM user that you use has object-related operation permissions. For more information about how to configure access permissions in different scenarios, see [Tutorial: Use RAM policies to control access to OSS](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Tutorial: Use RAM policies to control access to OSS.md).

-   Error message: Anonymous user has no right to access this bucket.

    Cause: Anonymous users do not have permissions to access the bucket.

    Solution: Configure a bucket policy that grants anonymous users permissions to access the bucket. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

-   Error message: Anonymous user has no right to access this object.

    Cause: Anonymous users do not have permissions to access the object.

    Solution: Configure a bucket policy that grants anonymous users permissions to access specific resources in the bucket. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

-   Error message: You are denied by bucket referer policy.

    Cause: The domain name used to access the resource is not included in the Referer whitelist.

    Solution: Configure a Referer whitelist for a bucket and select whether to allow empty Referer field. This way, only requests from the domain names that are included in the Referer whitelist can access the resources in the bucket. For more information, see [Configure hotlink protection](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure hotlink protection.md).

-   Error message: Invalid according to Policy: Policy expired.

    Cause: The policy form field in the PostObject request is invalid.

    Solution: Set the policy form field to a valid value. The policy form field in a PostObject request is used to verify the validity of the request. The value of the policy form field is a JSON string encoded in UTF-8 and Base64. This value declares the conditions that a PostObject request must meet. The following code provides an example of the policy form field in a PostObject request:

    ```
    { "expiration": "2014-12-01T12:00:00.000Z",
      "conditions": [
        {"bucket": "johnsmith" },
        ["starts-with", "$key", "user/eric/"]
      ]
    }
    ```

    For more information about the conditions supported in the policy form field, see [Appendix: Policy](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).

-   Error message: Invalid according to Policy: Policy Condition failed: " + RelatedUnit; //XXX

    Cause: The conditions specified in the policy form field are invalid.

    Solution: Make sure that the conditions specified in the policy form field are valid. For more information about the conditions supported in the policy form field and how the conditions are matched, see [Appendix: Policy](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).

-   Error message: Target object does not reside in the same data center as source object.

    Cause: Objects cannot be copied across buckets in different regions.

    Solution: Make sure that the source bucket and the destination bucket are in the same region. For more information, see [CopyObject](/intl.en-US/API Reference/Object operations/Basic operations/CopyObject.md).

-   Error message: Query string authentication requires the Signature, Expires and OSSAccessKeyId parameters.

    Cause: Required parameters are not included in the signed URL.

    Solution: Include the following parameters in the signed URL: Signature, Expires, and OSSAccessKeyId. Example: `http://oss-example.oss-cn-hangzhou.aliyuncs.com/oss-api.pdf?OSSAccessKeyId=nz2pc56s936**9l&Expires=1141889120&Signature=vjbyPxybdZaNmGa%2ByT272YEAiv****`. For more information about how to add a signature to a URL, see [Generate a signed URL](/intl.en-US/API Reference/Access control/Generate a signed URL.md).

-   Error message: Invalid date \(should be seconds since epoch\).

    Cause: The value of Expires is invalid.

    Solution: Set Expires to a valid value. The Expires parameter specifies the time when the URL expires. The value of this parameter is a [UNIX timestamp](https://baike.baidu.com/item/unix时间戳/2078227?fr=aladdin), which is the number of seconds that have elapsed since January 1, 1970 00:00:00 UTC.

-   Error message: Request has expired.

    Cause: The request expires.

    Solution: Configure the value of Expires based on scenarios.

-   Error message: You do not have read permission on this object.

    Cause: You do not have read permissions on the object.

    Solution: Contact the object owner to obtain read permissions on the object.

-   Error message: You do not have write permission on this object.

    Cause: You do not have write permissions on the object.

    Solution: Contact the object owner to obtain write permissions on the object.

-   Error message: You do not have read acl permission on this object.

    Cause: You do not have read permissions on the ACL of the object.

    Solution: Contact the object owner to obtain permissions to perform the GetObjectACL operation on the object.

-   Error message: You do not have write acl permission on this object.

    Cause: You do not have write permissions on the ACL of the object.

    Solution: Contact the object owner to obtain permissions to perform the PutObjectACL operation on the object.

-   Error message: You have no right to access this object because of bucket acl.

    Cause: You do not have permissions to access the object.

    Solution: Obtain the appropriate permissions on the object to perform operations on the object, such as PutObject, GetObject, and AppendObject. For more information, see [Common examples of RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Common examples of RAM policies.md).

-   Error message: Anonymous access is forbidden for this operation.

    Cause: Anonymous users do not have permissions to perform the operation.

    Solution: Configure a bucket policy that grants anonymous users permissions to access specific resources in the bucket. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

-   Error message: Access denied by bucket policy.

    Cause: The access to the bucket is denied by the bucket policy.

    Solution: Configure bucket policies to suit common access scenarios. For more information, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

-   Error message: Access denied by authorizer's policy.

    Cause: STS access on the resource is denied by the policy set by the resource owner.

    Solution: See [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).


## AccessForbidden

-   Error message: CORSResponse: This CORS request is not allowed. This is usually because the evalution of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.
-   Cause: CORS is not configured for the bucket or the configured CORS rules are incorrect.
-   Solution: See [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).

## PermanentRedirect

-   Error message: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.
-   Cause: The endpoint is not specified or the incorrect endpoint is specified when you use OSS SDKs to access a bucket. For example, the error message returned if you use the default endpoint `oss-cn-hangzhou.aliyuncs.com` to access a bucket that you create in the China \(Qingdao\) region.
-   Solution: Use the endpoint of the region in which the bucket is located to access the bucket. For example, if you want to access buckets in the China \(Hangzhou\) and China \(Qingdao\) regions, we recommend that you create individual OSSClient instances of each region. Add `oss-cn-hangzhou.aliyuncs.com` to the OSSClient instance you want to use to access buckets in the China \(Hangzhou\) region and `oss-cn-qingdao.aliyuncs.com` to the OSSClient instance you want to use to access buckets in the China \(Qingdao\) region.

## SecondLevelDomainForbidden

-   Error message: The bucket you are attempting to access must be addressed using OSS third level domain.

    Cause: The domain name in the request is not a third-level domain.

    Solution: Include third-level domains that contain the bucket information in all requests except for the GetService \(ListBuckets\) operation. The domain name used to access a bucket is in the `BucketName.Endpoint` format. BucketName indicates the name of the bucket and Endpoint indicates the endpoint of the region in which the bucket is located. Example: `https://examplebucket.oss-cn-hangzhou.aliyuncs.com`.

-   Error message: Please use virtual hosted style to access.

    Cause: The URL that you use to access OSS is invalid.

    Solution: Use an URL in the following format to access OSS resources over the Internet: `<Schema>://<Bucket>.<Public endpoint>/<Object>` In the preceding URL, Schema indicates the protocol that is used by the URL, such as HTTP and HTTPS, Bucket indicates the name of the bucket in which the object that you want to access is stored, Public endpoint indicates the endpoint used to access the region in which the bucket is located, and Object indicates the path of the uploaded object that you want to access.

    For example, if you want to access an object named `example.txt` in the destfolder/ folder of the bucket named examplebucket in the China \(Hangzhou\) region, you can use the following URL: `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/destfolder/example.txt`.


## NonStandardHostForbidden

-   Error message: Your host is invalid. Please use Open Storage Service standard host.
-   Cause: The domain name used to access OSS is invalid.
-   Solution: Use a standard domain name to access OSS resources. For more information, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).

## KmsUbsmsInvalidBid

-   Error message: Your account partner does not have KMS Service.
-   Cause: KMS is not activated.
-   Solution: Activate KMS before you use the SSE-KMS method to encrypt data in OSS. For more information, see [Activate KMS](/intl.en-US/Quick Start/Activate KMS.md).

## KmsInDebt

-   Error message: Current user is indebted.
-   Cause: Your Alibaba Cloud account has overdue payments. A notification is sent to you and your access to KMS is denied.
-   Solution: Make sure that your Alibaba Cloud account does not have overdue payments when you want to use KMS.

## KmsInDebtOverdue

-   Error message: Current user is indebted Overdue.
-   Cause: Your account has overdue payments for KMS.
-   Solution: Recharge your account to use KMS.

## WORMConfigurationLocked

-   Error message: The WORM Configuration is locked.
-   Cause: You attempt to delete a retention policy that is locked.
-   Solution: Do not attempt to delete a locked retention policy. Locked retention policies cannot be deleted. The protection period specified by the retention policy cannot be shortened but can only be extended. For more information, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).

## BucketNotBelongTo

-   Error message: The bucket you access does not belong to you.
-   Cause: The current user is not the owner of the bucket.
-   Solution: Make sure you are the bucket owner before you perform this operation. Only the bucket owner can perform this operation.

## InvalidAccessKeyId

-   Error message:
    -   The OSS Access Key Id you provided is disabled.
    -   The OSS Access Key Id you provided does not exist in our records.
    -   The OSS Access Key Id contains non-acceptable characters, which accepts only alphanumeric characters\[0-9a-zA-Z\] and several special characters\[. \_=\]
-   Cause: The AccessKey ID does not exist or is invalid.
-   Solution: Log on to the Alibaba Cloud Management Console. Move the pointer over your profile picture, and then click [AccessKey](https://ak-console.aliyun.com). Confirm that the AccessKey ID that you use to access OSS exists and is enabled.

    If your AccessKey ID is disabled, enable it. If you do not have an AccessKey ID, create an AccessKey ID and use it to access OSS.


## SignatureDoesNotMatch

-   Error message: The request signature we calculated does not match the signature you provided.
-   Cause: A signature error occurred.
-   Solution: See [FAQ](/intl.en-US/Developer Guide/Data security/Signature/FAQ.md).

## TransferAccelerationDisabled

-   Error message: Transfer acceleration is disabled.
-   Cause: Transfer acceleration is disabled.
-   Solution: Enable transfer acceleration when you accelerate remote data transfer, accelerate the upload and download of objects of gigabytes or terabytes in size, and accelerate the download of non-static and non-hotspot data. For more information, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

## InvalidSecurityToken

-   Error message: The security token you provided is invalid.
-   Cause: The temporary credential used to access OSS is invalid.
-   Solution: See [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).

## AccessKeyIdAndSecurityTokenNotMatch

-   Error message: The OSS access key id and security token you provided does not match.
-   Cause: The AccessKey pair provided by the user does not match the temporary credential used to access OSS.
-   Solution: See [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md).

## SecurityTokenExpired

-   Error message: The security token you provided has expired.
-   Cause: The temporary credential used to access OSS is expired.
-   Solution: Send a request to obtain a new temporary access credential from STS.

## AbnormalBucketOwnerStatus

-   Error message: The status of the bucket owner is abnormal.
-   Cause: The service is not available for the bucket owner.
-   Solution: Check whether the Alibaba Cloud account of the bucket owner is canceled, restricted due to security reasons, or suspended due to overdue payments.

## SecurityTokenNotSupported

-   Error message: This interface does not support security token.

    Cause: The current operation cannot be called by users that have only temporary access credentials.

    Solution: Use methods other than STS tokens to authorize users to access your buckets. STS tokens are applicable only to authorize specific users to temporarily access OSS resources. For more information about authorization methods, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

-   Error message: Security token is not supported in this region.

    Cause: STS tokens are not supported in the current region.

    Solution: Use methods other than STS tokens to authorize users to access your buckets. For more information about the regions that support STS tokens, see [Endpoints](/intl.en-US/API Reference/API Reference (STS)/Endpoints.md).


## RequestTimeTooSkewed

-   Error message: The difference between the request time and the current time is too large.
-   Cause: The time when the request is initiated is at least 15 minutes earlier than the current time of the OSS server.
-   Solution: Check the system time of the device used to send the request, and adjust the system time based on your time zone.

    You can refer to the following guidelines to adjust the system time of the device used to send the request:

    -   OSS uses Greenwich Mean Time \(GMT\) as the system time. Therefore, the system time of your device must be set to GMT or a zone time relative to GMT. For example, GMT+00:00 is a time zone relative to GMT.
        -   To check the time zone in which your device is located in Windows, choose **Control Panel** \> **Clock, Language, and Region** \> **Set Date and Time**.

            For example, if the Time Zone column displays +08:00, it indicates that your device is located in the GMT+08:00 time zone.

        -   To check the time zone in which your device is located in Linux or UNIX, run the data-R command.

            In the following example figure, +0800 indicates that the device is in the GMT+08:00 time zone.

            ![+0800](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3106793161/p171537.png)

    -   You can use OSS in multiple regions. OSS uses GMT as the system time in all regions. Therefore, the system time of your device used to send requests must also be GMT.

## ImageDamage

-   Error message: The image file may be damaged.
-   Cause: The image cannot be identified or processed due to damaged or missing data.
-   Solution: Make sure that the image is not damaged. If the image is damaged, reupload the image.

## UserDisable

-   Error message: UserDisable
-   Cause: You account is deactivated due to overdue payments or security reasons.
-   Solution: Check whether your account has overdue payments or contact technical support to perform a security check.

## BucketDisable

-   Error message: BucketDisable.
-   Cause: The bucket is disabled due to security reasons.
-   Solution: Check whether your account has overdue payments or contact technical support to perform a security check.

## CnameDenied

-   Error message: The cname belongs to another user.
-   Cause: The domain name is already bound to another bucket.
-   Solution: Use another domain name or verify the ownership of the domain name and forcibly bind the domain name to the bucket. This operation unbinds the domain name from the previous bucket. See [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md) to handle the problem.

## InvalidObjectState

-   Error message: The operation is not valid for the object's state.
-   Cause: If one of the following conditions are met, the state of an Archive object is invalid:
    -   The RestoreObject request sent for the object timed out or was not initiated.
    -   The RestoreObject request sent for the object was initiated but the object was not restored.
-   Solution: See [RestoreObject](/intl.en-US/API Reference/Object operations/Basic operations/RestoreObject.md).

