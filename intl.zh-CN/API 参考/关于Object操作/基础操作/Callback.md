# Callback

您只需在发送给OSS的请求中携带相应的Callback参数即能实现回调。本文介绍Callback的实现原理。

**说明：**

-   目前支持Callback的API接口包括[PutObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PutObject.md)、[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)和[CompleteMultipartUpload](/intl.zh-CN/API 参考/关于Object操作/分片上传（MulitipartUpload）/CompleteMultipartUpload.md)。关于Callback的更多信息，请参见[原理介绍](/intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/服务端签名直传并设置上传回调.md)。
-   Callback目前不支持服务器名称指示SNI（Server Name Indication ）。

## 步骤1：构造参数

-   Callback参数

    Callback参数是由一段经过Base64编码的JSON字符串（字段）。构建callback参数的关键是指定请求回调的服务器 URL（callbackUrl）以及回调的内容（callbackBody）。

    JSON字段的详细信息请参见下表。

    |字段|描述|是否必选|
    |:-|:-|:---|
    |callbackUrl|    -   文件上传成功后，OSS向此URL发送回调请求，请求方法为POST，body为callbackBody指定的内容。正常情况下，该URL需要响应`HTTP/1.1 200 OK`，body必须为JSON格式，响应头Content-Length必须为合法的值，且大小不超过3 MB。
    -   支持同时配置最多5个URL，多个URL间以分号（;）分隔。OSS会依次发送请求直到第一个返回成功为止。
    -   如果未配置或者配置值为空则表示未配置callback。
    -   支持 HTTPS 地址。
    -   为了保证正确处理中文等情况，callbackUrl 需做 URL 编码处理，例如`http://example.com/中文.php?key=value&中文名称=中文值` 需要编码成 `http://example.com/%E4%B8%AD%E6%96%87.php?key=value&%E4%B8%AD%E6%96%87%E5%90%8D%E7%A7%B0=%E4%B8%AD%E6%96%87%E5%80%BC`
|是|
    |callbackHost|    -   发起回调请求时 Host 头的值，只有在设置了 callbackUrl 时才有效。
    -   如果没有配置 callbackHost，则会解析 callbackUrl 中的 URL 并将解析出的 host 填充到 callbackHost 中。
|否|
    |callbackBody|    -   发起回调时请求 body 的值，例如：key=$\(object\)&etag=$\(etag\)&my\_var=$\(x:my\_var\)。
    -   支持 OSS 系统变量、自定义变量和常量，支持的系统变量如下表所示 。自定义变量的支持方式在 PutObject 和CompleteMultipart 中是通过 callback-var 来传递，在 PostObject中则是将各个变量通过表单域来传递。
|是|
    |callbackBodyType|    -   发起回调请求的 Content-Type，支持 application/x-www-form-urlencoded 和application/json，默认为前者。
    -   callbackBodyType的取值为 application/x-www-form-urlencoded，则 callbackBody 中的变量将会被经过 URL 编码的值替换掉；如果为 application/json，则会按照 JSON 格式替换其中的变量。
|否|

    JSON 字段示例如下：

    ```
    {
    "callbackUrl":"121.101.166.30/test.php",
    "callbackHost":"oss-cn-hangzhou.aliyuncs.com",
    "callbackBody":"{\"mimeType\":${mimeType},\"size\":${size}}",
    "callbackBodyType":"application/json"
    }
    ```

    ```
    {
    "callbackUrl":"121.43.113.8:23456/index.html",
    "callbackBody":"bucket=${bucket}&object=${object}&etag=${etag}&size=${size}&mimeType=${mimeType}&imageInfo.height=${imageInfo.height}&imageInfo.width=${imageInfo.width}&imageInfo.format=${imageInfo.format}&my_var=${x:my_var}"
    }
    ```

    callbackBody 中可以设置的系统参数如下表所示：

    |系统参数|含义|
    |:---|:-|
    |bucket|存储空间|
    |object|对象（文件）|
    |etag|文件的 ETag，即返回给用户的 ETag 字段|
    |size|Object 大小，CompleteMultipartUpload 时为整个Object 的大小|
    |mimeType|资源类型，如 jpeg 图片的资源类型为 image/jpeg|
    |imageInfo.height|图片高度|
    |imageInfo.width|图片宽度|
    |imageInfo.format|图片格式，如 jpg、png 等|

    **说明：** imageInfo 针对图片格式，如果为非图片格式 ，imageInfo.height、imageInfo.width、imageInfo.format 都为空。

