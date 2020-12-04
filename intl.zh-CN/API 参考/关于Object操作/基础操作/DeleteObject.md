# DeleteObject

DeleteObject用于删除某个文件（Object）。

## 注意事项

调用DeleteObject接口时，有如下注意事项：

-   您需要对要删除的Object有写权限。
-   无论要删除的Object是否存在，删除成功后均会返回204状态码。
-   如果Object类型为软链接，使用DeleteObject仅会删除该软链接。

## 版本控制

版本控制状态下的删除行为说明如下：

-   未指定versionId（临时删除）：

    如果在未指定versionId的情况下执行删除操作时，默认不会删除Object的当前版本，而是对当前版本插入删除标记（Delete Marker）。此时，在未指定versionId的情况下执行GetObject操作，OSS会检测到当前版本为删除标记，并返回`404 Not Found`。此外，响应中还会返回`header：x-oss-delete-marker = true`以及新生成的删除标记的版本号`x-oss-version-id`。

    `x-oss-delete-marker`的值为true，表示与返回的`x-oss-version-id`对应的版本为删除标记。

-   指定versionId（永久删除）：

    如果在指定versionId的情况下执行删除操作时，OSS会根据`params`中指定的`versionId`参数永久删除该版本。如果要删除ID为“null”的版本，请在`params`参数中添加`params['versionId'] = “null”`，OSS将“null”字符串当成“null”的versionId，从而删除versionId为“null”的Object。


## 请求头

此接口涉及的公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

|名称|类型|示例值|描述|
|:-|:-|---|:-|
|x-oss-delete-marker|布尔型|true|-   未指定versionId执行DeleteObject 操作时，OSS会创建删除标记，响应中会返回此Header，且值为true。
-   指定versionId来永久删除指定Object版本时，如果该版本是删除标记，响应中会返回此Header，且值为true。

有效值：true|
|x-oss-version-id|字符串|CAEQMxiBgIDh3ZCB0BYiIGE4YjIyMjExZDhhYjQxNzZiNGUyZTI4ZjljZDcz\*\*\*\*|-   未指定versionId执行DeleteObject操作时，OSS会创建删除标记，响应中会返回此Header，表示新创建的删除标记的versionId。
-   指定versionId来永久删除Object指定版本时，响应中会返回此Header，表示删除Object的versionId。 |

有关其他公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求语法

```
DELETE /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: Wed, 02 Jan 2019 13:28:38 GMT
Authorization: SignatureValue
```

## 示例

-   请求示例

    ```
    DELETE /AK.txt HTTP/1.1
    Host: test.oss-cn-zhangjiakou.aliyuncs.com
    Accept-Encoding: identity
    User-Agent: aliyun-sdk-python/2.6.0(Windows/7/AMD64;3.7.0)
    Accept: text/html
    Connection: keep-alive
    date: Wed, 02 Jan 2019 13:28:38 GMT
    authorization: OSS qn6qrrqxo2oawuk53otfjbyc:zUglwRPGkbByZxm1+y4eyu+NIUs=zV0****
    Content-Length: 0
    ```

    返回示例

    ```
    HTTP/1.1 204 No Content
    Server: AliyunOSS
    Date: Wed, 02 Jan 2019 13:28:38 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5C2CBC8653718B5511EF4535
    x-oss-server-time: 134
    ```

-   未指定versionId执行DeleteObject的请求示例

    此时，OSS中会插入删除标记，响应中返回了x-oss-delete-marker=true。

    ```
    DELETE /example HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:08:23 GMT
    Authorization: OSS twnetzwjkqr9eq6:z73SSKA6t2tNTP4GuPjPiyV/****
    ```

    返回示例

    ```
    HTTP/1.1 204 NoContent
    x-oss-delete-marker: true
    x-oss-version-id: CAEQMxiBgIDh3ZCB0BYiIGE4YjIyMjExZDhhYjQxNzZiNGUyZTI4ZjljZDcz****
    x-oss-request-id: 5CAC1AB7B7AEADE01700****
    Date: Tue, 09 Apr 2019 04:08:23 GMT
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   指定versionId执行DeleteObject操作的请求示例

    以下示例中通过指定versionId来执行DeleteObject操作时，将永久删除该指定versionId的Object。

    ```
    DELETE /example?versionId=CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2**** HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:11:54 GMT
    Authorization: OSS gb3m2qiwirupd6v:UjOXBmIbJD3qXL+DP1EDNyCI****
    ```

    返回示例

    ```
    HTTP/1.1 204 No Content
    x-oss-version-id: CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2****
    x-oss-request-id: 5CAC1B8AB7AEADE01700****
    Date: Tue, 09 Apr 2019 04:11:54 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   指定versionId删除“删除标记”的请求示例

    以下示例中指定删除的版本为删除标记，则响应中将返回`x-oss-delete-marker=true`。

    ```
    DELETE /example?versionId=CAEQOBiBgIDNlJeB0BYiIDAwYjJlNDQ4YjJkMzQxMmY5NTM5N2UzZWNiZTQ2**** HTTP/1.1
    Host: versioning-delete.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 04:16:25 GMT
    Authorization: OSS jh475i54ffozhoy:4tX6Z+fnhtINhp0g+sRiLEQb****
    ```

    返回示例

    ```
    HTTP/1.1 204 No Content
    x-oss-delete-marker: true
    x-oss-version-id: CAEQNhiBgIDFtp.B0BYiIDk4NzgwMmU4NDMyOTQyM2NiMDQxOTcxYWNhMjc1****
    x-oss-request-id: 5CAC1C99B7AEADE01700****
    Date: Tue, 09 Apr 2019 04:16:25 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

DeleteObject接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/管理文件/删除文件.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/管理文件/删除文件.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/管理文件/删除文件.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/管理文件/删除文件.md)
-   [C](/intl.zh-CN/SDK 示例/C/管理文件/删除文件.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/管理文件/删除文件.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/管理文件/删除文件.md)
-   [iOS](/intl.zh-CN/SDK 示例/iOS/管理文件/删除文件.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/管理文件/删除文件.md)
-   [Browser.js](/intl.zh-CN/SDK 示例/Browser.js/管理文件.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|FileImmutable|409|Bucket内的数据处于被保护状态时，若您尝试删除或修改这些数据，将返回此错误码。|

