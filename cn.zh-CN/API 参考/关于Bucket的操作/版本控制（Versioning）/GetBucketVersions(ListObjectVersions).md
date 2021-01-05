# GetBucketVersions\(ListObjectVersions\)

GetBucketVersions\(ListObjectVersions\)接口用于列出Bucket中包括删除标记（Delete Marker）在内的所有Object的版本信息。

## 注意事项

对于已开启版本控制的Bucket，调用GetBucketVersions\(ListObjectVersions\)与GetBucket\(ListObjects\)接口的返回结果区别如下：

-   GetBucket\(ListObjects\)接口仅返回Object的当前版本，且当前版本不能为删除标记。
-   GetBucketVersions\(ListObjectVersions\)接口返回Bucket中所有Object的所有版本。

    不同Object之间按字母排序返回，同一个Object的不同版本则按从新到旧排序，与版本ID的字母序无关。


## 请求头

此接口仅涉及公共请求头，例如`Authorization`、`Host`等更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求参数

调用GetBucketVersions\(ListObjectVersions\)接口时，可通过设置prefix、key-marker、version-id-marker、delimiter和max-keys参数限制仅返回部分结果。

|名称|类型|是否必选|示例值|描述|
|:-|:-|:---|---|:-|
|delimiter|字符串|否|/|对Object名字进行分组的字符。所有Object名字包含指定的前缀（prefix），第一次出现delimiter字符之间的Object作为一组元素（即CommonPrefixes）。如果将prefix设为文件夹名称后，再把delimiter设置为正斜线（/），则只返回该文件夹下的文件，该文件夹下的子文件名在CommonPrefixes中返回，子文件夹下递归的文件和文件夹不显示。

默认值：无 |
|key-marker|字符串|如果version-id-marker不为空，则key-marker不能为空|example|设定结果从key-marker之后按字母序开始返回，与version-id-marker组合使用。参数的长度必须小于1024字节。

默认值：无 |
|version-id-marker|字符串|否|CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1\*\*\*\*|设定结果从key-marker对象的version-id-marker之后按新旧版本排序开始返回。如果version-id-marker未设定，则默认从key-marker按字母序排序的下一个Key的第一个版本开始返回。 默认值：无

有效值：版本ID |
|max-keys|字符串|否|100|限定此次返回Object的最大个数。如果因为max-keys的设定无法一次完成列举，返回结果会附加`NextKeyMarker`和`NextVersionIdMarker`作为下一次列举的marker。列举结果中包含`NextKeyMarker`和`NextVersionIdMarker`的值。

取值：大于0小于1000

默认值：100 |
|prefix|字符串|否|fun|限定返回的Object Key必须以prefix作为前缀。-   prefix的长度必须小于1024字节。
-   使用prefix查询时，返回结果的Key仍会包含prefix。

如果将prefix设为某个文件夹名，则列举以此prefix开头的文件，即该文件夹下递归的所有的文件和子文件夹。

默认值：无 |
|encoding-type|字符串|否|URL|对返回的内容进行编码并指定编码类型。默认值：无

可选值：URL

**说明：** delimiter、marker、prefix、NextMarker以及Key使用UTF-8字符。如果delimiter、marker、prefix、NextMarker以及Key中包含XML 1.0标准不支持的控制字符，您可以通过指定encoding-type对返回结果中的Delimiter、Marker、Prefix、NextMarker以及Key进行编码。 |

## 响应元素

|名称|类型|示例值|描述|
|:-|:-|---|:-|
|ListVersionsResult|容器|不涉及|保存GetBucketVersions请求结果的容器。子节点：Name、Prefix、Marker、MaxKeys、Delimiter、IsTruncated、Nextmarker、Version、DeleteMarker

父节点：None |
|CommonPrefixes|字符串|不涉及|如果请求中指定了delimiter参数，则OSS返回的响应中包含CommonPrefixes元素。该元素标明以delimiter结尾，并有共同前缀的Object名称的集合。父节点：ListVersionsResult |
|Delimiter|字符串|/|用于对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现Delimiter字符之间的Object作为一组元素CommonPrefixes。父节点：ListVersionsResult |
|EncodingType|字符串|URL|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，则表示对返回结果中的Delimiter、Marker、Prefix、NextMarker和Key这些元素进行编码。父节点：ListVersionsResult |
|IsTruncated|字符串|true|指明是否已返回所有结果。 -   true：表示本次没有返回全部结果。
-   false：表示本次已返回全部结果。

