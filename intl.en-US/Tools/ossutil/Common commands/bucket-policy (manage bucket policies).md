# bucket-policy \(manage bucket policies\)

Bucket policies are resource-based authorization policies. Bucket owners can use bucket policies to authorize other users to access the specified resource in OSS. The bucket-policy command is used to add, modify, query, or delete bucket policy configurations for a bucket.

**Note:**

-   The commands described in this topic are for 64-bit Linux systems. To use the commands in the examples on 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.
-   For more information about bucket policies, see [Configure bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md).

## Add or modify bucket policies

Before you add or modify bucket policies for a bucket, you must create a JSON file on your local device, and configure bucket policies in the JSON file. You can configure multiple bucket policies in a single JSON file. However, the total size of the bucket policies cannot exceed 16 KB.

When you add or modify bucket policies, ossutil reads bucket policies from the JSON file, and adds the policies to the specified bucket. When you add bucket policies, existing bucket policies are overwritten.

-   Command syntax

    ```
    ./ossutil64 bucket-policy --method put oss://bucketname local\_json\_file
    ```

    The following table describes the parameters that you can configure when you run this command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to add or modify bucket policies.|
    |local\_json\_file|The name of the local JSON file in which you configure bucket policies.|

-   Examples
    1.  Create a file named local\_json\_file on your local device and write different bucket policies based on scenarios.

        The following examples show how to configure common bucket policies:

        -   Specify that only anonymous requests from the specified IP address are allowed to access all resources in a bucket named examplebucket.

            ```
            {
                "Statement": [
                    {
                        "Action": [
                            "oss:GetObject",
                            "oss:GetObjectAcl",
                            "oss:ListObjects",
                            "oss:RestoreObject",
                            "oss:GetVodPlaylist",
                            "oss:ListObjectVersions",
                            "oss:GetObjectVersion",
                            "oss:GetObjectVersionAcl",
                            "oss:RestoreObjectVersion"
                        ],
                        "Condition": {
                            "IpAddress": {
                                "acs:SourceIp": [
                                    "10.10.10.10"
                                ]
                            }
                        },
                        "Effect": "Allow",
                        "Principal": [
                            "*"
                        ],
                        "Resource": [
                            "acs:oss:*:1746495857602745:examplebucket/*"
                        ]
                    },
                    {
                        "Action": [
                            "oss:ListObjects",
                            "oss:GetObject"
                        ],
                        "Condition": {
                            "StringLike": {
                                "oss:Prefix": [
                                    "*"
                                ]
                            },
                            "IpAddress": {
                                "acs:SourceIp": [
                                    "10.10.10.10"
                                ]
                            }
                        },
                        "Effect": "Allow",
                        "Principal": [
                            "*"
                        ],
                        "Resource": [
                            "acs:oss:*:1746495857602745:examplebucket"
                        ]
                    }
                ],
                "Version": "1"
            }
            ```

        -   Grant the specified RAM user read-only permissions on the `hanghzou/2020` and `hanghzou/2015` directories in a bucket named examplebucket.

            ```
            {
                "Statement": [
                    {
                        "Action": [
                            "oss:GetObject",
                            "oss:GetObjectAcl",
                            "oss:ListObjects",
                            "oss:RestoreObject",
                            "oss:GetVodPlaylist",
                            "oss:ListObjectVersions",
                            "oss:GetObjectVersion",
                            "oss:GetObjectVersionAcl",
                            "oss:RestoreObjectVersion"
                        ],
                        "Effect": "Allow",
                        "Principal": [
                            "202147604049359142"
                        ],
                        "Resource": [
                            "acs:oss:*:174649585760****:examplebucket/hanghzou/2020/*",
                            "acs:oss:*:174649585760****:examplebucket/hangzhou/2015/*"
                        ]
                    },
                    {
                        "Action": [
                            "oss:ListObjects",
                            "oss:GetObject"
                        ],
                        "Condition": {
                            "StringLike": {
                                "oss:Prefix": [
                                    "hanghzou/2020/*",
                                    "hangzhou/2015/*"
                                ]
                            }
                        },
                        "Effect": "Allow",
                        "Principal": [
                            "202147604049359142"
                        ],
                        "Resource": [
                            "acs:oss:*:174649585760****:examplebucket"
                        ]
                    }
                ],
                "Version": "1"
            }
            ```

        -   Reject anonymous requests to all the objects in the `hangzhou/2021/` directory of a bucket named examplebucket.

            ```
            {
                "Statement": [
                    {
                        "Action": [
                            "oss:RestoreObject",
                            "oss:ListObjects",
                            "oss:AbortMultipartUpload",
                            "oss:PutObjectAcl",
                            "oss:GetObjectAcl",
                            "oss:ListParts",
                            "oss:DeleteObject",
                            "oss:PutObject",
                            "oss:GetObject",
                            "oss:GetVodPlaylist",
                            "oss:PostVodPlaylist",
                            "oss:PublishRtmpStream",
                            "oss:ListObjectVersions",
                            "oss:GetObjectVersion",
                            "oss:GetObjectVersionAcl",
                            "oss:RestoreObjectVersion"
                        ],
                        "Effect": "Deny",
                        "Principal": [
                            "*"
                        ],
                        "Resource": [
                            "acs:oss:*:174649585760****:examplebucket/hangzhou/2021/*"
                        ]
                    },
                    {
                        "Action": [
                            "oss:ListObjects",
                            "oss:GetObject"
                        ],
                        "Condition": {
                            "StringLike": {
                                "oss:Prefix": [
                                    "hangzhou/2021/*"
                                ]
                            }
                        },
                        "Effect": "Deny",
                        "Principal": [
                            "*"
                        ],
                        "Resource": [
                            "acs:oss:*:174649585760****:examplebucket"
                        ]
                    }
                ],
                "Version": "1"
            }
            ```

    2.  Add a bucket policy to examplebucket.

        ```
        ./ossutil64 bucket-policy --method put oss://examplebucket local_json_file
        ```

        If a similar output is displayed, the bucket policy is added to examplebucket.

        ```
        1.125101(s) elapsed
        ```


## Query bucket policies

-   Command syntax

    ```
    ./ossutil64 bucket-policy --method get oss://bucketname local\_json\_file
    ```

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket whose policies you want to query.|
    |local\_json\_file|The local JSON file that is used to store the obtained bucket policies. If this parameter is not specified, obtained bucket policies are displayed without being stored in the JSON file.|

-   Examples

    You can run the following commands to query the bucket policies configured for a bucket named examplebucket:

    ```
    ./ossutil64 bucket-policy --method get oss://examplebucket
    ```

    If a similar output is displayed, the bucket policies of examplebucket are obtained and written to the local JSON file.

    ```
    0.212407(s) elapsed
    ```


## Delete bucket policies

If you no longer need to use bucket policies to authorize other users to access your OSS resource, delete the configured bucket policies.

-   Command syntax

    ```
    ./ossuitl64 bucket-policy --method delete oss://bucketname
    ```

-   Examples

    You can run the following command to delete all bucket policies configured for a bucket named examplebucket:

    ```
    ./ossutil64 bucket-policy --method delete oss://examplebucket
    ```

    If a similar output is displayed, all bucket policies configured for examplebucket are deleted.

    ```
    0.530750(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to configure a bucket policy for a bucket named examplebucket, which is located in the China \(Hangzhou\) region and is owned by another Alibaba Cloud account:

```
./ossutil64 bucket-policy --method put oss://examplebucket local_json_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about the bucket-policy command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

