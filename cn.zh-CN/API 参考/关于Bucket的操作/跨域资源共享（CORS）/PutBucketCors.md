# PutBucketCors

PutBucketCors接口用于为指定的存储空间（Bucket）设置跨域资源共享CORS（Cross-Origin Resource Sharing）规则。

## 注意事项

使用跨域资源共享时，有如下注意事项：

-   默认不开启CORS

    Bucket默认不开启CORS功能，所有跨域请求的Origin都不被允许。

-   覆盖语义

    PutBucketCors为覆盖语义，即新配置的CORS规则将覆盖已有的CORS规则。

-   应用程序中使用CORS

    在应用程序中使用CORS功能时，需通过PutBucketCors接口手动上传CORS规则来开启CORS功能。

    例如，从`www.a.com`通过浏览器的`XMLHttpRequest`功能来访问OSS，需要通过本接口手动上传CORS规则，且CORS规则需由XML文档进行描述。

-   CORS规则匹配

    当OSS收到一个跨域请求或OPTIONS请求，会先读取Bucket对应的CORS规则，然后进行相应的权限检查。OSS会依次检查每一条规则，使用第一条匹配的规则来允许请求并返回对应的Header。如果所有规则都匹配失败，则不附加任何CORS相关的Header。

    CORS规则匹配成功必须满足以下三个条件：

    -   请求的Origin必须匹配一个`AllowedOrigin`项。
    -   请求的方法（如GET、PUT等）或者OPTIONS请求的`Access-Control-Request-Method`头对应的方法必须匹配一个`AllowedMethod`项。
    -   OPTIONS请求的`Access-Control-Request-Headers`头包含的每个header都必须匹配一个`AllowedHeader`项。

## 请求语法

```
PUT /?cors HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration>
    <CORSRule>
      <AllowedOrigin>the origin you want allow CORS request from</AllowedOrigin>
      <AllowedOrigin>…</AllowedOrigin>
      <AllowedMethod>HTTP method</AllowedMethod>
      <AllowedMethod>…</AllowedMethod>
        <AllowedHeader> headers that allowed browser to send</AllowedHeader>
          <AllowedHeader>…</AllowedHeader>
          <ExposeHeader> headers in response that can access from client app</ExposeHeader>
          <ExposeHeader>…</ExposeHeader>
          <MaxAgeSeconds>time to cache pre-fight response</MaxAgeSeconds>
    </CORSRule>
    <CORSRule>
      …
    </CORSRule>
…
</CORSConfiguration >
```

## 请求元素

|名称|类型|是否必选|示例值|描述|
|:-|:-|:---|---|:-|
|CORSRule|容器|是|不涉及|CORS规则的容器。每个Bucket最多允许10条CORS规则。上传的XML文档最大允许16 KB。

父节点：CORSConfiguration |
|AllowedOrigin|字符串|是|\*|指定允许的跨域请求来源。 -   OSS允许使用多个元素来指定多个允许的来源。
-   OSS仅允许使用一个星号（\*）通配符。如果指定为星号（\*），则表示允许所有来源的跨域请求。

父节点：CORSRule |
|AllowedMethod|枚举值|是|GET|指定允许的跨域请求方法。取值：GET、PUT、DELETE、POST、HEAD

父节点：CORSRule |
|AllowedHeader|字符串|否|Authorization|控制OPTIONS预取指令`Access-Control-Request-Headers`中指定的Header是否被允许。在`Access-Control-Request-Headers`中指定的每个Header都必须在AllowedHeader中有对应的项。

**说明：** 仅允许使用一个星号（\*）通配符 。

父节点：CORSRule |
|ExposeHeader|字符串|否|x-oss-test|指定允许用户从应用程序中访问的响应头。例如，一个Javascript的XMLHttpRequest对象。**说明：** 不允许使用星号（\*）通配符。

父节点：CORSRule |
|MaxAgeSeconds|整型|否|100|指定浏览器对特定资源的预取（OPTIONS）请求返回结果的缓存时间。单位为秒。 单条CORS规则仅允许一个MaxAgeSeconds。

父节点：CORSRule |
|CORSConfiguration|容器|是|不涉及|Bucket的CORS规则容器。父节点：无 |
|ResponseVary|布尔|否|false|是否返回Vary: Origin头。 取值：

-   true：不管发送的是否是跨域请求或跨域请求是否成功，均会返回Vary: Origin头。
-   false（默认值）：任何情况下均不返回Vary: Origin头

**说明：** 此字段不能单独配置，必须至少配置一项跨域规则才能生效。 |

此接口涉及的Host、Authorization等其他更多公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口仅返回公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
    PUT /?cors HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 186
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT*****
    <?xml version="1.0" encoding="UTF-8"?>
    <CORSConfiguration>
        <CORSRule>
          <AllowedOrigin>*</AllowedOrigin>
          <AllowedMethod>PUT</AllowedMethod>
          <AllowedMethod>GET</AllowedMethod>
          <AllowedHeader>Authorization</AllowedHeader>
        </CORSRule>
        <CORSRule>
          <AllowedOrigin>http://www.a.com</AllowedOrigin>
          <AllowedOrigin>http://www.b.com</AllowedOrigin>
          <AllowedMethod>GET</AllowedMethod>
          <AllowedHeader> Authorization</AllowedHeader>
          <ExposeHeader>x-oss-test</ExposeHeader>
          <ExposeHeader>x-oss-test1</ExposeHeader>
          <MaxAgeSeconds>100</MaxAgeSeconds>
        </CORSRule>
        <ResponseVary>false</ResponseVary>
    </CORSConfiguration >
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 50519080C4689A033D0*****
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
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
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致则返回此错误码。|

