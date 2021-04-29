# bucket-policy

Bucket policies are resource-based authorization policies. Bucket owners can use bucket policies to authorize other users to access the specified resource in OSS. The bucket-policy command is used to add, modify, query, or delete bucket policy configurations for a bucket.

**Note:**

-   The commands described in this topic are for 64-bit Linux systems. To use the commands in the examples on 64-bit Windows systems, replace ./ossutil64 in the commands with ossutil64.exe.
-   For more information about bucket policies, see [Add bucket policies](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

## Add or modify bucket policy configurations

Before you add or modify bucket policies for a bucket, you must create a JSON file on your local computer, and configure bucket policies in the JSON file. You can configure multiple bucket policies for a single JSON file. However, the total size of the bucket policies cannot exceed 16 KB.

When you add or modify bucket policies, ossutil reads bucket policy configurations from the JSON file, and adds the read configurations to the specified bucket. When you add bucket policies, existing bucket policies are overwritten.

-   Command syntax

    ```
    ./ossutil64 bucket-policy --method put oss://bucketname local\_json\_file
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket for which you want to add or modify bucket policies.|
    |local\_json\_file|The name of the local JSON file when you configure bucket policies.|

-   Examples
    1.  Create a file named local\_json\_file on the local computer and write different bucket policies based on scenarios.

        The following code provides examples on common configurations of bucket policies.

        -   Specify that only requests that include the specified IP address from anonymous users are allowed to access all resources in examplebucket.

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

        -   Grant the specified RAM user read-only permissions on the `hanghzou/2020` and `hanghzou/2015` folders in examplebucket.

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

        -   Reject anonymous access to all the objects in the `hangzhou/2021/` folder of examplebucket.

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

    2.  Add a bucket policy for examplebucket.

        ```
        ./ossutil64 bucket-policy --method put oss://examplebucket local_json_file
        ```

        If a similar output is displayed, the bucket policy is added:

        ```
        1.125101(s) elapsed
        ```


## Query bucket policy configurations

-   Command syntax

    ```
    ./ossutil64 bucket-policy --method get oss://bucketname local\_json\_file
    ```

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket of which you want to query bucket policies.|
    |local\_json\_file|The local JSON file that is used to store policy configurations. If this parameter is not specified, the policy configurations are displayed without being stored in the JSON file.|

-   Examples

    The following code provides an example on how to query the bucket policies configured for examplebucket:

    ```
    ./ossutil64 bucket-policy --method get oss://examplebucket
    ```

    If a similar output is displayed, the bucket policy configurations are obtained and written to the local JSON file:

    ```
    0.212407(s) elapsed
    ```


## Delete bucket policy configurations

When you do not need to use bucket policies to authorize other users to access your OSS resource, delete the configured bucket policies.

-   Command syntax

    ```
    ./ossuitl64 bucket-policy --method delete oss://bucketname
    ```

-   Examples

    The following code provides an example on how to delete all bucket policies configured for examplebucket:

    ```
    ./ossutil64 bucket-policy --method delete oss://examplebucket
    ```

    If a similar output is displayed, all bucket policies configured for examplebucket are deleted.

    ```
    0.530750(s) elapsed
    ```


## Common options

When you use ossutil to manage buckets across regions, you can add the -e option to use the endpoint of the region in which the specified bucket is located. When you use ossutil to manage buckets owned by multiple Alibaba Cloud accounts, you can add the -i option to commands to use the AccessKey ID of the specified Alibaba Cloud account and add the -k option to use the AccessKey secret of the specified Alibaba Cloud account.

For example, you can run the following command to create a bucket policy for a bucket named examplebucket, which is owned by another Alibaba Cloud account in the China \(Hangzhou\) region:

```
./ossutil64 bucket-policy --method put oss://examplebucket local_json_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options that apply to the bucket-tagging command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

