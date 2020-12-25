# Implement access control based on RAM policies

Resource Access Management \(RAM\) is a resource access control service provided by Alibaba Cloud. You can configure RAM policies based on the responsibilities of users. You can manage users by configuring RAM policies. For users such as employees, systems, or applications, you can control which resources are accessible. For example, you can create a RAM policy to grant users read permissions only on a specified bucket.

## Prerequisites

You understand the following basic concepts:

-   Syntax and structure of RAM policies

    A RAM policy consists of the Version and Statement element. A Statement contains the Effect, Action, Resource, and Condition fields.. The Condition field is optional. For more information about the syntax and structure of RAM policies, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

    The usage of Version, Statement, and Effect in RAM policies for OSS is the same as that in policies for RAM. For more information about the usage of Actions, Resources, and Conditions in RAM policies for OSS, see the following sections:

    -   [Appendix 1: Action in RAM policies for OSS](#section_x3c_nsm_2gb)
    -   [Appendix 2: Resource in RAM policies for OSS](#section_an0_sb1_5sh)
    -   [Appendix 3: Condition in RAM policies for OSS](#section_zhg_wsm_2gb)
-   Common RAM policies for OSS
    -   AliyunOSSFullAccess: grants a RAM user permissions to manage OSS resources.
    -   AliyunOSSReadOnlyAccess: grants a RAM user the read-only permission on OSS resources.
-   Access control methods of OSS

    For more information about access control methods supported by OSS, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).


## Assign a custom RAM policy for a RAM user

1.  Create a custom RAM policy based on the examples described in the following sections.

    For more information about how to create a custom RAM policy, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

2.  On the Policies page, click the name of the created policy.

3.  In the Add Permissions dialog box that appears, enter the name of the RAM user in the **Principal** field, and select the RAM user from the auto-complete results.

4.  Click the **References** tab, and then click **Grant Permission**.

5.  Click **OK**.

    **Note:** For more information about how to grant permissions to RAM users or user groups, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md) and [Grant permissions to a RAM user group](/intl.en-US/RAM User Group Management/Grant permissions to a RAM user group.md).


## Example 1: Grant a RAM user permissions to completely control a bucket

The following RAM policy grants a RAM user permissions to completely control a bucket named `myphotos`:

**Warning:** We recommend that you do not grant RAM users permissions to completely control a bucket used by mobile applications because it is highly risky.

```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "oss:*",
            "Resource": [
                "acs:oss:*:*:myphotos",
                "acs:oss:*:*:myphotos/*"
            ]
        }
    ]
}
```

## Example 2: Grant a RAM user to list and read objects in a bucket

-   Grant a RAM user permissions to list and read objects in a bucket by using OSS SDKs or ossutil

    The following RAM policy grants a RAM user permissions to list and read objects in a bucket named `myphotos` by using OSS SDKs or ossutil.

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": "oss:ListObjects",
                "Resource": "acs:oss:*:*:myphotos"
            },
            {
                "Effect": "Allow",
                "Action": "oss:GetObject",
                "Resource": "acs:oss:*:*:myphotos/*"
            }
        ]
    }
    ```

-   Grant a RAM user permissions to list and read objects in a bucket in the OSS console

    When a RAM user logs on to the OSS console, the ListBuckets, GetBucketAcl, and GetObjectAcl operations are called to check whether the bucket is private.

    The following RAM policy grants a RAM user permissions to list and read objects in a bucket named `myphotos` in the OSS console:

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                          "oss:ListBuckets",
                          "oss:GetBucketStat",
                          "oss:GetBucketInfo",
                          "oss:GetBucketTagging",
                          "oss:GetBucketAcl" 
                          ],    
                "Resource": "acs:oss:*:*:*"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:ListObjects",
                    "oss:GetBucketAcl"
                ],
                "Resource": "acs:oss:*:*:myphotos"
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:GetObject",
                    "oss:GetObjectAcl"
                ],
                "Resource": "acs:oss:*:*:myphotos/*"
            }
        ]
    }
    ```


## Example 3: Grant a RAM user permissions to access OSS from specified IP addresses

