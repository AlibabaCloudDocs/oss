# 基于RAM Policy的权限控制

RAM（Resource Access Management）是阿里云提供的资源访问控制服务，RAM Policy是基于用户的授权策略。通过设置RAM Policy，您可以集中管理您的用户（例如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限，例如限制您的用户只拥有对某一个Bucket的读权限。

## 前提条件

使用RAM Policy对OSS进行权限管理前，请先了解以下基本概念：

-   RAM Policy语法和结构

    RAM Policy包含版本号（Version）、授权语句（Statement）、每条授权语句包括授权效力（Effect）、操作（Action）、资源（Resource）以及限制条件（Condition，可选项）。有关权限策略的语法和结构的详情，请参见[权限策略语法和结构](/cn.zh-CN/权限策略管理/权限策略语言/权限策略语法和结构.md)。

    其中OSS中Version、Statement以及Effect的应用规则与RAM相同。OSS的Action、Resource以及Condition说明请参见：

    -   [附录1：OSS Action分类](#section_x3c_nsm_2gb)
    -   [附录2：OSS Resource说明](#section_an0_sb1_5sh)
    -   [附录3：OSS Condition说明](#section_zhg_wsm_2gb)
-   OSS常用的权限策略
    -   AliyunOSSFullAccess：为RAM用户授予OSS的完全管理权限。
    -   AliyunOSSReadOnlyAccess：为RAM用户授予OSS的只读访问权限。
-   OSS访问控制方式

    有关OSS包含的访问控制方式详情，请参见[访问控制概述](/cn.zh-CN/开发指南/数据安全/访问控制/访问控制概述.md)。


## 为RAM用户授权自定义的RAM Policy

1.  根据下述各场景示例创建相应的自定义策略。

    有关如何创建自定义策略的详情，请参见[创建自定义策略](/cn.zh-CN/权限策略管理/自定义策略/创建自定义策略.md)。

2.  找到创建好的权限策略，单击其权限策略名称。

3.  在**被授权主体**区域下，输入RAM用户名称后，单击需要授权的RAM用户。

4.  在**引用记录**页签下，单击**新增授权**。

5.  单击**确定**。

    **说明：** 更多为RAM用户或用户组授权的方式，请参见[为RAM用户授权](/cn.zh-CN/用户管理/为RAM用户授权.md)和[为用户组授权](/cn.zh-CN/用户组管理/为用户组授权.md)。


## 示例1：授权RAM用户对某个Bucket的完全控制权限

以下示例为授权RAM用户对名为`myphotos`的Bucket拥有完全控制的权限。

警告：

对于移动应用来说，授予用户对Bucket的完全控制权限有极高风险，应尽量避免。

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

    以下示例为授权RAM用户通过OSS SDK或OSS命令行工具列举并读取名为`myphotos`的Bucket中的资源：

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

    用户登录OSS控制台时，OSS控制台会额外调用ListBuckets、GetBucketAcl和GetObjectAcl，以确定存储空间属性是公开还是私有。

    以上示例为授权RAM用户通过OSS控制台列举并读取名为`myphotos`Bucket中的资源：

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


## 示例3：授权RAM用户通过特定的IP地址访问OSS

-   在`Allow`授权中增加IP限制

    以下示例为在`Allow`授权中增加IP限制，授权RAM用户仅允许通过`192.168.0.0/16`、`172.12.0.0/16`两个IP段读取名为`myphotos`Bucket中的资源。

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

-   在`Deny`授权中增加IP限制

    以下示例为在`Deny`授权中增加IP限制，授权RAM用户的源IP若不在`192.168.0.0/16`范围内，则禁止对OSS执行任何操作。

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


## 示例4：授权RAM用户对目录级别的访问

假设用于存放照片的Bucket为`myphotos`，该Bucket下有一些目录，代表照片的拍摄地，每个拍摄地目录下还包含了年份子目录。

```
myphotos[Bucket]
  ├── beijing
  │   ├── 2014
  │   └── 2015
  ├── hangzhou
  │   ├── 2013
  │   ├── 2014
  │   └── 2015 //授予此目录只读权限
  └── qingdao
      ├── 2014
      └── 2015
```

若要授权RAM用户访问`myphotos/hangzhou/2015/`目录的只读权限。目录级别的授权属于授权的高级功能，根据使用场景不同，授权策略的复杂程度也不同，以下几种场景可供参考。

-   场景1：授予RAM用户仅拥有读取目录`myphotos/hangzhou/2015/`中文件内容的权限

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
                    "acs:oss:*:*:myphotos/hangzhou/2015/*"
                ]
            }
        ]
    }
    ```

-   场景2：授权RAM用户使用OSS命令行工具访问目录`myphotos/hangzhou/2015/`并列举目录中文件的权限

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

-   场景3： 授予RAM用户使用OSS控制台访问目录

    RAM用户使用可视化的OSS客户端访问目录`myphotos/hangzhou/2015/`，可视化的客户端类似Windows文件管理器，RAM用户可以从根目录开始，逐层进入要访问的目录。

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


## 示例5：通过RAM或STS服务向其他用户授权

通过RAM或STS服务向其他用户授权的场景说明如下：

-   把用户自己名下的`mybucket`和`mybucket/file*`资源授权给相应的用户。
-   允许其他用户执行GetBucketAcl、GetBucket、PutObject、GetObject和DeleteObject操作。
-   Condition中的条件表示UserAgent为java-sdk，源IP为192.168.0.1的鉴权被允许，此时被授权的用户拥有相关资源的访问权限。
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

## 附录1：OSS Action分类

Action分为以下三大类：

-   Service级别操作，对应的是GetService操作，用来列举所有属于该用户的Bucket列表。
-   Bucket级别操作，对应类似于oss:PutBucketAcl、oss:GetBucketLocation之类的操作，操作的对象是Bucket，其名称和相应的接口名称一一对应。
-   Object级别操作，分为oss:GetObject、oss:PutObject、oss:DeleteObject和oss:AbortMultipartUpload，操作对象是Object。

具体的Action和API接口的对应关系如下：

-   Service级别

    |API|Action|
    |:--|:-----|
    |GetService（ListBuckets）|oss:ListBuckets|

-   Bucket 级别

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

-   Object级别

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
    |GetObject（请求参数中指定versionId）|oss:GetObjectVersion|
    |PutObjectACL（请求参数中指定versionId）|oss:PutObjectVersionAcl|
    |GetObjectAcl（请求参数中指定versionId）|oss:GetObjectVersionAcl|
    |RestoreObject（请求参数中指定versionId）|oss:RestoreObjectVersion|
    |DeleteObject（请求参数中指定versionId）|oss:DeleteObjectVersion|
    |PutObjectTagging（请求参数中指定versionId）|oss:PutObjectVersionTagging|
    |GetObjectTagging（请求参数中指定versionId）|oss:GetObjectVersionTagging|
    |DeleteObjectTagging（请求参数中指定versionId）|oss:DeleteObjectVersionTagging|
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


## 附录2：OSS Resource说明

在OSS中，Resource指代某个具体资源或者某些资源，支持通配符星号（\*）。单个RAM Policy允许包含多个Resource。

Resource规则：`acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`

针对Bucket级别的Resource设置，不需要在`{bucket_name}`之后添加正斜线（/）以及`{object_name}`，即`acs:oss:{region}:{bucket_owner}:{bucket_name}`。region字段当前仅支持设置为通配符星号（\*）。

## 附录3：OSS Condition说明

Condition代表Policy授权的一些条件，以上示例中可以设置对于acs:UserAgent的检查、acs:SourceIp的检查，还可以使用oss:Prefix项在调用GetBucket时对资源进行限制。

OSS支持的Condition如下：

|Condition|说明|
|:--------|:-|
|acs:SourceIp|指定普通IP网段，支持通配符星号（\*）。|
|acs:UserAgent|指定HTTP User-Agent头。类型：字符串 |
|acs:CurrentTime|请求到达OSS服务端的时间。格式：ISO8601 |
|acs:SecureTransport|请求的协议类型。如果请求是HTTP协议，则为HTTP，如果是HTTPS协议，则为HTTPS。|
|oss:Prefix|用于ListObjects请求时，列举指定前缀的Object。|
|oss:Delimiter|用于ListObjects请求时，对Object名字进行分组的字符。|
|acs:AccessId|请求中携带的AccessId。|
|oss:BucketTag|存储空间标签（BucketTag）。单个BucketTag可以作为一个Condition。当设置多个BucketTag时，需在每个BucketTag前加上oss:BucketTag/，组成多个Condition。 |
|acs:MFAPresent|是否启用了多因素认证MFA（Multi-factor authentication）。取值：

-   true：启用了多因素认证
-   false：未启用多因素认证 |
|oss:ExistingObjectTag|请求的Object已存在标签。单个ObjectTag可以作为一个Condition。当设置多个ObjectTag时，需在每个ObjectTag前加上oss:ExistingObjectTag/，组成多个Condition。

主要针对GetObject、HeadObject等读取文件接口以及PutObjectTagging、GetObjectTagging等ObjectTagging接口。 |
|oss:RequestObjectTag|请求中携带的对象标签。单个ObjectTag可以作为一个Condition。当设置多个ObjectTag时，需在每个ObjectTag前加上oss:RequestObjectTag/，组成多个Condition。

主要针对PutObject、PostObject等写入文件接口以及PutObjectTagging、GetObjectTagging等ObjectTagging接口。 |

例如，如果需要禁止RAM用户访问存储空间examplebucket下对象标签为status:ok以及key1:value1的Object，可添加以下Deny策略。

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

