# Examples

You can configure bucket policies to authorize other users to access the specified Object Storage Service \(OSS\) resources. For example, you can configure bucket policies to grant different permissions, such as read-only or read/write, to anonymous users, Resource Access Management \(RAM\) users of the same Alibaba Cloud account, or different Alibaba Cloud accounts.

## Introduction

The following examples show the bucket policies configured by the bucket owner whose UID is `174649585760xxxx` to grant different permissions to RAM users, such as the RAM user whose UID is `27737962156157xxxx`. Compared with RAM policies, bucket policies contain the Principal field used to specify the users to which you want to grant permissions. The syntax of other fields in bucket policies, such as Action and Condition, is the same as that of these fields in RAM policies. For more information about how to configure the fields in a RAM policy, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Overview.md).

## Example 1: Grant the specified RAM users permissions to read and write a bucket

The following bucket policy can be configured to grant the RAM users whose UIDs are `27737962156157xxxx` and `20214760404935xxxx` permissions to read and write a bucket named examplebucket:

```
{    
    "Version": "1",
    "Statement": [{
        "Effect": "Allow",
        "Action": [
            "oss:GetObject",
            "oss:PutObject",
            "oss:GetObjectAcl",
            "oss:PutObjectAcl",
            "oss:ListObjects",
            "oss:AbortMultipartUpload",
            "oss:ListParts",
            "oss:RestoreObject",
            "oss:GetVodPlaylist",
            "oss:PostVodPlaylist",
            "oss:PublishRtmpStream",
            "oss:ListObjectVersions",
            "oss:GetObjectVersion",
            "oss:GetObjectVersionAcl",
            "oss:RestoreObjectVersion"
        ],
        "Principal": [
            "27737962156157xxxx",
            "20214760404935xxxx"
        ],
        "Resource": [
            "acs:oss:*:174649585760xxxx:examplebucket/*"
        ]
      }, {
        "Effect": "Allow",
        "Action": [
            "oss:ListObjects",
            "oss:GetObject"
        ],
        "Principal": [
            "27737962156157xxxx",
            "20214760404935xxxx"
        ],
        "Resource": [
            "acs:oss:*:174649585760xxxx:examplebucket"
        ],
        "Condition": {
            "StringLike": {
                "oss:Prefix": [
                    "*"
                ]
            }
        }
      }]      
}
```

## Example 2: Grant a RAM user permissions only to read the specified directory of a bucket

The following bucket policy can be configured to grant a RAM user whose UID is `20214760404935xxxx` permissions only to read the `hangzhou/2020`and `shanghai/2015` directories of a bucket named examplebucket:

```
{
     "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:GetObject",
                "oss:GetObjectAcl",
                "oss:GetObjectVersion",
                "oss:GetObjectVersionAcl"
            ],
            "Effect": "Allow",
            "Principal": [
                "20214760404935xxxx"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket/hangzhou/2020/*",
                "acs:oss:*:174649585760xxxx:examplebucket/shanghai/2015/*"
            ]
        },
        {
            "Action": [
                "oss:ListObjects",
                "oss:ListObjectVersions"
            ],
            "Condition": {
                "StringLike": {
                    "oss:Prefix": [
                        "hangzhou/2020/*",
                        "shanghai/2015/*"
                    ]
                }
            },
            "Effect": "Allow",
            "Principal": [
                "20214760404935xxxx"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
            ]
        }
    ],
}
```

## Example 3: Grant anonymous users permissions only to list all objects in a bucket

The following bucket policy can be configured to grant anonymous users permissions only to list all objects in a bucket named examplebucket:

```
{   
   "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:ListObjects",
                "oss:ListObjectVersions"
            ],
            "Effect": "Allow",            
            "Principal": [
                "*"
            ],            
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
            ]
        },
    ],    
}
```

## Example 4: Prevent users who use IP addresses that are not in the specified CIDR block from performing operations on a bucket

The following bucket policies can be configured to prevent anonymous users who use IP addresses that are not in the CIDR block `192.168.0.0/16` from performing operations on a bucket named examplebucket:

```
{
    "Version": "1",
    "Statement": [
        {
            "Effect": "Deny",
            "Action": "oss:*",
            "Principal": [
                "*"
            ],
            "Resource": [
                "acs:oss:*:174649585760xxxx:examplebucket"
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

