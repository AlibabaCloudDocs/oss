# CompleteMultipartUpload

在将所有数据Part都上传完成后，必须调用CompleteMultipartUpload接口来完成整个文件的分片上传。

## 注意事项

调用CompleteMultipartUpload操作时，用户必须提供所有有效的Part列表（包括PartNumber和ETag）。OSS收到用户提交的Part列表后，会逐一验证每个Part的有效性。当所有的Part验证通过后，OSS将把这些Part组合成一个完整的Object。

-   确认Part的大小

    CompleteMultipartUpload时会确认除最后一个Part以外所有Part的大小是否都大于或等于100 KB，并检查用户提交的Part列表中的每一个PartNumber和ETag。所以在上传Part时，客户端除了需要记录Part号码外，还需要记录每次上传Part成功后服务器返回的ETag值。

-   处理请求

    由于OSS处理CompleteMultipartUpload请求时会持续一定的时间。在这段时间内，如果客户端与OSS之间连接中断，OSS仍会继续该请求。

-   PartNumber

    服务端在调用CompleteMultipartUpload接口时会对PartNumber做校验。

    PartNumber取值为1~10000。PartNumber可以不连续，但必须升序排列。例如第一个Part的PartNumber是1，第二个Part的PartNumber可以是5。

-   UploadId

    同一个Object可以同时拥有不同的UploadId，当Complete一个UploadId后，此UploadId将无效，但该Object的其他UploadId不受影响。

-   x-oss-server-side-encryption请求头

    若调用InitiateMultipartUpload接口时，指定了x-oss-server-side-encryption请求头，则在CompleteMultipartUpload的响应头中返回x-oss-server-side-encryption，其值表明该Object的服务器端加密算法。


## 版本控制

在开启了版本控制的状态下，调用CompleteMultipartUpload接口来完成整个文件的MultipartUpload，OSS会为整个文件生成唯一的版本ID，并在响应header中以x-oss-version-id的形式返回。

## 请求语法

```
POST /ObjectName?uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: Signature
<CompleteMultipartUpload>
<Part>
<PartNumber>PartNumber</PartNumber>
<ETag>ETag</ETag>
</Part>
...
</CompleteMultipartUpload>
```

## 请求参数（Request Parameters）

调用CompleteMultipartUpload时，可以通过Encoding-type对返回结果中的Key进行编码。

|名称|类型|描述|
|:-|:-|:-|
|encoding-type|字符串|指定对返回的Key进行编码，目前支持URL编码。Key使用UTF-8字符，但XML 1.0标准不支持解析一些控制字符，例如ascii值从0到10的字符。对于Key中包含XML 1.0标准不支持的控制字符，可以通过指定Encoding-type对返回的Key进行编码。

默认值：无

可选值：url |

