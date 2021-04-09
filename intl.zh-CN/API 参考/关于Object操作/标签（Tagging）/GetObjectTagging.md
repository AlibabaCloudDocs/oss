# GetObjectTagging

调用GetObjectTagging接口获取对象（Object）的标签（Tagging）信息。

## 版本控制

调用GetObjectTagging接口时，默认只能获取Object当前版本的标签信息。您可以通过指定versionId参数来获取指定Object版本的标签信息。如果Object的对应版本为删除标记（Delete Marker），则OSS将返回404 Not Found。

## 请求语法

```
GET /objectname?tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 20 Mar 2019 02:02:36 GMT
Authorization: SignatureValue
```

## 请求头

此接口仅包含公共请求头。关于公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅包含公共响应头。关于公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|描述|
|--|--|--|
|Tagging|容器|标签集合。 子节点：TagSet |
|TagSet|容器|标签集合。 父节点：Tagging

子节点：Tag |
|Tag|容器|标签集合。 父节点：TagSet

子节点：Key、Value |
|Key|字符串|标签键。 父节点：Tag

子节点：无 |
|Value|字符串|标签值。 父节点：Tag

子节点：无 |

## 示例

-   未开启版本控制

    在未开启版本控制的情况下，针对存储空间bucketname中的对象objectname发起GET请求时，获取到\{a:1\}和\{b:2\}的标签信息。标签获取成功后返回200 \(OK\)。

    请求示例

    ```
    GET /objectname?tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 20 Mar 2019 02:02:36 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    返回示例

    ```
    200 (OK)
    content‐length: 209
    server: AliyunOSS
    x‐oss‐request‐id: 5C919F38461FB4282600****
    date: Wed, 20 Mar 2019 02:02:32 GMT
    content‐type: application/xml
    <?xml version="1.0" encoding="UTF‐8"?>
    <Tagging>
      <TagSet>
        <Tag>
          <Key>a</Key>
          <Value>1</Value>
        </Tag>
        <Tag>
          <Key>b</Key>
          <Value>2</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

-   已启用版本控制

    在启用版本控制的情况下，针对存储空间bucketname中的对象objectname的指定版本（即请求示例中的versionId）发起GET请求时，获取到\{age:18\}的标签信息。标签获取成功后返回200 \(OK\)。

    请求示例

    ```
    GET /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 08:50:28 GMT
    Authorization: OSS ************:********************
    ```

    返回示例

    ```
    200 (OK)
    content-length: 161
    server: AliyunOSS
    x-oss-request-id: 5EF313D44506783438F3****
    date: Wed, 24 Jun 2020 08:50:28 GMT
    content-type: application/xml
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    <?xml version="1.0" encoding="UTF-8"?>
    <Tagging>
      <TagSet>
        <Tag>
          <Key>age</Key>
          <Value>18</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```


## SDK

GetObjectTagging接口对应的各语言SDK示例如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/对象标签/获取对象标签.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/对象标签/获取对象标签.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/对象标签/获取对象标签.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/对象标签/获取对象标签.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/对象标签/获取对象标签.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/对象标签/获取对象标签.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|FileAlreadyExists|409|Bucket开启分层命名空间后，当您要在该Bucket中获取某个Object的标签信息时，如果设置的Object为目录，则返回该错误。|

