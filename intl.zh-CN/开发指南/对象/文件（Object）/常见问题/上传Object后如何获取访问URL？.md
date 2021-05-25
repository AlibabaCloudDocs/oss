# 上传Object后如何获取访问URL？

本文举例说明如何在上传文件（Object）后获取文件的访问地址。

## 公共读Object

如果文件的读写权限ACL为公共读，即该文件允许匿名访问，那么文件URL的格式为`https://BucketName.Endpoint/ObjectName`。其中，ObjectName需填写包含文件夹以及文件后缀在内的该文件的完整路径。各地域的Endpoint信息介绍，请参见[访问域名和数据中心](/intl.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。

例如华东1（杭州）地域下名为bucketexample的Bucket下有名为example的文件夹，文件夹内有个名为example.jpg的文件。则该文件URL为：

-   外网访问URL：`https://bucketexample.oss-cn-hangzhou.aliyuncs.com/example/example.jpg`
-   内网访问URL（供同地域ECS实例访问）：`https://bucketexample.oss-cn-hangzhou-internal.aliyuncs.com/example/example.jpg`

**说明：** 如需确保通过文件URL访问文件时是预览行为，您需要绑定自定义域名并添加CNAME记录。详情请参见[使用自有域名访问OSS资源](/intl.zh-CN/快速入门/控制台快速入门/使用自有域名访问OSS资源.md)。

## 私有Object

如果文件读写权限ACL为私有，则必须进行签名操作。私有文件URL的格式为`https://BucketName.Endpoint/Object?签名参数`。您可以通过以下任意方法获取文件URL并设置URL的有效时长。

-   控制台

    您可以通过OSS控制台获取文件URL。具体操作，请参见[分享文件](/intl.zh-CN/快速入门/控制台快速入门/分享文件.md)。文件URL的有效时长因账号类型存在差异。例如，阿里云账号可设置的文件URL有效时长最大为32400秒（9小时），RAM用户以及STS用户可设置的文件URL有效时长最大为3600秒（1小时）。如需获取更长时效的文件URL，请使用命令行工具ossutil、图形化工具ossbrowser或SDK。

-   命令行工具ossutil

    请参见[ossutil-sign](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/sign（生成签名URL）.md)。

-   图形化工具ossbrowser

    请参见[ossbrowser快速入门](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速使用ossbrowser.md)。

-   SDK

    请参见：

    -   [Java](/intl.zh-CN/SDK 示例/Java/授权访问.md)
    -   [Python](/intl.zh-CN/SDK 示例/Python/授权访问.md)
    -   [Go](/intl.zh-CN/SDK 示例/Go/授权访问.md)
    -   [PHP](/intl.zh-CN/SDK 示例/PHP/授权访问.md)
    -   [C](/intl.zh-CN/SDK 示例/C/授权访问.md)
    -   [.NET](/intl.zh-CN/SDK 示例/.NET/授权访问.md)
    -   [Android](/intl.zh-CN/SDK 示例/Android/授权访问.md)
    -   [iOS](/intl.zh-CN/SDK 示例/iOS/授权访问.md)
    -   [Node.js](/intl.zh-CN/SDK 示例/Node.js/授权访问.md)
    -   [Browser.js](/intl.zh-CN/SDK 示例/Browser.js/授权访问.md)

## 自有域名Object

如果文件所在的Bucket绑定了自定义域名，则文件URL的格式为`https://YourDomainName/ObjectName`，其中ObjectName需填写包含文件夹以及文件后缀在内的该文件的完整路径。

例如您在华东1（杭州）地域下的存储空间bucketexample，绑定了自有域名`img.example.com`。且该bucket下有名为example的文件夹，文件夹内有名为example.jpg的文件，则该文件URL为`https://img.example.com/example/example.jpg`。

