# Overview

RAM policies are configured based on users. You can configure RAM policies to control the resources that can be accessed by users.

## Background information

-   Syntax and structure of RAM policies

    A RAM policy consists of the Version and Statement elements. A Statement contains the Effect, Action, Resource, and Condition fields, in which the Condition field is optional. For more information about the syntax and structure of RAM policies, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

    The usage of Version, Statement, and Effect in RAM policies for OSS is the same as that in policies for RAM. For more information about the usage of Actions, Resources, and Conditions in RAM policies for OSS, see the following sections:

    -   [Actions in RAM policies for OSS](#section_x3c_nsm_2gb)
    -   [Resources in RAM policies for OSS](#section_an0_sb1_5sh)
    -   [Conditions in RAM policies for OSS](#section_zhg_wsm_2gb)
-   Common RAM policies for OSS
    -   AliyunOSSFullAccess: grants a RAM user permissions to manage OSS resources.
    -   AliyunOSSReadOnlyAccess: grants a RAM user the read-only permission on OSS resources.
-   Access control methods supported by OSS

    For more information about access control methods supported by OSS, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).


## Actions in RAM policies for OSS

RAM policies for OSS support service-related actions, bucket-related actions, and object-related actions.

-   Service-related actions

    Service-related actions correspond to the GetService \(ListBuckets\) operation, which is used to list all buckets owned by the user.

    |API|Action|
    |:--|:-----|
    |GetService \(ListBuckets\)|oss:ListBuckets|

-   Bucket-related actions

    Bucket-related actions are performed on buckets. The names of bucket-related actions have one-to-one correspondence with the operations called by the these actions.

    |API|Action|
    |:--|:-----|
    |PutBucket|oss:PutBucket|
    |GetBucket \(ListObjects\)|oss:ListObjects|
    |GetBucketVersions \(ListObjectVersions\)|oss:ListObjectVersions|
    |PutBucketVersioning|oss:PutBucketVersioning|
    |GetBucketVersioning|oss:GetBucketVersioning|
    |PutBucketAcl|oss:PutBucketAcl|
    |GetBucketAcl|oss:GetBucketAcl|
    |DeleteBucket|oss:DeleteBucket|
    |GetBucketLocation|oss:GetBucketLocation|
    |GetBucketInfo|oss:GetBucketInfo|
    |GetBucketLogging|oss:GetBucketLogging|
    |PutBucketLogging|oss:PutBucketLogging|
    |DeleteBucketLogging|oss:DeleteBucketLogging|
    |GetBucketWebsite|oss:GetBucketWebsite|
    |PutBucketWebsite|oss:PutBucketWebsite|
    |DeleteBucketWebsite|oss:DeleteBucketWebsite|
    |GetBucketReferer|oss:GetBucketReferer|
    |PutBucketReferer|oss:PutBucketReferer|
    |GetBucketLifecycle|oss:GetBucketLifecycle|
    |PutBucketLifecycle|oss:PutBucketLifecycle|
    |DeleteBucketLifecycle|oss:DeleteBucketLifecycle|
    |ListMultipartUploads|oss:ListMultipartUploads|
    |PutBucketCors|oss:PutBucketCors|
    |GetBucketCors|oss:GetBucketCors|
    |DeleteBucketCors|oss:DeleteBucketCors|
    |PutBucketVersioning|oss:PutBucketVersioning|
    |GetBucketVersions\(ListObjectVersions\)|oss::ListObjectVersions|
    |PutBucketPolicy|oss:PutBucketPolicy|
    |GetBucketPolicy|oss:GetBucketPolicy|
    |DeleteBucketPolicy|oss:DeleteBucketPolicy|
    |PutBucketTags|oss:PutBucketTagging|
    |GetBucketTags|oss:GetBucketTagging|
    |DeleteBucketTags|oss:DeleteBucketTagging|
    |PutBucketEncryption|oss:PutBucketEncryption|
    |GetBucketEncryption|oss:GetBucketEncryption|
    |DeleteBucketEncryption|oss:DeleteBucketEncryption|
    |PutBucketRequestPayment|oss:PutBucketRequestPayment|
    |GetBucketRequestPayment|oss:GetBucketRequestPayment|
    |PutBucketReplication|oss:PutBucketReplication|
    |GetBucketReplication|oss:GetBucketReplication|
    |DeleteBucketReplication|oss:DeleteBucketReplication|
    |GetBucketReplicationLocation|oss:GetBucketReplicationLocation|
    |GetBucketReplicationProgress|oss:GetBucketReplicationProgress|

-   Object-related actions

    Object-related actions are performed on objects. The following table describes the correspondence between the object-related actions and the operations.

    |API|Action|
    |:--|:-----|
    |PutObject|oss:PutObject|
    |PostObject|
    |InitiateMultipartUpload|
    |UploadPart|
    |CompleteMultipart|
    |AppendObject|
    |CompleteMultipartUpload|
    |PutSymlink|
    |GetObject|oss:GetObject|
    |HeadObject|
    |GetObjectMeta|
    |SelectObject|
    |GetSymlink|
    |DeleteObject|oss:DeleteObject|
    |DeleteMultipleObjects|
    |CopyObject|oss:GetObject,oss:PutObject|
    |UploadPartCopy|
    |ListParts|oss:ListParts|
    |GetObjectAcl|oss:GetObjectAcl|
    |PutObjectAcl|oss:PutObjectAcl|
    |RestoreObject|oss:RestoreObject|
    |PutObjectTagging|oss:PutObjectTagging|
    |GetObjectTagging|oss:GetObjectTagging|
    |DeleteObjectTagging|oss:DeleteObjectTagging|
    |GetObject \(with versionId specified in the request\)|oss:GetObjectVersion|
    |PutObjectACL \(with versionId specified in the request\)|oss:PutObjectVersionAcl|
    |GetObjectAcl \(with versionId specified in the request\)|oss:GetObjectVersionAcl|
    |RestoreObject \(with versionId specified in the request\)|oss:RestoreObjectVersion|
    |DeleteObject \(with versionId specified in the request\)|oss:DeleteObjectVersion|
    |PutObjectTagging \(with versionId specified in the request\)|oss:PutObjectVersionTagging|
    |GetObjectTagging \(with versionId specified in the request\)|oss:GetObjectVersionTagging|
    |DeleteObjectTagging \(with versionId specified in the request\)|oss:DeleteObjectVersionTagging|
    |PutLiveChannel|oss:PutLiveChannel|
    |ListLiveChannel|oss:ListLiveChannel|
    |DeleteLiveChannel|oss:DeleteLiveChannel|
    |PutLiveChannelStatus|oss:PutLiveChannelStatus|
    |GetLiveChannelInfo|oss:GetLiveChannel|
    |GetLiveChannelStat|oss:GetLiveChannelStat|
    |GetLiveChannelHistory|oss:GetLiveChannelHistory|
    |PostVodPlaylist|oss:PostVodPlaylist|
    |GetVodPlaylist|oss:GetVodPlaylist|
    |ProcessImm|oss:ProcessImm|
    |ImgSaveAs|oss:PostProcessTask|
    |AbortMultipartUpload|oss:AbortMultipartUpload|


## Resources in RAM policies for OSS

In RAM policies for OSS, the Resource field indicates a specific resource or specific resources. This field supports asterisks \(\*\) as wildcards. A RAM policy can contain multiple Resource fields.

The Resource field is specified in the following format: `acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`.

When you specify the Resource field in a RAM policy for a bucket, you do not need to add a forward slash \(/\) and `{object_name}` after `{bucket_name}`. In this case, you can specify the Resource field in the following format: `acs:oss:{region}:{bucket_owner}:{bucket_name}`. The region field can be set only to a asterisk \(\*\) as a wildcard.

## Conditions in RAM policies for OSS

In RAM policies for OSS, the Condition field indicates the conditions for the RAM policies. The following table describes the conditions that are supported by RAM policies for OSS.

|Condition|Description|
|:--------|:----------|
|acs:SourceIp|The CIDR block from which the request is sent. Asterisks are supported as wildcards.|
|acs:UserAgent|The User-Agent header in the HTTP request.Type: string |
|acs:CurrentTime|The time when the request arrives at the OSS server.Format: ISO 8601 timestamp |
|acs:SecureTransport|The protocol type of the request. If the protocol type of the request is HTTP, set the value to HTTP. If the protocol type of the request is HTTPS, set the value to HTTPS.|
|oss:Prefix|The prefix of the objects listed by the ListObjects operation.|
|oss:Delimiter|The character that is used to group the names of objects listed by the ListObjects operation.|
|acs:AccessId|The AccessKey ID in the request.|
|oss:BucketTag|The tag of the bucket.A single BucketTag can be used as a condition. To configure multiple BucketTags as multiple conditions, you must add `oss:BucketTag/` before each BucketTag. |
|acs:MFAPresent|Indicates whether multi-factor authentication \(MFA\) is enabled.Valid values:

-   true: MFA is enabled.
-   false: MFA is disabled. |
|oss:ExistingObjectTag|Indicates that the requested object has tags.A single ObjectTag can be used as a condition. To configure multiple ObjectTags as multiple conditions, you must add `oss:ExistingObjectTag/` before each ObjectTag.

This condition applies to operations used to read objects, such as GetObject and HeadObject, and operations related to object tags, such as PutObjectTagging and GetObjectTagging. |
|oss:RequestObjectTag|The object tags contained in the request.A single ObjectTag can be used as a condition. To configure multiple ObjectTags as multiple conditions, you must add `oss:RequestObjectTag/` before each ObjectTag.

This condition applies to operations used to write objects, such as PutObject and PostObject, and operations related to object tags, such as PutObjectTagging and GetObjectTagging. |

## Examples

You can use RAM policies to authorize users in different scenarios. For more information, see [Common examples of RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Common examples of RAM policies.md).

