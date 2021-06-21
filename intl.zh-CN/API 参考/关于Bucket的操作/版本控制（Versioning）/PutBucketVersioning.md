# PutBucketVersioning

调用PutBucketVersioning设置指定存储空间（Bucket）的版本控制状态。

## 注意事项

-   要配置版本控制，您必须有PutBucketVersioning权限。
-   Bucket包括未开启、开启（Enabled）或者暂停（Suspended）三种版本控制状态。默认情况下Bucket处于未开启版本控制状态。
-   在Bucket处于开启版本控制状态下，所有新添加的文件（Object）都将拥有唯一的版本ID，OSS将累积所添加Object的多个版本。
-   在Bucket处于暂停版本控制状态下，所有新添加Object的版本ID将为null，且OSS将不再为此状态下添加的Object累积更多的版本。

关于版本控制的更多信息，请参见[版本控制介绍](/intl.zh-CN/开发指南/数据安全/版本控制/版本控制介绍.md)。

## 请求语法

```
PUT /?versioning HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<VersioningConfiguration>
    <Status>Enabled</Status>
</VersioningConfiguration>
```

## 示例

-   开启版本控制请求示例

    ```
    PUT /?versioning HTTP/1.1
    Host: bucket-versioning.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:20:12 GMT
    Authorization: OSS e7thre3jj5mlvqk:12ztptkaR8a74gIGFzOaZZQe****
    <?xml version="1.0" encoding="UTF-8"?>
    <VersioningConfiguration>
        <Status>Enabled</Status>
    </VersioningConfiguration>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC015CB7AEADE01700****
    Date: Tue, 09 Apr 2019 02:20:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   暂停版本控制请求示例

    ```
    PUT /?versioning HTTP/1.1
    Host: bucket-versioning.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 02:28:18 GMT
    Authorization: OSS m2qa99e9tpkaehr:DWAzr2EkqDwFJNke1Nuaogn7****
    <?xml version="1.0" encoding="UTF-8"?>
    <VersioningConfiguration>
        <Status>Suspended</Status>
    </VersioningConfiguration>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC0342B7AEADE01700****
    Date: Tue, 09 Apr 2019 02:28:18 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/版本控制/管理版本控制.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/版本控制/管理版本控制.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/版本控制/管理版本控制.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/版本控制/管理版本控制.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/版本控制/管理版本控制.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/版本控制/管理版本控制.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|AccessDenied|403|无设置版本控制状态的操作权限。|
|InvalidArgument|400|无效的Versioning状态。Versioning状态只能设置为Enabled或Suspended。|