有效值：true、false

父节点：ListVersionsResult |
|KeyMarker|字符串|example|标识此次GetBucketVersions的起点Object。父节点：ListVersionsResult |
|VersionIdMarker|字符串|CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1\*\*\*\*|与KeyMarker参数一同使用，以指定GetBucketVersions的起点。父节点：ListVersionsResult |
|NextKeyMarker|字符串|test|如果本次没有返回全部结果，响应请求中将包含NextKeyMarker元素，用于标明接下来请求的key-marker。父节点：ListVersionsResult |
|NextVersionIdMarker|字符串|CAEQGBiBgIC\_jq7P9xYiIDRiZWJkNjY2Y2Q4NDQ5ZTI5ZGE5ODIxMTIyZThl\*\*\*\*|如果本次没有返回全部结果，响应请求中将包含NextVersionIdMarker元素，用于标明接下来请求的version-id-marker。父节点：ListVersionsResult |
|MaxKeys|字符串|1000|响应请求内返回结果的最大数目。父节点：ListVersionsResult |
|Name|字符串|examplebucket-1250000000|Bucket名称。父节点：ListVersionsResult |
|Owner|容器|不涉及|保存Bucket拥有者信息的容器。父节点：ListVersionsResult |
|Prefix|字符串|fun|本次查询结果的前缀。父节点：ListVersionsResult |
|Version|容器|不涉及|保存除删除标记以外的Object版本的容器。父节点：ListVersionsResult |
|DeleteMarker|容器|不涉及|保存删除标记的容器。父节点：ListVersionsResult |
|ETag|字符串|250F8A0AE989679A22926A875F0A2\*\*\*\*|每个Object生成时创建的ETag ，用于标识Object的内容。 -   对于PutObject请求创建的Object，ETag值是其内容的MD5值。
-   对于其他方式创建的Object，ETag值是其内容的UUID。

**说明：** ETag值仅用于检查Object内容是否发生变化。不建议使用ETag值作为Object内容的MD5数据完整性校验的依据。

父节点：ListVersionsResult.Version |
|Key|字符串|example|Object的名称。父节点：ListVersionsResult.Version、ListVersionsResult.DeleteMarker |
|LastModified|时间|2019-04-09T07:27:28.000Z|Object最后被修改的时间。父节点：ListVersionsResult.Version、ListVersionsResult.DeleteMarker |
|VersionId|字符串|CAEQMxiBgMDNoP2D0BYiIDE3MWUxNzgxZDQxNTRiODI5OGYwZGMwNGY3MzZjN\*\*\*\*|Object的版本ID。父节点：ListVersionsResult.Version、 ListVersionsResult.DeleteMarker |
|IsLatest|字符串|true|Object是否为当前版本。取值：

-   true：Object为当前版本。
-   false：Object为非当前版本。

父节点：ListVersionsResult.Version、 ListVersionsResult.DeleteMarker |
|Size|字符串|93731|Object的字节数。父节点：ListVersionsResult.Version、ListVersionsResult.DeleteMarker |
|StorageClass|字符串|Standard|Object的存储类型。父节点：ListVersionsResult.Version、ListVersionsResult.DeleteMarker |
|DisplayName|字符串|12345125285864390|Object拥有者的名称。父节点：ListVersionsResult.Version.Owner、ListVersionsResult.DeleteMarker.Owner |
|ID|字符串|1234512528586\*\*\*\*|Bucket拥有者的用户ID。父节点：ListVersionsResult.Version.Owner、ListVersionsResult.DeleteMarker.Owner |

