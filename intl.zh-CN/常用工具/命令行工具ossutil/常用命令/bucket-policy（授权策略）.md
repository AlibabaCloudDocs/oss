# bucket-policy（授权策略）

Bucket Policy是基于资源的授权策略，Bucket拥有者可以通过Bucket Policy授权其他用户访问OSS指定资源。bucket-policy命令用于添加、修改、查询、删除Bucket授权策略（Bucket Policy）。

**说明：**

-   本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。
-   有关Bucket Policy的更多信息，请参见[添加Bucket授权策略（Bucket Policy）](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/添加Bucket授权策略（Bucket Policy）.md)。

## 添加或修改Bucket Policy

添加或修改Bucket Policy前，需要在本地创建JSON格式的文件，并在JSON文件中配置Bucket Policy。单个JSON文件可以配置多条Bucket Policy，但所有Bucket Policy的总大小不能超过16 KB。

添加或修改Bucket Policy时，ossutil先从JSON格式的文件中读取Bucket Policy配置，然后将读取到Bucket Policy添加到指定的Bucket。添加Bucket Policy为覆盖语义，即新添加的Bucket Policy会覆写已有的Bucket Policy配置。

-   命令格式

    ```
    ./ossutil64 bucket-policy --method put oss://bucket\_name local\_json\_file
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucket\_name|添加或修改Bucket Policy的目标存储空间名称。|
    |local\_json\_file|配置Bucket Policy的本地JSON文件名称。|

-   使用示例
    1.  在本地创建名为local\_json\_file文件，并根据使用场景写入不同的Bucket Policy。

        Bucket Policy的常见配置示例如下：

        -   仅允许指定IP的匿名用户访问目标存储空间examplebucket内的所有资源。

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

        -   授权指定的RAM用户拥有读取examplebucket中`hanghzou/2020`和`hanghzou/2015`目录的只读权限。

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

        -   拒绝匿名用户访问examplebucket中`hangzhou/2021/`目录下的所有文件。

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

    2.  为examplebucket添加Bucket Policy。

        ```
        ./ossutil64 bucket-policy --method put oss://examplebucket local_json_file
        ```

        以下输出结果表明已成功添加Bucket Policy。

        ```
        1.125101(s) elapsed
        ```


## 获取Bucket Policy配置

-   命令格式

    ```
    ./ossutil64 bucket-policy --method get oss://bucket\_name local\_json\_file
    ```

    |参数|说明|
    |--|--|
    |bucket\_name|获取Policy配置的目标Bucket名称。|
    |local\_json\_file|用于存放Policy配置的本地JSON文件名称。如果未指定此参数，则Policy配置将直接输出到屏幕。|

-   使用示例

    获取examplebucket配置的Bucket Policy。

    ```
    ./ossutil64 bucket-policy --method get oss://examplebucket
    ```

    以下输出结果表明已成功获取Bucket Policy配置，并将其写入了本地JSON文件。

    ```
    0.212407(s) elapsed
    ```


## 删除Bucket Policy配置

当您不再需要通过Bucket Policy授权其他用户访问您的OSS资源时，请删除已配置的Bucket Policy。

-   命令格式

    ```
    ./ossuitl64 bucket-policy --method delete oss://bucket\_name
    ```

-   使用示例

    删除examplebucket中所有已配置的Bucket Policy。

    ```
    ./ossutil64 bucket-policy --method delete oss://examplebucket
    ```

    以下输出结果表明examplebucket中已配置的所有Bucket Policy均已删除。

    ```
    0.530750(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的存储空间配置Bucket Policy，命令如下：

```
./ossutil64 bucket-policy --method put oss://examplebucket local_json_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

