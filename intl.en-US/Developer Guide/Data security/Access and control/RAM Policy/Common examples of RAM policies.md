# Common examples of RAM policies

You can configure RAM policies to manage the permissions of users such as employees, systems, or applications and control the resources that can be accessed by users. For example, you can create a RAM policy to grant users permissions to list and read the objects stored in a specified bucket.

## Assign a custom RAM policy for a RAM user

1.  Create a custom RAM policy.

    You can refer to the examples described in this topic based on actual scenarios and create a custom RAM policy by using scripts. For more information, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

2.  Assign the custom RAM policy for a RAM user

    Assign the RAM policy created in Step 1 for a RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).


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

    The following RAM policy grants a RAM user permissions to list and read objects in a bucket named `myphotos` by using OSS SDKs or ossutil:

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
                          "oss:GetBucketLifecycle",
                          "oss:GetBucketWorm",                      
                          "oss:GetBucketVersioning", 
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


## Example 3: Prohibit a RAM user from deleting a bucket

The following RAM policy prohibits a RAM user from deleting a bucket named `myphotos`:

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
      },
        {
         "Effect": "Deny",
         "Action": [
           "oss:DeleteBucket"
         ],
         "Resource": [
           "acs:oss:*:*:myphotos"
         ]
     }
   ]
}
```

## Example 4: Grant a RAM user permissions to access multiple folders in a bucket

In this example, a bucket named `myphotos` is used to store photos. The bucket contains multiple folders that are named based on the locations where the photos were captured. Each folder contains subfolders that are named based on the years when the photos were captured.

```
myphotos[Bucket]
  ├── beijing
  │   ├── 2014
  │   └── 2015
  ├── hangzhou
  │   ├── 2013
  │   ├── 2014
  │   └── 2015 
  └── qingdao
      ├── 2014
      └── 2015
```

In this example, RAM policies are created to grant a RAM user read-only permissions on the `myphotos/hangzhou/2014/` and `myphotos/hangzhou/2015/` folders. Authorization based on folders are advanced features of RAM policies. The complexity of RAM policies is different based on scenarios. You can refer to the RAM policies in the following scenarios to authorize users.

-   Scenario 1: Grant a RAM user permissions to only read objects in the `myphotos/hangzhou/2014/` and `myphotos/hangzhou/2015/` folders

    In this scenario, the RAM user knows the full path of the object to be accessed. Therefore, we recommend that you configure the RAM policy to allow the RAM user to access the object by using the full path of the object.

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
                    "acs:oss:*:*:myphotos/hangzhou/2014/*",
                    "acs:oss:*:*:myphotos/hangzhou/2015/*"
                ]
            }
        ]
    }
    ```

-   Scenario 2: Grant a RAM user permissions to access the `myphotos/hangzhou/2014/` and `myphotos/hangzhou/2015/` folders and list the objects in the folders by using ossutil

    In this scenario, the RAM user does not know the objects in the folders and can use ossutil or call API operations to obtain the information about the objects in the folders. In this case, the permission to perform `ListObjects` must be specified in the policy.

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
                    "acs:oss:*:*:myphotos/hangzhou/2014/*",
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
                        "oss:Prefix": [
                            "hangzhou/2014/*",
                        "hangzhou/2015/*"
                         ]
                    }
                }
            }
        ]
    }
    ```

-   Scenario 3: Grant a RAM user permissions to access a folder in the OSS console

    In this scenario, the RAM user can use the OSS console to access the `myphotos/hangzhou/2014/` and `myphotos/hangzhou/2015/` folders from the root folder by level.

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
                    "acs:oss:*:*:myphotos/hangzhou/2014/*",
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
                            "hangzhou/2014/*",
                            "hangzhou/2015/*"
                        ]
                    }
                }
            }
        ]
    }
    ```


## Example 5: Prohibit a RAM user from deleting objects in a bucket

The following RAM policy prohibits a RAM user from deleting objects in a bucket named `myphotos`:

```
{
  "Version": "1",
  "Statement": [
        {
         "Effect": "Deny",
         "Action": [
           "oss:DeleteObject"
         ],
         "Resource": [
           "acs:oss:*:*:myphotos/*"
         ]
     }
   ]
}
```

## Example 6: Prohibit a RAM user from accessing objects with specified tags

The following RAM policy includes a Deny statement that prohibits a RAM user from accessing objects that are stored in the examplebucket bucket and have the status:ok and key1:value1 tags:

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

## Example 7: Grant a RAM user permissions to access OSS from specified IP addresses

-   Add IP address conditions in the `Allow` statement

    The following RAM policy grants a RAM user permissions to read objects in a bucket named `myphotos` from only IP addresses in the `192.168.0.0/16` and `172.12.0.0/16` CIDR blocks that are specified in the `Allow` statement:

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

-   Add IP address conditions in the `Deny` statement

    The following RAM policy grants a RAM user permissions to perform operations on OSS resources from only IP addresses in the `192.168.0.0/16` CIDR block that is specified in the `Deny` statement. Operations performed from other IP addresses are prohibited.

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

    **Note:** In a RAM policy, a Deny statement takes precedence over an Allow statement. Therefore, when a RAM user attempts to read data in the myphotos bucket from an IP address that is not in the `192.168.0.0/16` CIDR block, OSS notifies the RAM user of having no permissions.


## Example 8: Use RAM or STS to grant other users permissions to access OSS resources

In this scenario, you can create a RAM policy to perform the following operations:

-   Grant specific users permissions to access the bucket named `mybucket` and the objects prefixed by `mybucket/file`.
-   Grant the users permissions to perform the following operations: GetBucketAcl, GetBucket, PutObject, GetObject, and DeleteObject.
-   In the Condition field, set UserAgent to java-sdk and the source IP address to `192.168.0.1`. Only users that meet these conditions can access specified OSS resources.
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

