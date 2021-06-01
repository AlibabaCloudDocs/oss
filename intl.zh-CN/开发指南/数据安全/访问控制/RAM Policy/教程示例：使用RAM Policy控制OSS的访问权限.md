# 教程示例：使用RAM Policy控制OSS的访问权限

本教程示例详细演示了如何使用RAM Policy控制用户对OSS存储空间（Bucket）、文件夹以及文件夹下文件（Object）的访问权限。

## 背景信息

RAM Policy是基于用户的授权策略。通过设置RAM Policy，您可以集中管理您的用户（例如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限，例如限制您的用户只拥有对某一个Bucket的读权限。

RAM Policy为JSON格式。各字段定义如下：

-   Statement：授权语句，一个权限策略可以有多条授权语句。
-   Effect：授权效力，包括允许（Allow）和拒绝（Deny）两种。

    **说明：** 当权限策略中既有Allow又有Deny的授权语句时，遵循Deny优先的原则。

-   Action：对具体资源的操作权限。

如果您选择使用RAM Policy，建议您通过官方工具[RAM策略编辑器](/intl.zh-CN/常用工具/RAM策略编辑器.md)快速生成RAM策略。

相比于RAM Policy，Bucket Policy支持在控制台直接进行图形化配置操作，并且Bucket拥有者可以直接进行授权访问。详情请参见[使用Bucket Policy授权其他用户访问OSS资源](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/通过Bucket Policy授权用户访问指定资源.md)。

## 存储空间和文件夹的基本概念

阿里云OSS的数据模型为扁平型结构，所有文件都直接隶属于其对应的存储空间。因此，OSS缺少文件系统中类似于目录与子文件夹的层次结构。但是，您可以在OSS控制台上模拟文件夹层次结构。在该控制台中，您可以按文件夹对相关文件进行分组、分类和管理，如下图所示。

![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5300734061/p178620.png)

OSS提供使用键值（key）对格式的分布式对象存储服务。您可以根据其唯一的key（对象名）检索对象的内容。例如，名为ramtest-bucket的存储空间有三个文件夹，分别为Development、Marketing和Private，以及一个对象oss-dg.pdf。

-   在创建Development文件夹时，控制台会创建一个key为`Development/`的对象，文件夹的key包括分隔符`/`。
-   当您将名为ProjectA.docx 的对象上传到Development 文件夹中时，控制台会上传该对象并将其key设置为`Development/ProjectA.docx`。

    在该key中，`Development`为前缀，而`/`为分隔符。您可以从存储空间中获取具有特定前缀和分隔符的所有对象的列表。在控制台中，单击Development 文件夹时，控制台会列出文件夹中的对象，如下图所示。

    ![development](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5300734061/p178622.png)

    **说明：** 当控制台列举ramtest-bucket存储空间中的 Development文件夹时，它会向OSS发送一个用于指定前缀 `Development`和分隔符`/`的请求。因此，存储空间ramtest-bucket有三个对象，其key分别为`Development/Alibaba Cloud.pdf`、`Development/ProjectA.docx`及`Development/ProjectB.docx`。


在本教程开始之前，您还需要了解根级存储空间内容的概念。假设ramtest-bucket存储空间包含以下对象：

-   Development/Alibaba Cloud.pdf
-   Development/ProjectA.docx
-   Development/ProjectB.docx
-   Marketing/data2020.xlsx
-   Marketing/data2021.xlsx
-   Private/2017/images.zip
-   Private/2017/promote.pptx
-   oss-dg.pdf

这些对象的key构建了一个以Development、Marketing和Private作为根级文件夹并以 oss-dg.pdf作为根级对象的逻辑层次结构。当您单击OSS控制台中的存储空间名时，控制台会将一级前缀和一个分隔符，例如Development/、Marketing/和Private/显示为根级文件夹。对象oss-dg.pdf 没有前缀，因此显示为根级别项。

![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5300734061/p178620.png)

## OSS的请求和响应逻辑

在授予RAM用户相关权限之前，您需要了解单击某个存储空间的名字时控制台向OSS发送请求、OSS返回响应，以及控制台如何解析该响应的逻辑。

