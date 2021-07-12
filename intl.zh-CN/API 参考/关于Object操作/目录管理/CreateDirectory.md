# CreateDirectory

CreateDirectory接口用于创建目录（Directory）。只有开启分层命名空间的Bucket支持调用此接口。

## 注意事项

-   要创建目录，您必须有PutObject权限。
-   创建目录时，如果已存在同名目录且用户对该目录具有访问权限，则OSS返回200 OK，但是不会执行创建目录的操作。
-   创建目录时，不支持传入数据，目录的Content-Length固定为0。
-   目录的Content-Type固定为application/x-directory，无法修改。
-   创建目录时，设置的目录绝对路径（DirectoryName）中不能出现连续的正斜线（/）。

## 请求语法

```
POST /objectName?x-oss-dir HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

|名称|类型|是否必选|描述|
|--|--|----|--|
|Authorization|字符串|否|表示请求本身已被授权。更多信息，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 通常情况下Authorization是必选请求头，但如果采用了URL包含签名，则不用携带该请求头。更多信息，请参见[在URL中包含签名](/intl.zh-CN/API 参考/访问控制/在URL中包含签名.md)。

默认值：无 |

此接口还需要包含Host、Date等公共请求头。关于公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅包含公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

请求示例

```
POST /desktop/oss?x-oss-dir HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Thu, 29 Apr 2021 05:21:12 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
```

返回示例

```
HTTP/1.1 200 OK
x-oss-request-id: 559CC9BDC755F95A64485981
Date: Thu, 29 Apr 2021 05:21:12 GMT
Last-Modified: Wed, 24 Feb 2021 06:07:48 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
```

## SDK

[Java SDK：创建目录](/intl.zh-CN/SDK 示例/Java/管理目录.md)

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|AccessDenied|403|返回此错误的可能原因如下：-   创建目录时，用户对设置的Bucket没有访问权限。
-   创建目录时，已存在同名目录但用户对该目录没有访问权限。 |
|FileAlreadyExists|409|创建目录时，如果当前目录层级已存在同名文件，则返回该错误。例如desktop目录下已存在名为osstest的文件，则在desktop目录下无法再创建名为osstest的目录。|

