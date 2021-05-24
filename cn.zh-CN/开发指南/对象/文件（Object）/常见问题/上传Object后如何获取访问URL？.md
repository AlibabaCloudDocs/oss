# 上传Object后如何获取访问URL？

本文举例说明如何在上传文件（Object）后获取文件的访问地址。

## 公共读Object

如果Object允许匿名访问，那么文件URL的格式为：`https://BucketName.Endpoint/ObjectName`

例如华东1（杭州）地域下名为bucketexample的Bucket下有名为example的文件夹，文件夹内有个名为example.jpg的文件。则该文件URL为：

-   外网访问URL：`https://bucketexample.oss-cn-hangzhou.aliyuncs.com/example/example.jpg`
-   内网访问URL（供同地域ECS实例访问）：`https://bucketexample.oss-cn-hangzhou-internal.aliyuncs.com/example/example.jpg`

**说明：**

-   各地域Endpoint信息介绍，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。
-   ObjectName是包含文件夹以及文件后缀在内的该文件的完整路径。
-   如需确保通过文件URL访问文件时是预览行为，您需要绑定自定义域名并添加CNAME记录。详情请参见[使用自有域名访问OSS资源](/cn.zh-CN/快速入门/控制台快速入门/使用自有域名访问OSS资源.md)。

## 私有Object

如果Object是私有权限，则必须进行签名操作。文件URL的格式为：`https://BucketName.Endpoint/Object?签名参数`。您可以通过以下任意方法获取Object的访问URL并设置URL的有效时间。

-   控制台

    请参见控制台用户指南中的[下载文件](/cn.zh-CN/控制台用户指南/文件管理/下载文件.md)。 通过OSS控制台获取文件URL时，主账号用户最长有效时间是32400秒（9小时），RAM用户（子账号用户）以及STS用户最长有效时间是3600秒（1小时）。如果要获取更长时效的文件URL，请使用命令行工具ossutil、图形化工具ossbrowser或SDK。

-   命令行工具ossutil

    请参见[ossutil-sign](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/sign（生成签名URL）.md)。

-   图形化工具ossbrowser

    请参见[ossbrowser快速入门](/cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速使用ossbrowser.md)。

-   SDK

    请参见：

    -   [Java](/cn.zh-CN/SDK 示例/Java/授权访问.md)
    -   [Python](/cn.zh-CN/SDK 示例/Python/授权访问.md)
    -   [Go](/cn.zh-CN/SDK 示例/Go/授权访问.md)
    -   [PHP](/cn.zh-CN/SDK 示例/PHP/授权访问.md)
    -   [C](/cn.zh-CN/SDK 示例/C/授权访问.md)
    -   [.NET](/cn.zh-CN/SDK 示例/.NET/授权访问.md)
    -   [Android](/cn.zh-CN/SDK 示例/Android/授权访问.md)
    -   [iOS](/cn.zh-CN/SDK 示例/iOS/授权访问.md)
    -   [Node.js](/cn.zh-CN/SDK 示例/Node.js/授权访问.md)
    -   [Browser.js](/cn.zh-CN/SDK 示例/Browser.js/授权访问.md)

## 自有域名Object

如果Object所在的Bucket绑定了自定义域名，那么文件URL的格式为：`https://YourDomainName/ObjectName`。

例如您在华东1（杭州）地域下名为bucketexample的存储空间下有名为example的文件夹，文件夹内有个名为example.jpg的文件；您有一个自己的域名`img.example.com`。

-   如果未绑定自定义域名，则该文件URL为：`https://bucketexample.oss-cn-hangzhou.aliyuncs.com/example/example.jpg`
-   如果绑定了自定义域名，则该文件URL为：`https://img.example.com/example/example.jpg`

**说明：** ObjectName是包含文件夹（如果有的话）以及文件后缀在内的该文件的完整路径。

