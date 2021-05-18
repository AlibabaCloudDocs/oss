# OptionObject

浏览器在发送跨域请求之前会发送一个preflight请求（OPTIONS）给OSS，并带上特定的来源域、HTTP方法和header等信息，以决定是否发送真正的请求。

## 请求语法

```
OPTIONS /ObjectName HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Origin:Origin
Access-Control-Request-Method:HTTP method
Access-Control-Request-Headers:Request Headers
```

## 请求头

|名称|类型|是否必选|示例值|描述|
|:-|:-|----|---|:-|
|Origin|字符串|是|http://www.example.com|请求来源域，用于标识跨域请求。在实际请求中只能设置一个该请求头。

默认值：无 |
|Access-Control-Request-Method|字符串|是|PUT|在实际请求中会用到的方法。在实际请求中只能设置一个该请求头。

默认值：无 |
|Access-Control-Request-Headers|字符串|否|x-oss-test1,x-oss-test2|在实际请求中会用到的除了简单头部之外的header。在实际请求中可以为该请求头设置多个header，多个header之间使用英文逗号\(,\)隔开。

默认值：无 |

## 响应头

|名称|类型|示例值|描述|
|:-|:-|---|:-|
|Access-Control-Allow-Origin|字符串|http://www.example.com|请求中包含的Origin。如果不允许该请求，则响应头不包含该头部。|
|Access-Control-Allow-Methods|字符串|PUT|允许请求的HTTP方法。如果不允许该请求，则响应头不包含该头部。|
|Access-Control-Allow-Headers|字符串|x-oss-test,x-oss-test1|允许请求携带的header的列表。如果请求中有不被允许的header，则不包含该头部，请求也将被拒绝。|
|Access-Control-Expose-Headers|字符串|x-oss-test1,x-oss-test2|允许在客户端JavaScript程序中访问的headers的列表。|
|Access-Control-Max-Age|整型|60|允许浏览器缓存preflight结果的时间，单位为秒。|

## 示例

请求示例

```
OPTIONS /testobject HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com  
Date: Fri, 24 Feb 2012 05:45:34 GMT  
Origin:http://www.example.com
Access-Control-Request-Method:PUT
Access-Control-Request-Headers:x-oss-test1,x-oss-test2
```

返回示例

```
HTTP/1.1 200 OK 
x-oss-request-id: 5051845BC4689A033D00****
Date: Fri, 24 Feb 2012 05:45:34 GMT
Access-Control-Allow-Origin: http://www.example.com
Access-Control-Allow-Methods: PUT
Access-Control-Expose-Headers: x-oss-test1,x-oss-test2
Connection: keep-alive
Content-Length: 0  
Server: AliyunOSS
```

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|AccessForbidden|403|OSS可以通过PutBucketCORS接口来开启Bucket的CORS功能。开启CORS功能后，OSS在收到浏览器preflight请求时会根据设定的规则评估是否允许本次请求，如果不允许或者CORS功能未开启，则返回此错误码。|

