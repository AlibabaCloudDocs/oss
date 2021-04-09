# Rename

Rename接口用于重命名目录（Directory）或者文件（Object）。只有开启分层命名空间的Bucket支持调用此接口。

## 注意事项

将源目录或者源文件重命名为目标目录或者目标文件时，有如下注意事项：

-   您必须有源目录或者源文件的DeleteObject权限以及目标目录或者目标文件的PutObject权限。
-   源目录或者源文件以及目标目录或者目标文件的父级目录必须存在。
-   目标目录或者目标文件的父级目录中不能存在同名的目录和文件。

## 请求语法

```
POST /dstObjectName?x-oss-rename HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
x-oss-rename-source:srcPathName
```

## 请求头

|名称|类型|是否必选|描述|
|--|--|----|--|
|x-oss-rename-source|字符串|是|源目录或者源文件的绝对路径，例如desktop/oss/a。该路径必须存在。|

此接口还需包含Host、Date等公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅包含公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

以下示例用于将desktop目录下oss目录中的a重命名为b。

```
POST /desktop/oss/b?x-oss-rename HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 29 Apr 2021 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
x-oss-rename-source: desktop/oss/a
```

返回示例

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Thu, 29 Apr 2021 05:21:12 GMT
Connection: keep-alive
Server: AliyunOSS
```

## SDK

[Java SDK：重命名]()

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|AccessDenied|403|返回该错误的可能原因如下：-   用户对设置的Bucket没有访问权限。
-   用户对目录或文件没有访问权限。 |
|NoSuchKey|404|返回该错误的可能原因如下：-   源目录或者源文件不存在。
-   目标目录或者目标文件的父级目录不存在。 |
|FileAlreadyExists|409|目标目录或者目标文件已存在。|