-   请求某个存储空间

    单击ramtest-bucket存储空间时，控制台会将[GetBucket \(ListObjects\)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)请求发送至OSS。

    -   请求示例

        ```
        GET /?prefix=&delimiter=/ HTTP/1.1
        Host: ramtest-bucket.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        此请求包括prefix和delimiter参数，其中prefix的值为空字符串，delimiter的值为正斜线（/）。

    -   响应示例

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <?xml version="1.0" encoding="UTF-8"?>
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>ramtest-bucket</Name>
        <Prefix></Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>oss-dg.pdf</Key>
                ...
            </Contents>
           <CommonPrefixes>
                <Prefix>Development</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Marketing</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Private</Prefix>
           </CommonPrefixes>
        </ListBucketResult>
        ```

    -   控制台解析

        控制台会解析此结果并显示如下的根级别项：

        ![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5300734061/p178620.png)

-   请求存储空间下的某个文件夹

    单击Development/文件夹，控制台会将[GetBucket \(ListObjects\)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucket (ListObjects).md)请求发送至OSS。此请求包括以下参数：

    -   请求示例

        ```
        GET /?prefix=Development/&delimiter=/ HTTP/1.1
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        此请求包括prefix和delimiter参数，其中prefix的值为`Development/`，delimiter的值为正斜线（/）。

    -   响应示例

        作为响应，OSS返回以指定前缀开头的key：

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <?xml version="1.0" encoding="UTF-8"?>
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>ramtest-bucket</Name>
        <Prefix>Development/</Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>ProjectA.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>ProjectB.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>Alibaba Cloud.pdf</Key>
                ...
            </Contents>
        </ListBucketResult>
        ```

    -   控制台解析

        控制台会解析此结果并显示如下的key：

        ![development](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5300734061/p178622.png)


## 场景示例

假设您是目标存储空间`ramtest-bucket`的Owner，且该Bucket下所有的文件或目录读写权限ACL默认为私有。现在，您希望授予RAM用户Anne访问该Bucket下文件夹`Development`及其子文件夹和文件的读写权限，RAM用户Leo访问文件夹`Marketing`及其子文件夹和文件的只读权限，以及当前阿里云账号下的所有RAM用户均无权访问文件夹`Private`的权限。

## 步骤一：创建存储空间并上传文件