此接口涉及的其他公共响应头，例如`x-oss-request-id`、`Content-Type`等的更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   未启用版本控制

    请求示例

    ```
    GET /?versions HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76ov9cu:WFx4****+e7Rc0jawCsh7hlk****
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Content-Type: application/xml
    Content-Length: 1262
    Connection: keep-alive
    Date: Thu, Tue, 09 Apr 2019 07:27:48 GMT
    Server: AliyunOSS
    x-cos-request-id: 534B371674E88A4D8906****
    
    <ListVersionsResult>
        <Name>examplebucket-1250000000</Name>
        <Prefix/>
        <KeyMarker/>
        <VersionIdMarker/>
        <MaxKeys>1000</MaxKeys>
        <IsTruncated>false</IsTruncated>
        <Version>
            <Key>example-object-1.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-5T12:03:10.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B669CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-2.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-9T12:03:09.000Z</LastModified>
            <ETag>5B3C1A2E053D763E1B002CC607C5A0FE1****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>example-object-3.jpg</Key>
            <VersionId/>
            <IsLatest>true</IsLatest>
            <LastModified>2019-08-10T12:03:08.000Z</LastModified>
            <ETag>4B3F1A2E053D763E1B002CC607C5AGTRF****</ETag>
            <Size>20</Size>
            <StorageClass>STANDARD</StorageClass>
            <Owner>
                <ID>1250000000</ID>
                <DisplayName>1250000000</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```

-   已开启版本控制

    假设在名为oss example的Bucket中有example和pic.jpg 2个Object，其中example这个Object有3个版本，从新到旧排序的版本ID依次为111222、000123（删除标记）、222333 。pic.jpg这个Object仅有1个版本，其版本ID为232323 。

    列举文件时将key-marker设置为example，version-id-marker指定为111222 ，则按example 000123、example 222333、pic.jpg 232323的顺序返回3个版本。

    请求示例：

    ```
    GET /?versions&key-marker=example&version-id-marker=CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1**** HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Authorization: OSS ami4tq0x76o****:WFx4kLpx+e7Rc0jawCsh7hlk****
    ```

    返回示例：

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5CAC4974B7AEADE01700****
    Date: Tue, 09 Apr 2019 07:27:48 GMT
    Content-Type: application/xml
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListVersionsResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Name>oss-example</Name>
        <Prefix></Prefix>
        <KeyMarker>example</KeyMarker>
        <VersionIdMarker>CAEQMxiBgICbof2D0BYiIGRhZjgwMzJiMjA3MjQ0ODE5MWYxZDYwMzJlZjU1****</VersionIdMarker>
        <MaxKeys>100</MaxKeys>
        <Delimiter></Delimiter>
        <IsTruncated>false</IsTruncated>
        <DeleteMarker>
            <Key>example</Key>
            <VersionId>CAEQMxiBgICAof2D0BYiIDJhMGE3N2M1YTI1NDQzOGY5NTkyNTI3MGYyMzJm****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </DeleteMarker>
        <Version>
            <Key>example</Key>
            <VersionId>CAEQMxiBgMDNoP2D0BYiIDE3MWUxNzgxZDQxNTRiODI5OGYwZGMwNGY3MzZjN****</VersionId>
            <IsLatest>false</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"250F8A0AE989679A22926A875F0A2****"</ETag>
            <Type>Normal</Type>
            <Size>93731</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
        <Version>
            <Key>pic.jpg</Key>
            <VersionId>CAEQMxiBgMCZov2D0BYiIDY4MDllOTc2YmY5MjQxMzdiOGI3OTlhNTU0ODIx****</VersionId>
            <IsLatest>true</IsLatest>
            <LastModified>2019-04-09T07:27:28.000Z</LastModified>
            <ETag>"3663F7B0B9D3153F884C821E7CF4****"</ETag>
            <Type>Normal</Type>
            <Size>574768</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
              <ID>1234512528586****</ID>
              <DisplayName>12345125285864390</DisplayName>
            </Owner>
        </Version>
    </ListVersionsResult>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/版本控制/管理版本控制.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/版本控制/管理版本控制.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/版本控制/管理版本控制.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/版本控制/管理版本控制.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/版本控制/管理版本控制.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/版本控制/管理版本控制.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|NoSuchBucket|404|目标Bucket不存在，包括试图访问因命名不规范而无法创建的Bucket。|
|AccessDenied|403|无访问此Bucket的权限。|
|InvalidArgument|400|-   max-keys小于0或者大于1000。
-   prefix、marker、delimiter参数长度不符合要求。
-   version-id-marker为无效的版本ID。 |

