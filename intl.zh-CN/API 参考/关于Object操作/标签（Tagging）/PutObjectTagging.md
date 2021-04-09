# PutObjectTagging

调用PutObjectTagging接口设置或更新对象（Object）的标签（Tagging）信息。对象标签使用一组键值对（Key-Value）标记对象。

## 注意事项

-   单个Object最多能设置10个标签，Key不能重复。
-   每个Key长度不能超过128字节，每个Value长度不能超过256字节。
-   Key和Value区分大小写。
-   标签的合法字符集包括大小写字母、数字、空格和以下符号：

    +‑=.\_:/

    通过HTTP header的方式设置标签且标签中包含任意字符时，您需要对标签的Key和Value进行URL编码。

-   更改标签信息不会更新Object的Last‑Modified时间。

关于对象标签的更多信息，请参见[对象标签](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/对象标签.md)。

## 版本控制

调用PutObjectTagging接口时，默认设置Object当前版本的标签信息。您可以通过指定versionId参数来设置指定Object版本的标签信息。如果Object的对应版本为删除标记（Delete Marker），则OSS将返回404 Not Found。

## 请求语法

```
PUT /objectname?tagging
Content‐Length: 114
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Mon, 18 Mar 2019 08:25:17 GMT
Authorization: SignatureValue
<Tagging>
  <TagSet>
    <Tag>
      <Key>Key</Key>
      <Value>Value</Value>
    </Tag>
  </TagSet>
</Tagging>            
```

## 请求元素

|名称|类型|是否必需|描述|
|--|--|----|--|
|Tagging|容器|是|标签集合。 子节点：TagSet |
|TagSet|容器|是|标签集合。 父节点：Tagging

子节点：Tag |
|Tag|容器|否|标签集合。 父节点：TagSet

子节点：Key、Value |
|Key|字符串|否|标签键。 父节点：Tag

子节点：无 |
|Value|字符串|否|标签值。 父节点：Tag

子节点：无 |

## 示例

-   未开启版本控制

    在未开启版本控制情况下，针对存储空间bucketname中的对象objectname，通过PUT请求设置\{a:1\}和\{b:2\}两个标签。标签设置成功后返回200 \(OK\)。

    请求示例

    ```
    PUT /objectname?tagging
    Content‐Length: 114
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Mon, 18 Mar 2019 08:25:17 GMT
    Authorization: OSS ***********:*********************
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

    返回示例

    ```
    200 (OK)
    content‐length: 0
    server: AliyunOSS
    x‐oss‐request‐id: 5C8F55ED461FB4A64C00****
    date: Mon, 18 Mar 2019 08:25:17 GMT
    ```

-   已启用版本控制

    在启用了版本控制的情况下，针对存储空间bucketname中的对象objectname的指定版本（即请求示例中的versionId），通过PUT请求设置\{age:18\}标签。标签设置成功后返回200 \(OK\)。

    请求示例

    ```
    PUT /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Content-Length: 90
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 08:58:15 GMT
    Authorization: OSS ************:********************
    <Tagging>
      <TagSet>
        <Tag>
          <Key>age</Key>
          <Value>18</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

    返回示例

    ```
    200 (OK)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF315A7FBD3EC3232B4****
    date: Wed, 24 Jun 2020 08:58:15 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDK

PutObjectTagging接口对应的各语言SDK示例如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/对象标签/设置对象标签.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/对象标签/设置对象标签.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/对象标签/设置对象标签.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/对象标签/设置对象标签.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/对象标签/设置对象标签.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/对象标签/设置对象标签.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|FileAlreadyExists|409|Bucket开启分层命名空间后，当您要在该Bucket中设置或者更改Object的标签信息时，如果设置的Object为目录，则返回该错误。|

