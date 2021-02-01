# RAM Policy常见示例

通过RAM Policy，您可以集中管理您的用户（例如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限，例如授权RAM用户列举并读取某个存储空间（Bucket）的资源。

## 为RAM用户授权自定义的RAM Policy

1.  创建自定义的RAM Policy。

    您可以结合实际使用场景，选用下文列举的常见授权示例，然后通过脚本配置方式创建自定义的RAM Policy。具体操作，请参见[创建自定义策略](/intl.zh-CN/权限策略管理/自定义策略/创建自定义策略.md)。

2.  为RAM用户授权RAM Policy。

    为RAM用户授权[步骤1](#step_bln_7uj_1up)中创建好的RAM Policy。具体操作，请参见[为RAM用户授权](/intl.zh-CN/用户管理/为RAM用户授权.md)。


## 示例1：授权RAM用户对某个Bucket的完全控制权限

以下示例为授权RAM用户对名为`myphotos`的Bucket拥有完全控制的权限。

**警告：** 对于移动应用来说，授予用户对Bucket的完全控制权限有极高风险，应尽量避免。

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

## 示例2：授权RAM用户列举并读取某个Bucket的资源

-   授权RAM用户通过OSS SDK或OSS命令行工具列举并读取某个Bucket的资源

    以下示例为授权RAM用户通过OSS SDK或OSS命令行工具列举并读取名为`myphotos`存储空间中的资源：

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

-   授权RAM用户通过OSS控制台列举并读取某个Bucket的资源

    以下示例为授权RAM用户通过OSS控制台列举并读取名为`myphotos`存储空间中的资源：

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


## 示例3：拒绝RAM用户删除某个Bucket的权限

以下示例用于授权RAM用户禁止删除名为`myphotos`存储空间的权限：

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

## 示例4：授权RAM用户访问某个Bucket下多个目录的权限

假设用于存放照片的Bucket为`myphotos`，该Bucket下有一些目录，代表照片的拍摄地，每个拍摄地目录下还包含了年份子目录。

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

若要授权RAM用户访问`myphotos/hangzhou/2014/`和`myphotos/hangzhou/2015/`目录的只读权限。目录级别的授权属于授权的高级功能，根据使用场景不同，授权策略的复杂程度也不同，以下几种场景可供参考。

-   场景1：授予RAM用户仅拥有读取目录`myphotos/hangzhou/2014/`和`myphotos/hangzhou/2015/`中文件内容的权限

    由于RAM用户知道文件的完整路径，建议直接使用完整的文件路径来读取目录下的文件内容。

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

-   场景2：授权RAM用户使用OSS命令行工具访问目录`myphotos/hangzhou/2014/`和`myphotos/hangzhou/2015/`并列举目录中文件的权限

    RAM用户不清楚目录中有哪些文件，可以使用OSS命令行工具或API直接获取目录信息，此场景下需要添加`ListObjects`权限。

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

-   场景3： 授予RAM用户使用OSS控制台访问目录

    使用OSS控制台访问目录`myphotos/hangzhou/2014/`和`myphotos/hangzhou/2015/`时，RAM用户可以从根目录开始，逐层进入要访问的目录。

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


## 示例5：拒绝RAM用户删除某个Bucket下任意文件的权限

以下示例用于授权RAM用户禁止删除名为`myphotos`的存储空间下任意文件的权限：

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

## 示例6：拒绝RAM用户访问指定标签的Object

以下为添加Deny策略，用于拒绝RAM用户访问存储空间examplebucket下对象标签为status:ok以及key1:value1的Object。

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

## 示例7：授权RAM用户通过特定的IP地址访问OSS

-   在`Allow`授权中增加IP地址限制

    以下示例为在`Allow`授权中增加IP地址限制，授权RAM用户仅允许通过`192.168.0.0/16`、`172.12.0.0/16`两个IP地址段读取名为`myphotos`Bucket中的资源。

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

-   在`Deny`授权中增加IP地址限制

    以下示例为在`Deny`授权中增加IP地址限制，授权RAM用户的源IP地址若不在`192.168.0.0/16`范围内，则禁止对OSS执行任何操作。

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

    **说明：** 由于权限策略的鉴权规则是Deny优先，所以访问者从`192.168.0.0/16`以外的IP地址访问myphotos中的内容时，OSS会提示没有权限。


## 示例8：通过RAM或STS服务向其他用户授权

通过RAM或STS服务向其他用户授权的场景说明如下：

-   把用户自己名下的`mybucket`和`mybucket/file*`资源授权给相应的用户。
-   允许其他用户执行GetBucketAcl、GetBucket、PutObject、GetObject和DeleteObject操作。
-   Condition中的条件表示UserAgent为java-sdk，源IP地址为`192.168.0.1`的鉴权被允许，此时被授权的用户拥有相关资源的访问权限。
-   仅列举Prefix为foo的Object。

符合上述场景的RAM Policy示例如下：

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