-   Add IP addresses in the `Allow` field

    The following RAM policy grants a RAM user permissions to read objects in a bucket named `myphotos` from only IP addresses in the `192.168.0.0/16` and `172.12.0.0/16` CIDR blocks that are specified in the `Allow` field:

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                          "oss:ListBuckets",
                          "oss:GetBucketStat",
                          "oss:GetBucketInfo",
                          "oss:GetBucketTagging",
                          "oss:GetBucketAcl" 
                          ], 
                "Resource": [
                    "acs:oss:*:*:*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:ListObjects",
                    "oss:GetObject"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos",
                    "acs:oss:*:*:myphotos/*"
                ],
                "Condition":{
                    "IpAddress": {
                        "acs:SourceIp": ["192.168.0.0/16", "172.12.0.0/16"]
                    }
                }
            }
        ]
    }
    ```

-   Add IP addresses in the `Deny` field

    The following RAM policy grants a RAM user permissions to perform operations on OSS from only IP addresses in the `192.168.0.0/16` CIDR block that is specified in the `Deny` field. Operations performed from other IP addresses are forbidden.

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                          "oss:ListBuckets",
                          "oss:GetBucketStat",
                          "oss:GetBucketInfo",
                          "oss:GetBucketTagging",
                          "oss:GetBucketAcl" 
                          ], 
                "Resource": [
                    "acs:oss:*:*:*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:ListObjects",
                    "oss:GetObject"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos",
                    "acs:oss:*:*:myphotos/*"
                ]
            },
            {
                "Effect": "Deny",
                "Action": "oss:*",
                "Resource": [
                    "acs:oss:*:*:*"
                ],
                "Condition":{
                    "NotIpAddress": {
                        "acs:SourceIp": ["192.168.0.0/16"]
                    }
                }
            }
        ]
    }
    ```

    **Note:** In a RAM policy, a statement in which the Effect field is Deny takes precedence over a statement in which the Effect field is Allow. Therefore, when a RAM user attempts to read data from the myphotos bucket from an IP address that is not in the `192.168.0.0/16` CIDR block, OSS notifies the RAM user of having no permissions.


## Example 4: Grant a RAM user permissions to access a folder in a bucket

In this example, a bucket named `myphotos` is used to store photos. The bucket contains multiple folders that are named based on the location where the photos were captured. Each folder contains subfolders that are named based on the years when the photos were captured.

```
myphotos[Bucket]
  ├── beijing
  │   ├── 2014
  │   └── 2015
  ├── hangzhou
  │   ├── 2013
  │   ├── 2014
  │   └── 2015 // Grant the RAM user the read-only permission on this folder.
  └── qingdao
      ├── 2014
      └── 2015
```

You can create different RAM policies to grant read-only permissions on the `myphotos/hangzhou/2015/` folder to a RAM user based on specific scenarios. The following examples describe three typical scenarios:

