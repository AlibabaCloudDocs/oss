# PutSymlink

调用PutSymlink接口用于为OSS的目标文件（TargetObject）创建软链接（Symlink），您可以通过该软链接访问TargetObject。

## 注意事项

-   使用PutSymlink接口创建软链接时不会检查目标文件是否存在、目标文件类型是否合法以及目标文件是否有访问权限。
-   Symlink自身的访问权限（ACL）以及目标文件的ACL检查仅会在GetObject等需要访问目标文件的API中进行。
-   使用PutSymlink接口时，携带以x-oss-meta-为前缀的参数，则被视为user meta，例如x-oss-meta-location。一个Object可以有多个类似的参数，但所有的user meta总大小不能超过8 KB。
-   默认情况下，如果试图添加的文件已经存在，并且有访问权限，则新添加的文件将覆盖原来的文件，成功添加后将返回200 OK。

## 版本控制

您可以通过TargetObject创建的软链接指向TargetObject的当前版本。

软链接本身也可以有多个版本，每个不同的版本可以指向不同的TargetObject，版本ID由OSS自动生成，在响应Header中返回x-oss-version-id。

## 请求语法

```
PUT /ObjectName?symlink HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-symlink-target: TargetObjectName
```

## 请求头

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|x-oss-forbid-overwrite|字符串|否|指定PutSymlink操作时是否覆盖同名Object。 -   不指定x-oss-forbid-overwrite或者指定x-oss-forbid-overwrite为false时，表示允许覆盖同名Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object。

设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求Header（QPS\>1000），请联系技术支持，避免影响您的业务。

**说明：** 当目标Bucket处于已开启或已暂停版本控制状态时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。 |
|x-oss-symlink-target|字符串|是|软链接指向的目标文件。

合法值：命名规范同Object

-   TargetObjectName同ObjectName一样，需要对其进行URL编码。
-   软链接的目标文件类型不能为软链接。 |
|x-oss-object-acl|字符串|否|指定OSS创建Object时的访问权限。 取值：

-   default（默认）：Object遵循所在存储空间的访问权限。
-   private：Object是私有资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户没有权限操作该Object。
-   public-read：Object是公共读资源。只有Object的拥有者和授权用户有该Object的读写权限，其他用户只有该Object的读权限。请谨慎使用该权限。
-   public-read-write：Object是公共读写资源。所有用户都有该Object的读写权限。请谨慎使用该权限。

关于访问权限的更多信息，请参见[读写权限ACL](/intl.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。 |
|x-oss-storage-class|字符串|否|指定Object的存储类型。

对于任意存储类型的Bucket，如果上传Object时指定此参数，则此次上传的Object将存储为指定的类型。例如在IA类型的Bucket中上传Object时，如果指定x-oss-storage-class为Standard，则该Object直接存储为Standard。

取值：

-   Standard：标准存储
-   IA：低频访问
-   Archive：归档存储

IA与Archive类型的单个Object大小如果不足64 KB，则会按64 KB计量计费。建议在使用PutSymlink接口时不要将Object的存储类型指定为IA或Archive。

关于存储类型的更多信息，请参见[存储类型介绍](/intl.zh-CN/开发指南/存储类型/存储类型介绍.md)。 |

此接口还需要包含Host、Date等公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅包含公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
    PUT /link-to-oss.jpg?symlink HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Cache-control: no-cache 
    Content-Disposition: attachment;filename=oss_download.jpg 
    Date: Tue, 08 Nov 2016 02:00:25 GMT 
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:kZoYNv66bsmc10+dcGKw5x2****= x-oss-symlink-target: oss****
    x-oss-storage-class: Standard
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 08 Nov 2016 02:00:25 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 582131B9109F4EE66CDE56A5
    ETag: "0A477B89B4602AA8DECB8E19BFD4****"
    ```

-   版本控制请求示例

    ```
    PUT /link-to-oss.jpg?symlink HTTP/1.1 
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com 
    Date: Tue, 09 Apr 2019 06:50:48 GMT 
    Authorization: OSS o3shiyktjw16xw1:NVXXKiyUJ2tg07PxINinU0eO****
    x-oss-symlink-target: oss.jpg
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 09 Apr 2019 06:50:48 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-version-id: CAEQNRiBgMClj7qD0BYiIDQ5Y2QyMjc3NGZkODRlMTU5M2VkY2U3MWRiNGRh****
    x-oss-request-id: 5CAC40C8B7AEADE01700064B
    ETag: "136A5E127272200EDAB170DD84DE****"
    ```


## SDK

PutSymlink接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/管理文件/管理软链接.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/管理文件/管理软链接.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/管理文件/管理软链接.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/管理文件/管理软链接.md)
-   [C](/intl.zh-CN/SDK 示例/C/管理文件/管理软链接.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/管理文件/管理软链接.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|InvalidArgument|400|StorageClass的值不合法。|
|FileAlreadyExists|409|当请求Header中携带x-oss-forbid-overwrite=true时，表示禁止覆盖同名文件。如果文件已存在，则返回此错误。|
|FileImmutable|409|Bucket内的数据处于被保护状态时，若您尝试删除或修改这些数据，将返回此错误码。|

