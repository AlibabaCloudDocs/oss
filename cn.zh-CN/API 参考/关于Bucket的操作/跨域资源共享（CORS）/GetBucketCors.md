# GetBucketCors

GetBucketCors接口用于获取指定存储空间（Bucket）当前的跨域资源共享CORS（Cross-Origin Resource Sharing）规则。

## 请求语法

```
GET /?cors HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

此接口仅使用公共请求头部，详情请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|CORSRule|容器|CORS规则的容器。每个Bucket最多允许10条规则。

父节点：CORSConfiguration |
|AllowedOrigin|字符串|允许的跨域请求来源。 星号（\*）表示允许所有的来源的跨域请求。

父节点：CORSRule |
|AllowedMethod|枚举（GET,PUT,DELETE,POST,HEAD）|允许的跨域请求方法。

父节点：CORSRule |
|AllowedHeader|字符串|控制在OPTIONS预取指令中Access-Control-Request-Headers指定的header是否允许。在Access-Control-Request-Headers中指定的每个header都在AllowedHeader中有一条对应的项。

父节点：CORSRule |
|ExposeHeader|字符串|允许用户从应用程序中访问的响应头（例如一个JavaScript的XMLHttpRequest对象）。

父节点：CORSRule |
|MaxAgeSeconds|整型|浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间。 一个CORS规则里面最多允许出现一个MaxAgeSeconds。

单位：秒

父节点：CORSRule |
|CORSConfiguration|容器|Bucket的CORS规则容器。

父节点：无 |

## 示例

请求示例

```
Get /?cors HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc: BuG4rRK+zNhH1AcF51NNHD39****
```

返回示例

```
HTTP/1.1 200
x-oss-request-id: 50519080C4689A033D00****
Date: Thu, 13 Sep 2012 07:51:28 GMT
Connection: keep-alive
Content-Length: 218  
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>*</AllowedOrigin>
      <AllowedMethod>GET</AllowedMethod>
      <AllowedHeader>*</AllowedHeader>
      <ExposeHeader>x-oss-test</ExposeHeader>
      <MaxAgeSeconds>100</MaxAgeSeconds>
    </CORSRule>
</CORSConfiguration>
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/跨域资源共享.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/跨域资源共享.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/跨域资源共享.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/跨域资源共享.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/存储空间/跨域资源共享.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/跨域资源共享.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/跨域资源共享.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/跨域资源共享.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/跨域资源共享.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|NoSuchCORSConfiguration|404|目标CORS规则不存在。|
|AccessDenied|403|只有Bucket的拥有者才能获取CORS规则。|