-   callback-var 自定义参数

    用户可以通过 callback-var 参数来配置自定义参数。自定义参数是一个 Key-Value 的 Map，用户可以配置自己需要的参数到该 Map。在 OSS 发起 POST 回调请求的时候，会将这些参数和上述的系统参数一起放在 POST 请求的 body 中以方便接收回调方获取。

    构造自定义参数的方法和 callBack 参数的方法是一样的，也是以 JSON 格式来传递。该 JSON 字符串就是一个包含所有自定义参数的 Key-Value 的 Map。

    **说明：** 用户自定义参数的 Key 一定要以 x: 开头且必须为小写，否则 OSS 会返回错误。

    假定用户需要设定两个自定义的参数分别为 x:var1 和 x:var2，对应的值分别为 value1 和 value2，那么构造出来的 JSON 格式如下：

    ```
    {
    "x:var1":"value1",
    "x:var2":"value2"
    }
    ```


**说明：** 如果传入的 callback 或者 callback-var 参数不合法，则会返回 400 错误，错误码为”InvalidArgument”，不合法的情况包括以下几类：

-   PutObject\(\) 和 CompleteMultipartUpload\(\) 接口中 URL 和 header 同时传入 callback\(x-oss-callback\) 或者 callback-var\(x-oss-callback-var\) 参数。
-   callback 或者 callback-var 参数过长，超过 5 KB \( PostObject\(\) 由于没有 callback-var 参数，因此没有此限制，下同）。
-   callback 或者 callback-var 参数没有经过 base64 编码或经过 base64 解码后不是合法的 JSON 格式。
-   callback 参数解析后 callbackUrl 字段包含的 URL 超过限制（5 个），或者 URL 中传入的 port 不合法，例如：

    ```
    {"callbackUrl":"10.101.166.30:test",
        "callbackBody":"test"}
    ```

-   callback 参数解析后 callbackBody 字段为空。
-   callback 参数解析后 callbackBodyType 字段的值不是 `application/x-www-form-urlencoded` 或者 `application/json`。
-   callback 参数解析后 callbackBody 字段中变量的格式不合法，合法的格式为 $\{var\}。
-   callback-var 参数解析后不是预期的 JSON格式，预期的格式应该为`{"x:var1":"value1","x:var2":"value2"...}`。

## 步骤2：构造回调请求

构造完成上述的 callback 和 callback-var 两个参数后，需将参数附加到 OSS 的请求中。

附加方式共有三种，方式如下：

-   在 URL 中携带参数。
-   在 Header 中携带参数。
-   在 POST 请求的 body 中使用表单域来携带参数。

    **说明：** 在使用 POST 请求上传 object 时只能使用这种方式来指定回调参数。


以上三种方式只能同时使用其中一种，否则 OSS 会返回 InvalidArgument 错误。

要将参数附加到 OSS 的请求中，首先要将上文构造的 JSON 字符串使用 base64 编码，然后按照如下的方法附加到 OSS 的请求中。

-   如果在 URL 中携带参数。把`callback=[CallBack]`或者`callback-var=[CallBackVar]`作为一个 URL 参数带入请求发送。计算签名 CanonicalizedResource 时 ，将 callback 或者 callback-var 当做一个 sub-resource 计算在内。
-   如果在 Header 中携带参数。把`x-oss-callback=[CallBack]`或者`x-oss-callback-var=[CallBackVar]`作为一个 Header 带入请求发送。在计算签名CanonicalizedOSSHeaders 时，将 x-oss-callback-var 和 x-oss-callback 计算在内。示例如下：

    ```
    PUT /test.txt HTTP/1.1
    Host: callback-test.oss-test.aliyun-inc.com
    Accept-Encoding: identity
    Content-Length: 5
    x-oss-callback-var: eyJ4Om15X3ZhciI6ImZvci1jYWxsYmFjay10ZXN0In0=
    User-Agent: aliyun-sdk-python/0.4.0 (Linux/2.6.32-220.23.2.ali1089.el5.x86_64/x86_64;2.5.4)
    x-oss-callback: eyJjYWxsYmFja1VybCI6IjEyMS40My4xMTMuODoyMzQ1Ni9pbmRleC5odG1sIiwgICJjYWxsYmFja0JvZHkiOiJidWNrZXQ9JHtidWNrZXR9Jm9iamVjdD0ke29iamVjdH0mZXRhZz0ke2V0YWd9JnNpemU9JHtzaXplfSZtaW1lVHlwZT0ke21pbWVUeXBlfSZpbWFnZUluZm8uaGVpZ2h0PSR7aW1hZ2VJbmZvLmhlaWdodH0maW1hZ2VJbmZvLndpZHRoPSR7aW1hZ2VJbmZvLndpZHRofSZpbWFnZUluZm8uZm9ybWF0PSR7aW1hZ2VJbmZvLmZvcm1hdH0mbXlfdmFyPSR7eDpteV92YXJ9In0=
    Host: callback-test.oss-test.aliyun-inc.com
    Expect: 100-Continue
    Date: Mon, 14 Sep 2015 12:37:27 GMT
    Content-Type: text/plain
    Authorization: OSS mlepou3zr4u7b14:5a74vhd4UXpmyuudV14Kaen5****
    Test
    ```

-   在 POST 请求的 body 中使用表单域来携带参数。
    -   如果需要在 POST 上传 Object 时附带回调参数会稍微复杂一点，callback 参数要使用独立的表单域来附加，示例如下：

        ```
        --9431149156168
        Content-Disposition: form-data; name="callback"
        eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkmdGFibGU9JHt4OnRhYmxlfSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==
        ```

    -   如果是自定义参数，不能直接将 callback-var 参数直接附加到表单域中，每个自定义的参数都需要使用独立的表单域来附加。如果用户的自定义参数 JSON 字段示例如下：

        ```
        {
        "x:var1":"value1",
        "x:var2":"value2"
        }
        ```

        那么 POST 请求的表单域为：

        ```
        --9431149156168
        Content-Disposition: form-data; name="callback"
        eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkmdGFibGU9JHt4OnRhYmxlfSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==
        --9431149156168
        Content-Disposition: form-data; name="x:var1"
        value1
        --9431149156168
        Content-Disposition: form-data; name="x:var2"
        value2
        ```

        同时可以在 policy 中添加 callback 条件（如果不添加 callback，则不对该参数做上传验证）如：

        ```
        { "expiration": "2014-12-01T12:00:00.000Z",
          "conditions": [
            {"bucket": "johnsmith" },
            {"callback": "eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkiLCJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24veC13d3ctZm9ybS11cmxlbmNvZGVkIn0="},
            ["starts-with", "$key", "user/eric/"],
          ]
        }
        ```


## 步骤3：发起回调请求

如果文件上传成功，OSS 会根据用户请求中的 callback 参数和 callback-var 自定义参数，将特定内容以 POST 方式发送给应用服务器。

```
POST /index.html HTTP/1.0
Host: 121.43.113.8
Connection: close
Content-Length: 181
Content-Type: application/x-www-form-urlencoded
User-Agent: http-client/0.0.1
bucket=callback-test&object=test.txt&etag=D8E8FCA2DC0F896FD7CB4CB0031BA249&size=5&mimeType=text%2Fplain&imageInfo.height=&imageInfo.width=&imageInfo.format=&x:var1=for-callback-test
```

## （可选）步骤4：回调签名

用户设置 callback 参数后，OSS 将按照用户设置的 callbackUrl 发送 POST 回调请求给用户的应用服务器。应用服务器收到回调请求之后，如果希望验证回调请求确实是由 OSS 发起的话，可以通过在回调中带上签名来验证 OSS 的身份。

-   生成签名

    签名发生在 OSS 端，采用 RSA 非对称方式。

    私钥加密生成签名的过程为：

    ```
    authorization = base64_encode(rsa_sign(private_key, url_decode(path) + query_string + ‘\n’ + body, md5))
    ```

    **说明：** 其中 private\_key 为私钥，只有 OSS 知晓，path 为回调请求的资源路径，query\_string 为查询字符串，body 为回调的消息体。

    签名过程分为以下三步：

    1.  获取待签名字符串：资源路径经过 URL解码后，加上原始的查询字符串、加上一个回车符、然后加上回调消息体。
    2.  RSA签名：使用密钥对待签名字符串进行签名，签名的 hash 函数为 md5。
    3.  将签名后的结果做 base64 编码，得到最终的签名，签名放在回调请求的 authorization 头中。
    示例如下：

    ```
    POST /index.php?id=1&index=2 HTTP/1.0
    Host: 121.43.113.8
    Connection: close
    Content-Length: 18
    authorization: kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2t****
    Content-Type: application/x-www-form-urlencoded
    User-Agent: http-client/0.0.1
    x-oss-pub-key-url: aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==
    bucket=yonghu-test
    ```

    path 为`/index.php`，query\_string 为`?id=1&index=2`，body 为`bucket=yonghu-test`，最终签名结果为`kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==`。

-   验证签名

    验证签名的过程即为签名的逆过程，由应用服务器验证，过程如下：

    ```
    Result = rsa_verify(public_key, md5(url_decode(path) + query_string + ‘\n’ + body), base64_decode(authorization))
    ```

    字段的含义与签名过程中描述相同，其中 public\_key 为公钥， authorization 为回调头中的签名，整个验证签名的过程分为以下几步：

    1.  回调请求的 x-oss-pub-key-url 头保存的是公钥 URL 地址的 base64 编码，因此需要对其做 base64 解码后获取到公钥，即

        ```
        public_key = urlopen(base64_decode(x-oss-pub-key-url头的值))
        ```

        **说明：** 了保证这个 publickey 是由 OSS 颁发的，用户需要校验`x-oss-pub-key-url`头的值必须以`http://gosspublic.alicdn.com/`或者`https://gosspublic.alicdn.com/`开头。

    2.  获取 base64 解码后的签名

        ```
        signature = base64_decode(authorization头的值)
        ```

    3.  获取待签名字符串，方法与签名一致

        ```
        sign_str = url_decode(path) + query_string + ‘\n’ + body
        ```

    4.  验证签名

        ```
        result = rsa_verify(public_key, md5(sign_str), signature)
        ```

    以上述示例来说明验证签名的完整过程：

    1.  获取到公钥的 URL 地址，即`aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==`经过base64 解码后得到`http://gosspublic.alicdn.com/callback_pub_key_v1.pem`。
    2.  签名头`kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==`做 base64 解码（由于为非打印字符，无法显示出解码后的结果）。
    3.  获取待签名字符串，即 url\_decode\(“index.php”\) + “?id=1&index=2” + “\\n” + “bucket=yonghu-test”，并做MD5校验。
    4.  验证签名。
-   应用服务器示例

    以下为一段 Python 示例，演示了一个简单的应用服务器，主要是说明验证签名的方法，此示例需要安装 M2Crypto库。

    ```
    import httplib
    import base64
    import md5
    import urllib2
    from BaseHTTPServer import BaseHTTPRequestHandler, HTTPServer
    from M2Crypto import RSA
    from M2Crypto import BIO
    def get_local_ip():
        try:
            csock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
            csock.connect(('8.8.8.8', 80))
            (addr, port) = csock.getsockname()
            csock.close()
            return addr
        except socket.error:
            return ""
    class MyHTTPRequestHandler(BaseHTTPRequestHandler):
        '''
        def log_message(self, format, *args):
            return
        '''
        def do_POST(self):
            #get public key
            pub_key_url = ''
            try:
                pub_key_url_base64 = self.headers['x-oss-pub-key-url']
                pub_key_url = pub_key_url_base64.decode('base64')
                if not pub_key_url.startswith("http://gosspublic.alicdn.com/") and not pub_key_url.startswith("https://gosspublic.alicdn.com/"):
                    self.send_response(400)
                    self.end_headers()
                    return
                url_reader = urllib2.urlopen(pub_key_url)
                #you can cache it
                pub_key = url_reader.read() 
            except:
                print 'pub_key_url : ' + pub_key_url
                print 'Get pub key failed!'
                self.send_response(400)
                self.end_headers()
                return
            #get authorization
            authorization_base64 = self.headers['authorization']
            authorization = authorization_base64.decode('base64')
            #get callback body
            content_length = self.headers['content-length']
            callback_body = self.rfile.read(int(content_length))
            #compose authorization string
            auth_str = ''
            pos = self.path.find('?')
            if -1 == pos:
                auth_str = urllib2.unquote(self.path) + '\n' + callback_body
            else:
                auth_str = urllib2.unquote(self.path[0:pos]) + self.path[pos:] + '\n' + callback_body
            print auth_str
            #verify authorization
            auth_md5 = md5.new(auth_str).digest()
            bio = BIO.MemoryBuffer(pub_key)
            rsa_pub = RSA.load_pub_key_bio(bio)
            try:
                result = rsa_pub.verify(auth_md5, authorization, 'md5')
            except:
                result = False
            if not result:
                print 'Authorization verify failed!'
                print 'Public key : %s' % (pub_key)
                print 'Auth string : %s' % (auth_str)
                self.send_response(400)
                self.end_headers()
                return
            #do something according to callback_body
            #response to OSS
            resp_body = '{"Status":"OK"}'
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.send_header('Content-Length', str(len(resp_body)))
            self.end_headers()
            self.wfile.write(resp_body)
    class MyHTTPServer(HTTPServer):
        def __init__(self, host, port):
            HTTPServer.__init__(self, (host, port), MyHTTPRequestHandler)
    if '__main__' == __name__:
        server_ip = get_local_ip()
    server_port = 23451
    server = MyHTTPServer(server_ip, server_port)
    server.serve_forever()
    ```

    其它语言的服务端代码请参见下表。

    |SDK语言|描述|
    |-----|--|
    |Java|    -   下载地址：[Java](https://gosspublic.alicdn.com/images/AppCallbackServer.zip)
    -   运行方法：解压包运行`java -jar oss-callback-server-demo.jar 9000`（9000指运行的端口，可以自行指定）。 |
    |Python|    -   下载地址：[Python](https://gosspublic.alicdn.com/images/callback_app_server.py.zip)
    -   运行方法：解压包直接运行`python callback_app_server.py`，运行该程序需要安装 RSA 的依赖。 |
    |Go|    -   下载地址：[Go](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048745465/callback-server-go.zip)
    -   运行方法：解压后参看 `README.md`。 |
    |PHP|    -   下载地址：[PHP](https://gosspublic.alicdn.com/callback-php-demo.zip)
    -   运行方法：部署到 Apache 环境下，因为 PHP 本身语言的特点，取一些数据头部会依赖于环境。所以可以参考例子根据所在环境修改。 |
    |.NET|    -   下载地址：[.NET](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/131396/intl_en/1597211267799/callback-server-dotnet-20200810.zip)
    -   运行方法：解压后参看 `README.md`。 |
    |Node.js|    -   下载地址：[Node.js](http://gosspublic.alicdn.com/doc/oss-doc/callback-nodejs-demo.zip)
    -   运行方法：解压包直接运行`node example.js`。 |
    |Ruby|    -   下载地址：[Ruby](https://github.com/rockuw/oss-callback-server)
    -   运行方法： ruby aliyun\_oss\_callback\_server.rb |


## 步骤5：返回回调结果

应用服务器返回响应给OSS。

返回的回调请求为：

```
HTTP/1.0 200 OK
Server: BaseHTTP/0.3 Python/2.7.6
Date: Mon, 14 Sep 2015 12:37:27 GMT
Content-Type: application/json
Content-Length: 9
{"a":"b"}
```

**说明：** 应用服务器返回 OSS 的响应必须带有 Content-Length 的 Header，Body 大小不能超过1 MB。

## 步骤6：返回上传结果

OSS将应用服务器返回的内容返回给用户。

返回的内容响应为：

```
HTTP/1.1 200 OK
Date: Mon, 14 Sep 2015 12:37:27 GMT
Content-Type: application/json
Content-Length: 9
Connection: keep-alive
ETag: "D8E8FCA2DC0F896FD7CB4CB0031BA249"
Server: AliyunOSS
x-oss-bucket-version: 1442231779
x-oss-request-id: 55F6BF87207FB30F2640C548
{"a":"b"}
```

**说明：**

-   如果类似 CompleteMultipartUpload 这样的请求，在返回请求本身 body 中存在内容（如 XML 格式的信息），使用上传回调功能后会覆盖原有的 body 中的内容如`{"a":"b"}`，希望对此处做好判断处理。
-   如果回调失败，则返回 203，错误码为 ”CallbackFailed”。文件已经成功上传到了 OSS，但回调失败。回调失败只是表示 OSS 没有收到预期的回调响应，不代表应用服务器没有收到回调请求（例如应用服务器返回的内容不是 JSON 格式）。

