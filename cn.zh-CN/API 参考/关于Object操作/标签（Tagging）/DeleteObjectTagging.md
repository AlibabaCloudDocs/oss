# DeleteObjectTagging

调用DeleteObjectTagging接口删除指定对象（Object）的标签（Tagging）信息。

## 版本控制

调用DeleteObjectTagging接口时，默认只能删除Object当前版本的标签信息。您可以通过指定versionId参数来删除指定Object版本的标签信息。如果Object的对应版本为删除标记（Delete Marker），则OSS将返回404 Not Found。

## 请求语法

```
DELETE /objectname?tagging
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Mon, 18 Mar 2019 08:25:17 GMT
Authorization: SignatureValue
```

## 请求头

此接口仅包含公共请求头。关于公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅包含公共响应头。关于公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   未开启版本控制

    在未开启版本控制的情况下，通过DELETE请求删除了存储空间bucketname中对象objectname已有的标签信息，OSS解析此请求后删除此对象中的所有标签。

    请求示例

    ```
    DELETE /objectname?tagging
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:00:33 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    返回示例

    ```
    204 (No Content)
    content‐length: 0
    server: AliyunOSS
    x‐oss‐request‐id: 5CAC0AD16D0232E2051B****
    date: Tue, 09 Apr 2019 03:00:33 GMT
    ```

-   已启用版本控制

    在启用版本控制的情况下，通过DELETE请求删除存储空间bucketname中对象objectname已有的标签信息，删除时指定了对象版本号（即请求示例中的versionId），OSS解析此请求后删除此版本对象中的所有标签。

    请求示例

    ```
    DELETE /objectname?tagging&versionId=CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    Host: BucketName.oss-cn-hangzhou.aliyuncs.com
    Date: Wed, 24 Jun 2020 09:01:09 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:kZoYNv66bsmc10+dcGKw5x2P****
    ```

    返回示例

    ```
    204 (No Content)
    content-length: 0
    server: AliyunOSS
    x-oss-request-id: 5EF3165525D95C3338E8****
    date: Wed, 24 Jun 2020 09:01:09 GMT
    x-oss-version-id: CAEQExiBgID.jImWlxciIDQ2ZjgwODIyNDk5MTRhNzBiYmQwYTZkMTYzZjM0****
    ```


## SDK

DeleteObjectTagging接口对应的各语言SDK示例如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/对象标签/删除对象标签.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/对象标签/删除对象标签.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/对象标签/删除对象标签.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/对象标签/删除对象标签.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/对象标签/删除对象标签.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/对象标签/删除对象标签.md)

