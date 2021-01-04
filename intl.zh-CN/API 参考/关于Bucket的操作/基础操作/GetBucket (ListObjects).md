# GetBucket \(ListObjects\)

GetBucket \(ListObjects\)接口用于列举存储空间（Bucket）中所有文件（Object）的信息。

## 注意事项

调用该接口时，有如下注意事项：

-   GetBucket \(ListObjects\)接口已修订为GetBucketV2 \(ListObjectsV2\)。建议您在开发应用程序时使用较新的版本GetBucketV2 \(ListObjectsV2\)。为保证向后兼容性，OSS继续支持GetBucket \(ListObjects\)。有关GetBucketV2 \(ListObjectsV2\)的更多信息，请参见[GetBucketV2 \(ListObjectsV2\)](/intl.zh-CN/API 参考/关于Bucket的操作/基础操作/GetBucketV2 (ListObjectsV2).md)。
-   执行GetBucket \(ListObjects\)请求时不会返回Object中自定义的元信息。

## 请求语法

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

此接口仅涉及公共请求头，例如`Authorization`、`Host`等更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求参数

|名称|类型|是否必选|示例值|描述|
|:-|:-|:---|---|:-|
|delimiter|字符串|否|/|对Object名字进行分组的字符。所有Object名字包含指定的前缀，第一次出现delimiter字符之间的Object作为一组元素（即CommonPrefixes）。 默认值：无 |
|marker|字符串|否|test1.txt|设定从marker之后按字母排序开始返回Object。 marker用来实现分页显示效果，参数的长度必须小于1024字节。

做条件查询时，即使marker在列表中不存在，也会从符合marker字母排序的下一个开始打印。

默认值：无 |
|max-keys|字符串|否|200|指定返回Object的最大数。 如果因为max-keys的设定无法一次完成列举，返回结果会附加NextMarker元素作为下一次列举的marker。取值：大于0小于1000

默认值：100 |
|prefix|字符串|否|fun|限定返回文件的Key必须以prefix作为前缀。 -   prefix参数的长度必须小于1024字节。
-   使用prefix查询时，返回的Key中仍会包含prefix。

如果把prefix设为某个文件夹名，则列举以此Prefix开头的文件，即该文件夹下递归的所有文件和子文件夹。

在设置prefix的基础上，将delimiter设置为正斜线（/）时，返回值中只列举该文件夹下的文件，文件夹下的子文件夹名返回在CommonPrefixes中，子文件夹下递归的所有文件和文件夹不显示。

例如，一个Bucket中有三个Object ，分别为fun/test.jpg、 fun/movie/001.avi和fun/movie/007.avi。如果设定prefix为fun/，则返回三个Object；如果在prefix设置为fun/的基础上，将delimiter设置为正斜线（/），则返回fun/test.jpg和fun/movie/。

默认值：无 |
|encoding-type|字符串|否|URL|对返回的内容进行编码并指定编码的类型。 默认值：无

可选值：URL

**说明：** delimiter、marker、prefix、NextMarker以及Key使用UTF-8字符。如果delimiter、marker、prefix、NextMarker以及Key中包含XML 1.0标准不支持的控制字符，您可以通过指定encoding-type对返回结果中的Delimiter、Marker、Prefix、NextMarker以及Key进行编码。 |

## 响应元素

|名称|类型|示例值|描述|
|--|--|---|--|
|Contents|容器|不涉及|保存每个返回Object元信息的容器。 父节点：ListBucketResult |
|CommonPrefixes|字符串|不涉及|如果请求中指定了Delimiter参数，则会在返回的响应中包含CommonPrefixes元素。该元素表明以Delimiter结尾，并有共同前缀的Object名称的集合。 父节点：ListBucketResult |
|Delimiter|字符串|/|对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现Delimiter字符之间的Object作为一组元素CommonPrefixes。 父节点：ListBucketResult |
|EncodingType|字符串|不涉及|指明了返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，则会对返回结果中的Delimiter、Marker、Prefix、NextMarker和Key这些元素进行编码。 父节点：ListBucketResult |
|DisplayName|字符串|user\_example|Object拥有者名称。 父节点：ListBucketResult.Contents.Owner |
|ETag|字符串|5B3C1A2E053D763E1B002CC607C5A0FE1\*\*\*\*|ETag \(Entity Tag\) 在每个Object生成时创建，用于标识一个Object的内容。 -   对于PutObject请求创建的Object，ETag值是其内容的MD5值。
-   对于其他方式创建的Object，ETag值是其内容的UUID。
-   ETag值可以用于检查Object内容是否发生变化。不建议使用ETag值作为Object内容的MD5校验数据完整性的依据。

父节点：ListBucketResult.Contents |
|ID|字符串|0022012\*\*\*\*|Bucket拥有者的用户ID。 父节点：ListBucketResult.Contents.Owner |
|IsTruncated|枚举字符串|false|请求中返回的结果是否被截断。 返回值：true、false

-   true表示本次没有返回全部结果。
-   false表示本次已经返回了全部结果。

