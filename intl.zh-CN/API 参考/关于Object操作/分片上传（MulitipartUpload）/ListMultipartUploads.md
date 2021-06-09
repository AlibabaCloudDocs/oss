# ListMultipartUploads

调用ListMultipartUploads接口列举所有执行中的Multipart Upload事件，即已经初始化但还未完成（Complete）或者还未中止（Abort）的Multipart Upload事件。

## 请求语法

```
Get /?uploads HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: Signature
```

## 请求参数

|名称|类型|描述|
|:-|:-|:-|
|delimiter|字符串|用于对Object名称进行分组的字符。所有名称包含指定的前缀且首次出现delimiter字符之间的Object作为一组元素CommonPrefixes。|
|max-uploads|字符串|限定此次返回Multipart Upload事件的最大个数，默认值为1000。最大值为1000。|
|key-marker|字符串|与upload-id-marker参数配合使用，用于指定返回结果的起始位置。 -   如果未设置upload-id-marker参数，查询结果中包含所有Object名称的字典序大于key-marker参数值的Multipart Upload事件。
-   如果设置了upload-id-marker参数，查询结果中包含：
    -   所有Object名称的字典序大于key-marker参数值的Multipart Upload事件。
    -   Object名称等于key-marker参数值，但是UploadId比upload-id-marker参数值大的Multipart Upload事件。 |
|prefix|字符串|限定返回的Object Key必须以prefix作为前缀。注意使用prefix查询时，返回的Key中仍会包含prefix。 **说明：** 您可以灵活地使用prefix参数对Bucket内的Object进行分组管理（类似文件夹功能）。 |
|upload-id-marker|字符串|与key-marker参数配合使用，用于指定返回结果的起始位置。 -   如果未设置key-marker参数，则OSS会忽略upload-id-marker参数。
-   如果设置了key-marker参数，查询结果中包含：
    -   所有Object名称的字典序大于key-marker参数值的Multipart Upload事件。
    -   Object名称等于key-marker参数值，但是UploadId比upload-id-marker参数值大的Multipart Upload事件。 |
|encoding-type|字符串|指定对返回的内容进行编码，指定编码的类型。Delimiter、KeyMarker、Prefix、NextKeyMarker和Key使用UTF-8字符，但xml 1.0标准不支持解析一些控制字符，例如ASCII值从0到10的字符。对于包含xml 1.0标准不支持的控制字符，可以通过指定encoding-type对返回的Delimiter、KeyMarker、Prefix、NextKeyMarker和Key进行编码。 默认值：无 |

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|ListMultipartUploadsResult|容器|保存ListMultipartUpload请求结果的容器。 子节点：Bucket, KeyMarker, UploadIdMarker, NextKeyMarker, NextUploadIdMarker, MasUploads, Delimiter, Prefix, CommonPrefixes, IsTruncated, Upload

父节点：None |
|Bucket|字符串|Bucket名称。 父节点：ListMultipartUploadsResult |
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求参数中指定了encoding-type，那返回的结果会对Delimiter、KeyMarker、Prefix、NextKeyMarker和Key这些元素进行编码。 父节点：ListMultipartUploadsResult |
|KeyMarker|字符串|列表的起始Object位置。 父节点：ListMultipartUploadsResult |
|UploadIdMarker|字符串|列表的起始UploadId位置。 父节点：ListMultipartUploadsResult |
|NextKeyMarker|字符串|如果本次没有返回全部结果，响应请求中将包含NextKeyMarker元素，用于表示接下来请求的KeyMarker值。 父节点：ListMultipartUploadsResult |
|NextUploadMarker|字符串|如果本次没有返回全部结果，响应请求中将包含NextUploadMarker元素，用于表示接下来请求的UploadMarker值。 父节点：ListMultipartUploadsResult |
|MaxUploads|整数|返回的最大Upload个数。 父节点：ListMultipartUploadsResult |
|IsTruncated|枚举字符串|表示本次返回的MultipartUpload结果列表是否被截断。取值范围如下：-   true：表示本次没有返回全部结果。
-   false（默认）：表示本次已经返回了全部结果。

父节点：ListMultipartUploadsResult |
|Upload|容器|保存Multipart Upload事件信息的容器。 子节点：Key, UploadId, Initiated

父节点：ListMultipartUploadsResult |
|Key|字符串|初始化Multipart Upload事件的Object名称。 父节点：Upload

**说明：**

OSS的返回结果按照Object名称字典序升序排列，对于同一个Object，则按照UploadId的字典序升序排列。 |
|UploadId|字符串|Multipart Upload事件的ID。 父节点：Upload |
|Initiated|日期|Multipart Upload事件初始化的时间。 父节点：Upload |

## 示例

请求示例

```
Get /?uploads  HTTP/1.1
Host:oss-example. oss-cn-hangzhou.aliyuncs.com
Date: Thu, 23 Feb 2012 06:14:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:JX75CtQqsmBBz+dcivn7kwBM****
```

返回示例

```
HTTP/1.1 200 
Server: AliyunOSS
Connection: keep-alive
Content-length: 1839
Content-type: application/xml
x-oss-request-id: 58a41847-3d93-1905-20db-ba6f561c****
Date: Thu, 23 Feb 2012 06:14:27 GMT

<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadsResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Bucket>oss-example</Bucket>
    <KeyMarker></KeyMarker>
    <UploadIdMarker></UploadIdMarker>
    <NextKeyMarker>oss.avi</NextKeyMarker>
    <NextUploadIdMarker>0004B99B8E707874FC2D692FA5D77D3F</NextUploadIdMarker>
    <Delimiter></Delimiter>
    <Prefix></Prefix>
    <MaxUploads>1000</MaxUploads>
    <IsTruncated>false</IsTruncated>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF518A1FE585B0C9360DC4C8</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>multipart.data</Key>
        <UploadId>0004B999EF5A239BB9138C6227D6****</UploadId>
        <Initiated>2012-02-23T04:18:23.000Z</Initiated>
    </Upload>
    <Upload>
        <Key>oss.avi</Key>
        <UploadId>0004B99B8E707874FC2D692FA5D7****</UploadId>
        <Initiated>2012-02-23T06:14:27.000Z</Initiated>
    </Upload>
</ListMultipartUploadsResult>
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/上传文件/分片上传.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/上传文件/分片上传.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/上传文件/分片上传.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/上传文件/分片上传.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/上传文件/分片上传.md)

