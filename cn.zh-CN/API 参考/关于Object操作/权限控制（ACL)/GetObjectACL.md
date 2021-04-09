# GetObjectACL

调用GetObjectACL接口获取存储空间（Bucket）下某个文件（Object）的访问权限（ACL）。

## 版本控制

调用GetObjectACL接口时，默认只能获取Object当前版本的ACL。您可以通过指定versionId参数来获取指定Object版本的ACL。如果Object的对应版本为删除标记（Delete Marker），则OSS将返回404 Not Found。

**说明：** 如果一个Object从未设置过ACL，则调用GetObjectACL时，返回的ObjectACL为default，表示该Object的ACL遵循Bucket ACL。即如果Bucket的访问权限是private，则该Object的访问权限也是private。

## 请求语法

```
GET /ObjectName?acl HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|AccessControlList|容器|存储ACL信息的容器。父节点：AccessControlPolicy |
|AccessControlPolicy|容器|保存Get Object ACL结果的容器。父节点：None |
|DisplayName|字符串|Bucket拥有者的名称（目前和用户ID一致）。父节点：AccessControlPolicy.Owner |
|Grant|枚举字符串|Object的ACL权限。有效值：private，public-read，public-read-write

父节点：AccessControlPolicy.AccessControlList |
|ID|字符串|Bucket拥有者的用户ID。父节点：AccessControlPolicy.Owner |
|Owner|容器|保存Bucket拥有者信息的容器。父节点：AccessControlPolicy |

## 示例

-   未开启版本控制

    请求示例

    ```
    GET /test-object?acl HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Wed, 29 Apr 2015 05:21:12 GMT
    Content-Length: 253
    Content-Tupe: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" ?>
    <AccessControlPolicy>
        <Owner>
            <ID>00220120222</ID>
            <DisplayName>00220120222</DisplayName>
        </Owner>
        <AccessControlList>
            <Grant>public-read </Grant>
        </AccessControlList>
    </AccessControlPolicy>
    ```

-   已启用版本控制

    请求示例

    ```
    GET /example?acl&versionId=CAEQMhiBgMC1qpSD0BYiIGQ0ZmI5ZDEyYWVkNTQwMjBiNTliY2NjNmY3ZTVk**** HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 06:30:10 GMT
    Authorization: OSS qctg2ns3l8u51iu:w4DK66Kb/0M9GJKdsrpNs8l1****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-version-id: CAEQMhiBgMC1qpSD0BYiIGQ0ZmI5ZDEyYWVkNTQwMjBiNTliY2NjNmY3ZTVk****
    x-oss-request-id: 5CAC3BF2B7AEADE017000621
    Date: Tue, 09 Apr 2019 06:30:10 GMT
    Content-Length: 261
    Content-Tupe: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <AccessControlPolicy>
      <Owner>
        <ID>1234513715092****</ID>
        <DisplayName>1234513715092****</DisplayName>
      </Owner>
      <AccessControlList>
        <Grant>public-read</Grant>
      </AccessControlList>
    </AccessControlPolicy>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/管理文件/管理文件访问权限.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/管理文件/管理文件访问权限.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/管理文件/管理文件访问权限.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/管理文件/管理文件访问权限.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/管理文件/管理文件访问权限.md)

## 错误码

|错误码|HTTP状态码|错误信息|描述|
|:--|:------|:---|:-|
|AccessDenied|403|You do not have read acl permission on this object.|没有操作权限。只有Bucket的拥有者才能使用GetObjectACL接口来获取该Bucket下某个Object的ACL。|