-   Scenario 1: Grant a RAM user permissions to only read objects in the `myphotos/hangzhou/2015/` folder

    In this scenario, the RAM user knows the full path of the folder. Therefore, we recommend that the RAM user should use the full path of an object to access the object.

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "oss:GetObject"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos/hangzhou/2015/*"
                ]
            }
        ]
    }
    ```

-   Scenario 2: Grant a RAM user permissions to access the `myphotos/hangzhou/2015/` folder and list the objects in the folder by using ossutil

    In this scenario, the RAM user does not know the objects in the folder and can use ossutil or call API operations to obtain the folder information. In this case, the permission to perform `ListObjects` must be specified in the policy.

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                    "oss:GetObject"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos/hangzhou/2015/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:ListObjects"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos"
                ],
                "Condition":{
                    "StringLike":{
                        "oss:Prefix":"hangzhou/2015/*"
                    }
                }
            }
        ]
    }
    ```

-   Scenario 3: Grant a RAM user permissions to access a folder in the OSS console

    In this scenario, the RAM user can use a visualized OSS client that is analogous to Windows File Explorer to enter the `myphotos/hangzhou/2015/` folder from the root folder by level.

    ```
    {
        "Version": "1",
        "Statement": [
            {
                "Effect": "Allow",
                "Action": [
                          "oss:ListBuckets",
                          "oss:GetBucketStat",
                          "oss:GetBucketInfo",
                          "oss:GetBucketTagging",
                          "oss:GetBucketAcl" 
                          ], 
                "Resource": [
                    "acs:oss:*:*:*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:GetObject",
                    "oss:GetObjectAcl"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos/hangzhou/2015/*"
                ]
            },
            {
                "Effect": "Allow",
                "Action": [
                    "oss:ListObjects"
                ],
                "Resource": [
                    "acs:oss:*:*:myphotos"
                ],
                "Condition": {
                    "StringLike": {
                        "oss:Delimiter": "/",
                        "oss:Prefix": [
                            "",
                            "hangzhou/",
                            "hangzhou/2015/*"
                        ]
                    }
                }
            }
        ]
    }
    ```


## Example 5: Use RAM or STS to grant other users permissions to access OSS resources

In this scenario, you can create a RAM policy to perform the following operations:

-   Grant specific users permissions to access the bucket named `mybucket` and the objects prefixed with `file` in the bucket.
-   Grant the users permissions to perform the following operations: GetBucketAcl, GetBucket, PutObject, GetObject, and DeleteObject.
-   In the Condition field, set UserAgent to java-sdk and the source IP address to 192.168.0.1. Only users who meet these conditions can access specified OSS resources.
-   Grant the users permissions to list only objects prefixed by foo.

The following RAM policy can meet the requirements of the preceding scenario:

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:GetBucketAcl",
                "oss:ListObjects"
            ],
            "Resource": [
                "acs:oss:*:177530505652XXXX:mybucket"
            ],
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "acs:UserAgent": "java-sdk",
                    "oss:Prefix": "foo"
                },
                "IpAddress": {
                    "acs:SourceIp": "192.168.0.1"
                }
            }
        },
        {
            "Action": [
                "oss:PutObject",
                "oss:GetObject",
                "oss:DeleteObject"
            ],
            "Resource": [
                "acs:oss:*:177530505652XXXX:mybucket/file*"
            ],
            "Effect": "Allow",
            "Condition": {
               "StringEquals": {
                    "acs:UserAgent": "java-sdk"
                },
                "IpAddress": {
                    "acs:SourceIp": "192.168.0.1"
                }
            }
        }
    ]
}
```

## Appendix 1: Action in RAM policies for OSS

The Action field in RAM policies for OSS supports the following operations:

-   Service operations: correspond to the GetService operation, which is used to list all buckets owned by the user.
-   Bucket operations: correspond to operations such as oss:PutBucketAcl and oss:GetBucketLocation on buckets. The names of operations correspond to those of the actions.
-   Object operations: correspond to operations such as oss:GetObject, oss:PutObject, oss:DeleteObject, and oss:AbortMultipartUpload on objects.

The following table describes the mapping between the values of Action and API operations.

-   Service operations

    |API|Action|
    |:--|:-----|
    |GetService \(ListBuckets\)|oss:ListBuckets|

-   Buckets

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
    |ListParts|oss:ListParts|
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

-   Objects

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
    |ImgSaveAs|oss:PostProcessTask|
    |AbortMultipartUpload|oss:AbortMultipartUpload|


## Appendix 2: Resource in RAM policies for OSS

In RAM policies for OSS, the Resource field indicates a specific resource or specific resources and supports asterisks \(\*\) as wildcards. A RAM policy can contain multiple Resource fields.

The Resource field is specified in the following format: `acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`.

When you specify the Resource field in a RAM policy for a bucket, you do not need to add a slash \(/\) and `{object_name}` after `{bucket_name}`. In this case, you can specify the Resource field in the following format: `acs:oss:{region}:{bucket_owner}:{bucket_name}`. The region field can be set only to a asterisk \(\*\) as a wildcard.

## Appendix 3: Condition in RAM policies for OSS

The Condition field specifies the conditions for the RAM policy. In the preceding examples, the acs:UserAgent, acs:SourceIp, and oss:Prefix fields are specified as the conditions used to restrict the GetBucket operation.

The following table describes the conditions that are supported by RAM policies for OSS.

|Condition|Description|
|:--------|:----------|
|acs:SourceIp|The CIDR block from which the request is sent. Asterisks are supported as wildcards.|
|acs:UserAgent|The User-Agent header in the HTTP request.Type: string |
|acs:CurrentTime|The time when the request arrives at the OSS server.Format: ISO 8601 timestamp |
|acs:SecureTransport|The protocol type of the request. If the protocol type of the request is HTTP, set the value to HTTP. If the protocol type of the request is HTTPS, set the value to HTTPS.|
|oss:Prefix|The prefix of the objects listed by the ListObjects operation.|
|oss:Delimiter|The character that is used to group the names of objects listed by the ListObjects operation.|
|acs:AccessId|The AccessKey ID in the request.|
|oss:BucketTag|The tag of the bucket.A single BucketTag can be used as a condition. To configure multiple BucketTags as multiple conditions, you must add oss:BucketTag/ before each BucketTag. |
|acs:MFAPresent|Indicates whether multi-factor authentication \(MFA\) is enabled.Valid values:

-   true: MFA is enabled.
-   false: MFA is disabled. |
|oss:ExistingObjectTag|Indicates that the requested object has tags.A single ObjectTag can be used as a condition. To configure multiple ObjectTags as multiple conditions, you must add oss:ExistingObjectTag/ before each ObjectTag.

This condition applies to operations used to read objects, such as GetObject and HeadObject, and operations related to object tags, such as PutObjectTagging and GetObjectTagging. |
|oss:RequestObjectTag|The object tags contained in the request.A single ObjectTag can be used as a condition. To configure multiple ObjectTags as multiple conditions, you must add oss:RequestObjectTag/ before each ObjectTag.

This condition applies to operations used to write objects, such as PutObject and PostObject, and operations related to object tags, such as PutObjectTagging and GetObjectTagging. |

For example, to prohibit a RAM user from accessing objects with the status:ok and key1:value1 tags in the examplebucket bucket, create the following RAM policy that contains a Deny statement:

```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": [
                "oss:GetObject"
            ],
            "Resource": [
                "acs:oss:*:1746495857602745:examplebucket/*"
            ],
            "Condition": {
                "StringEquals": {
                    "oss:ExistingObjectTag/status":"ok",
                    "oss:ExistingObjectTag/key1":"value1"
                }
            }
        }
    ]
}
```