父节点：ListBucketResult |
|Key|字符串|fun/test.jpg|Object的Key。 父节点：ListBucketResult.Contents |
|LastModified|时间|2012-02-24T08:42:32.000Z|Object最后被修改的时间。 父节点：ListBucketResult.Contents |
|ListBucketResult|容器|不涉及|保存GetBucket请求结果的容器。 子节点：Name、Prefix、 Marker、MaxKeys、 Delimiter、IsTruncated、Nextmarker、Contents

父节点：None |
|Marker|字符串|test1.txt|标识此次GetBucket（ListObjects）的起点。 父节点：ListBucketResult |
|MaxKeys|字符串|100|响应请求内返回结果的最大数目。 父节点：ListBucketResult |
|Name|字符串|oss-example|Bucket名称。 父节点：ListBucketResult |
|Owner|容器|不涉及|保存Bucket拥有者信息的容器。 子节点：DisplayName、ID

父节点：ListBucketResult |
|Prefix|字符串|fun/|本次查询结果的前缀。 父节点：ListBucketResult |
|Size|字符串|344606|返回Object大小，单位为字节。 父节点：ListBucketResult.Contents |
|StorageClass|字符串|Standard|Object的存储类型。 父节点：ListBucketResult.Contents |

此接口涉及的其他公共响应头，例如`x-oss-request-id`、`Content-Type`等的更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   简单请求示例

    ```
    GET / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>oss.jpg</Key>
          <LastModified>2012-02-24T06:07:48.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user-example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   带prefix参数的请求示例

    ```
    GET /?prefix=fun HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykbo****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix>fun</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/movie/001.avi</Key>
          <LastModified>2012-02-24T08:43:07.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/movie/007.avi</Key>
          <LastModified>2012-02-24T08:43:27.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    </ListBucketResult>
    ```

-   带prefix和delimiter参数的请求示例

    ```
    GET /?prefix=fun/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix>fun/</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
          <Key>fun/test.jpg</Key>
          <LastModified>2012-02-24T08:42:32.000Z</LastModified>
          <ETag>"5B3C1A2E053D763E1B002CC607C5A0FE1****"</ETag>
          <Type>Normal</Type>
          <Size>344606</Size>
          <StorageClass>Standard</StorageClass>
          <Owner>
              <ID>0022012****</ID>
              <DisplayName>user_example</DisplayName>
          </Owner>
    </Contents>
    <CommonPrefixes>
          <Prefix>fun/movie/</Prefix>
    </CommonPrefixes>
    </ListBucketResult>
    ```

-   指定marker分页列举的请求示例

    此示例中指定了max-keys=2，即最多返回2条列举信息。

    ```
    GET /?max-keys=2&marker=test1.txt HTTP/1.1
    Host: test-bucket-xxx.oss-cn-shenzhen.aliyuncs.com
    Accept-Encoding: identity
    Accept: */*
    Connection: keep-alive
    User-Agent: aliyun-sdk-python/2.11.0(Darwin/18.2.0/x86_64;3.4.1)
    date: Tue, 26 May 2020 08:39:48 GMT
    authorization: OSS LTAIB1VW9xxxxxxx:MmY11jLlO8UlAqjqHK3Ckp****
    ```

    返回示例

    结果中的NextMarker可以做为下次列举请求的Marker。

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Tue, 26 May 2020 08:39:48 GMT
    Content-Type: application/xml
    Content-Length: 1032
    Connection: keep-alive
    x-oss-request-id: 5ECCD5D4881816373582xxx
    x-oss-server-time: 3
    
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult>
      <Name>test-bucket-xxx</Name>
      <Prefix></Prefix>
      <Marker>test1.txt</Marker>
      <MaxKeys>2</MaxKeys>
      <Delimiter></Delimiter>
      <EncodingType>url</EncodingType>
      <IsTruncated>true</IsTruncated>
      <NextMarker>test100.txt</NextMarker>
      <Contents>
        <Key>test10.txt</Key>
        <LastModified>2020-05-26T07:50:18.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
      <Contents>
        <Key>test100.txt</Key>
        <LastModified>2020-05-26T07:50:20.000Z</LastModified>
        <ETag>"C4CA4238A0B923820DCC509A6F75****"</ETag>
        <Type>Normal</Type>
        <Size>1</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>1305433xxx</ID>
          <DisplayName>1305433xxx</DisplayName>
        </Owner>
      </Contents>
    </ListBucketResult>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/管理文件/列举文件.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/管理文件/列举文件.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/管理文件/列举文件.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/管理文件/列举文件.md)
-   [C](/intl.zh-CN/SDK 示例/C/管理文件/列举文件.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/管理文件/列举文件.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/管理文件/列举文件.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/管理文件/概述.md)
-   [Browser.js](/intl.zh-CN/SDK 示例/Browser.js/管理文件.md)
-   [Ruby](/intl.zh-CN/SDK 示例/Ruby/管理文件.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有访问该Bucket的权限。|
|InvalidArgument|400|-   max-keys小于0或者大于1000。
-   prefix、marker、delimiter参数的长度不符合要求。 |