其他公共请求头例如Host、Date等，详情请参见[公共HTTP头定义](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求头

|名称|类型|是否必选|描述|
|--|--|----|--|
|x-oss-forbid-overwrite|字符串|否|指定CompleteMultipartUpload操作时是否覆盖同名Object。 -   不指定x-oss-forbid-overwrite时，默认覆盖同名Object。
-   指定x-oss-forbid-overwrite为true时，表示禁止覆盖同名Object；指定x-oss-forbid-overwrite为false时，表示允许覆盖同名Object。

**说明：**

-   当目标Bucket的版本控制状态为“开启”或“暂停”时，x-oss-forbid-overwrite请求Header设置无效，即允许覆盖同名Object。
-   设置x-oss-forbid-overwrite请求Header会导致QPS处理性能下降，如果您有大量的操作需要使用x-oss-forbid-overwrite请求Header（QPS \> 1000），请工单联系我们进行确认，避免影响您的业务。 |
|x-oss-complete-all:yes|字符串|否|指定是否列举当前UploadId已上传的所有Part。-   若指定了x-oss-complete-all:yes，则OSS会列举当前UploadId已上传的所有Part，然后按照PartNumber的序号排序并执行CompleteMultipartUpload操作。执行CompleteMultipartUpload过程中无法检测正在上传或者漏传的Part，因此用户需要自己确保Part的完整性。
-   若指定了x-oss-complete-all:yes，则不允许继续指定body，否则报错。
-   若指定了x-oss-complete-all:yes，response的格式保持不变。 |

## 请求元素

|名称|类型|描述|
|:-|:-|:-|
|CompleteMultipartUpload|容器|保存CompleteMultipartUpload请求内容的容器。 子节点：一个或多个Part元素

父节点：无 |
|ETag|字符串|Part成功上传后，OSS返回的ETag值。 父节点：Part |
|Part|容器|保存已经上传Part信息的容器。 子节点：ETag、PartNumber

父节点：CompleteMultipartUpload |
|PartNumber|整数|Part数目父节点：Part |

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|Bucket|字符串|Bucket名称父节点：CompleteMultipartUploadResult |
|CompleteMultipartUploadResult|容器|保存Complete Multipart Upload请求结果的容器。 子节点：Bucket、Key、ETag、Location

父节点：None |
|ETag|字符串|Object生成时会创建相应的ETag ，ETag用于标识一个Object的内容。通过CompleteMultipartUpload请求创建的Object，ETag值是其内容的UUID。

**说明：** ETag值可以用于检查Object内容是否发生变化。不建议使用ETag作为Object内容的MD5来校验数据完整性。

父节点：CompleteMultipartUploadResult |
|Location|字符串|新创建Object的URL。 父节点：CompleteMultipartUploadResult |
|Key|字符串|新创建Object的名字。 父节点：CompleteMultipartUploadResult |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求参数中指定了Encoding-type，那返回的结果会对Key进行编码。 父节点：容器 |

## 示例

-   请求示例

    ```
    POST /multipart.data?uploadId=0004B9B2D2F7815C432C9057C031****&encoding-type=url HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 1056
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:8VwFhFUWmVecK6jQlHlXMK/z****
    <CompleteMultipartUpload> 
        <Part> 
            <PartNumber>1</PartNumber>  
            <ETag>"3349DC700140D7F86A0784842780****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>5</PartNumber>  
            <ETag>"8EFDA8BE206636A695359836FE0A****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>8</PartNumber>  
            <ETag>"8C315065167132444177411FDA14****"</ETag> 
        </Part> 
    </CompleteMultipartUpload>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Content-Length: 329
    Content-Type: Application/xml
    Connection: keep-alive
    x-oss-request-id: 594f0751-3b1e-168f-4501-4ac71d21****
    Date: Fri, 24 Feb 2012 10:19:18 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <CompleteMultipartUploadResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <EncodingType>url</EncodingType>
        <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com /multipart.data</Location>
        <Bucket>oss-example</Bucket>
        <Key>multipart.data</Key>
        <ETag>"B864DB6A936D376F9F8D3ED3BBE540****"</ETag>
    </CompleteMultipartUploadResult>
    ```

-   版本控制请求示例

    ```
    POST /multipart.data?uploadId=63C06A5CFF6F4AE4A6BB3AD7F01C****  HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 223
    Date: Tue, 09 Apr 2019 07:01:56 GMT
    Authorization: OSS 6jftttm6x6s****:XljBrYBYxDnxKdFMj9WYI6qu****
    <CompleteMultipartUpload> 
        <Part> 
            <PartNumber>1</PartNumber>  
            <ETag>"25A9F4ABFCC05743DF6E2C886C56****"</ETag> 
        </Part>  
        <Part> 
            <PartNumber>5</PartNumber>  
            <ETag>"25A9F4ABFCC05743DF6E2C886C56****"</ETag> 
        </Part>  
    </CompleteMultipartUpload>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Content-Length: 314
    Content-Type: Application/xml
    Connection: keep-alive
    x-oss-version-id: CAEQMxiBgID6v86D0BYiIDc3ZDI0YTBjZGQzYjQ2Mjk4OWVjYWNiMDljYzhlN****
    x-oss-request-id: 5CAC4364B7AEADE017000662
    Date: Tue, 09 Apr 2019 07:01:56 GMT
    <?xml version="1.0" encoding="UTF-8"?>
    <CompleteMultipartUploadResult>
      <Location>http://oss-example.oss-cn-hangzhou.aliyuncs.com/multipart.data</Location>
      <Bucket>oss-example</Bucket>
      <Key>multipart.data</Key>
      <ETag>"097DE458AD02B5F89F9D0530231876****"</ETag>
    </CompleteMultipartUploadResult>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/上传文件/分片上传.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/上传文件/分片上传.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/上传文件/分片上传.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/上传文件/分片上传.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/上传文件/分片上传.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/上传文件/分片上传.md)
-   [C](/cn.zh-CN/SDK 示例/C/上传文件/分片上传.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/上传文件/分片上传.md)
-   [Android](/cn.zh-CN/SDK 示例/Android/上传文件/分片上传.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|InvalidDigest|400|为了保证数据在网络传输过程中不出现错误，用户发送请求时可以携带Content-MD5，OSS计算上传数据的MD5与用户上传的MD5值不一致。|
|FileAlreadyExists|409|当请求的Header中携带x-oss-forbid-overwrite=true时，表示禁止覆盖同名文件。如果文件已存在，则返回此错误。|