1.  创建存储空间ramtest-bucket。

    1.  使用阿里云账号登录[OSS控制台](https://oss.console.aliyun.com/)。

    2.  创建名为ramtest-bucket的存储空间。具体操作，请参见[创建存储空间](/intl.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。

2.  创建目录Development、Marketing和Private。具体操作，请参见[创建目录](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/创建目录.md)。

3.  按如下要求将文件上传至指定路径。

    -   将oss-dg.pdf文件上传至ramtest-bucket的根目录。
    -   将文件Alibaba Cloud.pdf、ProjectA.docx以及ProjectB.docx上传至Development目录。
    -   将文件data2020.xlsx和data2021.xlsx上传至Marketing目录。
    -   将文件images.zip和promote.pptx上传至Private目录。
    具体操作，请参见[上传文件](/intl.zh-CN/控制台用户指南/上传、下载和管理文件/上传文件.md)。


## 步骤二：创建RAM用户Anne和Leo

通过[RAM控制台](https://ram.console.aliyun.com/)创建RAM用户Anne和Leo为例。有关创建RAM用户的详情，请参见[创建RAM用户](/intl.zh-CN/用户管理/基本操作/创建RAM用户.md)。

## 步骤三：授予RAM用户Anne拥有文件夹Development的读写权限

1.  创建自定义权限策略AllowAnneToReadAndWriteFolderDevelopment，并授予RAM用户Anne拥有文件夹Development及文件夹下所有文件的读写权限。

    1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面，**策略名称**填写为AllowAnneToReadAndWriteFolderDevelopment，配置模式选择**脚本配置**，**策略内容**配置如下：

        ```
        {
            "Version":"1",
            "Statement":[
                {
                    "Effect":"Allow",
                    "Action":[
                        "oss:ListObjects"
                    ],
                    "Resource":[
                        "acs:oss:*:*:ramtest-bucket"
                    ],
                    "Condition":{
                        "StringEquals":{
                            "oss:Prefix":[
                                "Development",
                                "Development/*"
                            ]
                        }
                    }
                },
                {
                    "Effect":"Allow",
                    "Action":[
                        "oss:GetObject",
                        "oss:PutObject",
                        "oss:GetObjectAcl"
                    ],
                    "Resource":[
                        "acs:oss:*:*:ramtest-bucket/Development/*"
                    ]
                }
            ]
        }
        ```

    4.  单击**确定**。

2.  为RAM用户Anne添加自定义权限策略AllowAnneToReadAndWriteFolderDevelopment。具体操作，请参见[为RAM用户授权](/intl.zh-CN/用户管理/授权管理/为RAM用户授权.md)。


## 步骤四：授予RAM用户Leo拥有文件夹Marketing的只读权限

参见[步骤三](#section_gqp_p63_kl8)创建自定义权限策略AllowLeoToReadAndWriteFolderMarketing，并授予RAM用户Leo只读访问文件夹Marketing及文件夹下所有文件的权限。其策略内容配置如下：

```
{
    "Version":"1",
    "Statement":[
        {
            "Effect":"Allow",
            "Action":[
                "oss:ListObjects"
            ],
            "Resource":[
                "acs:oss:*:*:ramtest-bucket"
            ],
            "Condition":{
                "StringEquals":{
                    "oss:Prefix":[
                        "Marketing",
                        "Marketing/*"
                    ]
                }
            }
        },
        {
            "Effect":"Allow",
            "Action":[
                "oss:GetObject",
                "oss:GetObjectAcl"
            ],
            "Resource":[
                "acs:oss:*:*:ramtest-bucket/Marketing/*"
            ]
        }
    ]
}
```

## 步骤5：拒绝当前阿里云账号下的所有RAM用户访问Private文件夹

1.  创建用户组并添加用户组成员。

    创建用户组的具体操作，请参见[创建用户组](/intl.zh-CN/用户组管理/创建用户组.md)。用户组创建完成后，将当前阿里云账号下的所有RAM用户添加到该用户组。具体操作，请参见[为用户组添加RAM用户](/intl.zh-CN/用户组管理/为用户组添加RAM用户.md)。

2.  创建自定义权限策略DenyAllRamToAccessFolderPrivate，并授予当前阿里云账号下的所有RAM用户拒绝访问Private文件夹的权限。

    1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。

    2.  单击**创建权限策略**。

    3.  在新建自定义权限策略页面，**策略名称**填写为DenyAllRamToAccessFolderPrivate，配置模式选择**脚本配置**，**策略内容**配置如下：

        ```
        {
            "Version":"1",
            "Statement":[
                {
                    "Effect":"Deny",
                    "Action":[
                        "oss:*"
                    ],
                    "Resource":[
                        "acs:oss:*:*:ramtest-bucket/Private/*"
                    ],
                    "Condition":{
        
                    }
                },
                {
                    "Effect":"Deny",
                    "Action":[
                        "oss:ListObjects"
                    ],
                    "Resource":[
                        "acs:oss:*:*:*"
                    ],
                    "Condition":{
                        "StringEquals":{
                            "oss:Prefix":[
                                "Private/",
                                "Private/*"
                            ]
                        }
                    }
                }
            ]
        }
        ```

    4.  单击**确定**。

3.  为用户组添加自定义权限策略DenyAllRamToAccessFolderPrivate。具体操作，请参见[为用户组授权](/intl.zh-CN/用户组管理/为用户组授权.md)。

    添加权限策略后，用户组中的任何RAM用户都不能访问您存储空间`ramtest-bucket`中的文件夹`Private`，且当RAM用户请求列举`Private`文件夹下的`Private/2017/images.zip`、`Private/2017/promote.pptx`文件时，OSS也将返回错误响应。


