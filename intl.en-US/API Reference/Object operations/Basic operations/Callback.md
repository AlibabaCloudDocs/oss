# Callback

You can call this operation to implement callback by sending a request that contains callback parameters to OSS. This topic describes how to implement upload callback.

**Note:**

-   The following API operations support callback: [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md), [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md), and [CompleteMultipartUpload](/intl.en-US/API Reference/Object operations/Multipart upload/CompleteMultipartUpload.md). For more information about callback, see [Add signatures on the server, configure upload callback, and directly transfer data](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server, configure upload callback, and directly transfer data.md).
-   Callback does not support server name indication \(SNI\).

## Step 1: Construct parameters

-   Callback parameter

    A callback parameter is a Base64-encoded string \(field\) in the JSON format. To create a callback parameter, you must specify the URL \(callbackUrl\) of the server to which the callback request is sent and the content \(callbackBody\) of the callback information.

    The following table describes the fields in the JSON format.

    |Field|Description|Required|
    |:----|:----------|:-------|
    |callbackUrl|    -   The URL of the server to which OSS sends a callback request. After an object is uploaded, OSS uses POST to send a callback request to the URL. The body of the request is the content specified in callbackBody. In most cases, the server configured with the URL returns the `HTTP/1.1 200 OK` response. The response body must be in the JSON format, and the Content-Length response header value must be valid and smaller than 3 MB.
    -   You can configure up to five URLs that are separated by semicolons \(;\) in a request. OSS sends requests to each URL until the first success response is returned.
    -   If the value of this field is not configured or is empty, callback is not configured.
    -   HTTPS-based URLs are supported.
    -   |Yes|
    |callbackHost|    -   The value of the Host header in the callback request. This field is valid only when callbackUrl is specified.
    -   If callbackHost is not specified, the domain names of hosts are resolved from the URLs specified by the callbackUrl field and are specified as the value of callbackHost.
|No|
    |callbackBody|    -   The value of the callback request body. Example: key=$\(object\)&etag=$\(etag\)&my\_var=$\(x:my\_var\).
    -   System variables, custom variables, and constants are supported for this field. The following table lists the supported system variables. Custom variables are passed by using callback-var in the PutObject and CompleteMultipart operations and by using form fields in the PostObject operation.
|Yes|
    |callbackBodyType|    -   The Content-Type header in the callback request. Valid values: application/x-www-form-urlencoded and application/json. Default value: application/x-www-form-urlencoded.
    -   If the value of callbackBodyType is application/x-www-form-urlencoded, variables in callbackBody are replaced by the URL-encoded values. If the value of callbackBodyType is application/json, the variables are replaced by the values in the JSON format.
|No|

    Examples of the JSON fields in a callback parameter:

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

    The following table describes the system parameters you can configure for callbackBody.

    |System parameter|Description|
    |:---------------|:----------|
    |bucket|The bucket that contains the requested object.|
    |object|The name of the requested object.|
    |etag|The ETag field configured for the object and returned to the requester.|
    |size|The size of the requested object, which is the size of the entire object in a CompleteMultipartUpload operation.|
    |mimeType|The resource type. For example, the resource type of JPEG images is image/jpeg.|
    |imageInfo.height|The height of the image.|
    |imageInfo.width|The width of the image.|
    |imageInfo.format|The format of the image. Example: JPG or PNG.|

    **Note:** The imageInfo parameter can be configured only for an image object. If the object is not an image, imageInfo.height, imageInfo.width, and imageInfo.format are empty.

-   Custom parameters of callback-var

    You can configure custom parameters by using the callback-var parameter. Custom parameters are key-value pairs in a map. You can add required parameters to the map. When a callback request is initiated by using the POST method, OSS adds the custom parameters and the system parameters described in the preceding section to the body of the POST request. This way, the parameters can be easily obtained by the requester.

    You can construct a custom parameter the way you construct a callback parameter. The parameters are passed in the JSON format. Each parameter has a key-value pair, which is a map that consists of key-value pairs of all custom parameters.

    **Note:** The key of a custom parameter must start with "x:" and must contain only lowercase letters. Otherwise, OSS returns an error.

    For example, if you want to configure two custom parameters x:var1 and x:var2. The value of x:var1 is value1. The value of x:var2 is value2. The following JSON string is constructed:

    ```
    {
    "x:var1":"value1",
    "x:var2":"value2"
    }
    ```


**Note:** If the imported callback parameter or callback-var parameters are invalid, HTTP status code 400 and the InvalidArgument error code are returned. This error occurs in the following scenarios:

-   URLs and headers are passed in at the same time to the callback parameter \(x-oss-callback\) or the callback-var parameter \(x-oss-callback-var\) in PutObject\(\) and CompleteMultipartUpload\(\) operations.
-   The size of the callback or callback-var parameter exceeds 5 KB. This does not occur in PostObject operations because the callback-var parameter is unavailable in PostObject operations.
-   The callback or callback-var parameter is not Base64-encoded or is not in the valid JSON format after the parameter is decoded.
-   The callbackUrl field value decoded from the callback parameter includes more than five URLs, or the port number in the URL is invalid. Example:

    ```
    {"callbackUrl":"10.101.166.30:test",
        "callbackBody":"test"}
    ```

-   The callbackBody field decoded from the callback parameter is empty.
-   The value of the callbackBodyType field decoded from the callback parameter is not `application/x-www-form-urlencoded` or `application/json`.
-   The variables in the callbackBody field decoded from the callback parameter are not in the valid format. The valid format is $ \{var\}.
-   The variables in the callbackBody field decoded from the callback-var parameter are not in the expected JSON format of `{"x:var1":"value1","x:var2":"value2"...}`.

## Step 2: Construct a callback request

After you construct the callback and callback-var parameters, you must add the parameters to the callback request sent to OSS.

You can use the following methods to add parameters:

-   Include the parameters in the URL.
-   Include the parameters in the header.
-   Include the parameters in the form fields in the body of a POST request.

    **Note:** You can use only this method to specify callback parameters when you upload objects by using POST requests.


You can use only one of the three methods to add parameters. If you use more than one method, OSS returns the InvalidArgument error code.

To include the parameters in a request sent to OSS, you must use Base64 to encode the JSON string constructed in the preceding section. Then, use the following methods to include the parameters:

-   To include the parameters in the URL, include `callback=[CallBack]` or `callback-var=[CallBackVar]` as a URL parameter in the request. When the CanonicalizedResource field in the signature is calculated, callback or callback-var is used as a subresource.
-   To include the parameters to the header, include `x-oss-callback=[CallBack]` or `x-oss-callback-var=[CallBackVar]` as a header in the request. When the system calculates the CanonicalizedOSSHeaders field, include x-oss-callback-var and x-oss-callback. Examples:

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

-   Include the parameters in the form fields in the body of a POST request.
    -   If the callback parameters are included when the POST method is used to upload an object, the procedure becomes more complex. The callback parameters must be included by using a separate form field. Example:

        ```
        --9431149156168
        Content-Disposition: form-data; name="callback"
        eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkmdGFibGU9JHt4OnRhYmxlfSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==
        ```

    -   Each custom parameter uses a separate form field. You cannot directly include the callback-var parameters in existing form fields. Examples of JSON fields for customer parameters:

        ```
        {
        "x:var1":"value1",
        "x:var2":"value2"
        }
        ```

        Form fields in the POST request:

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

        You can also add callback conditions to the policy. If the callback parameters are not added, upload verification is not performed on the policy. Example:

        ```
        { "expiration": "2014-12-01T12:00:00.000Z",
          "conditions": [
            {"bucket": "johnsmith" },
            {"callback": "eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkiLCJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24veC13d3ctZm9ybS11cmxlbmNvZGVkIn0="},
            ["starts-with", "$key", "user/eric/"],
          ]
        }
        ```


## Step 3: Initiate a callback request

If an object is uploaded, OSS uses POST to send the content specified by the callback and callback-var parameters in the request to the application server. Example:

```
POST /index.html HTTP/1.0
Host: 121.43.113.8
Connection: close
Content-Length: 181
Content-Type: application/x-www-form-urlencoded
User-Agent: http-client/0.0.1
bucket=callback-test&object=test.txt&etag=D8E8FCA2DC0F896FD7CB4CB0031BA249&size=5&mimeType=text%2Fplain&imageInfo.height=&imageInfo.width=&imageInfo.format=&x:var1=for-callback-test
```

## Step 4: \(Optional\) Sign the callback request

If the callback parameters are configured in the request, OSS uses POST to send a callback request to the application server based on the specified callback URL. To verify whether the callback request received by the application server is initiated by OSS, you can sign the callback request.

-   Generate a signature

    A callback request is signed by OSS by using the Rivest–Shamir–Adleman \(RSA\) asymmetric algorithm.

    To generate a signature, encrypt the callback string by using a private key. Example:

    ```
    authorization = base64_encode(rsa_sign(private_key, url_decode(path) + query_string + '\n' + body, md5))
    ```

    **Note:** In the preceding code, private\_key specifies a private key known only by OSS. path specifies the resource path included in the callback request. query\_string specifies the query string. body specifies the message body specified in the callback request.

    To sign a callback request, perform the following steps:

    1.  Obtain the callback string-to-sign. The callback string consists of the resource path that is obtained by decoding the URL-encoded string, the original query string, a carriage return, and the callback message body.
    2.  Sign the callback string by using the RSA encryption algorithm. Use the private key to encrypt the signature string. The hash function used for the signature is MD5.
    3.  Use Base64 to encode the signed result to obtain the final signature and add the signature to the Authorization header in the callback request.
    Examples:

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

    In the preceding code, path is set to `/index.php`. query\_string is set to `?id=1&index=2`. body is set to `bucket=yonghu-test`. The final signature is `kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==`.

-   Verify the signature

    Signature verification is an inverse process of signing a request. The signature is verified by the application server. Process:

    ```
    Result = rsa_verify(public_key, md5(url_decode(path) + query_string + '\n' + body), base64_decode(authorization))
    ```

    The descriptions of some fields in the preceding code are the same as those of the fields that are used to sign the request. public\_key specifies the public key. authorization specifies the signature in the callback request header. To verify the signature, perform the following steps:

    1.  Decode the Base64-encoded URL to obtain the public key. The x-oss-pub-key-url header in the callback request stores the Base64-encoded URL of the public key.

        ```
        public_key = urlopen(base64_decode(x-oss-pub-key-url header value))
        ```

        **Note:** To ensure that the public key is issued by OSS, you must verify whether the `x-oss-pub-key-url` header value starts with `http://gosspublic.alicdn.com/` or `https://gosspublic.alicdn.com/`.

    2.  Obtain the signature decoded in Base64.

        ```
        signature = base64_decode(authorization header value)
        ```

    3.  Obtain the string-to-sign by using the same procedure that is used to obtain the string when the callback request is signed.

        ```
        sign_str = url_decode(path) + query_string + '\n' + body
        ```

    4.  Verify the signature.

        ```
        result = rsa_verify(public_key, md5(sign_str), signature)
        ```

    Complete process of signature verification:

    1.  To obtain the URL of the public key, decode `aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==` in Base64. The decoded URL is `http://gosspublic.alicdn.com/callback_pub_key_v1.pem`.
    2.  Decode signature header `kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==` in Base64. The decoded result cannot be displayed because it is a nonprintable string. Therefore, the result cannot be displayed.
    3.  Obtain the string-to-sign, which is url\_decode\("index.php"\) + "?id=1&index=2" + "\\n" + "bucket=yonghu-test", and perform MD5 verification on the string.
    4.  Verify the signature.
-   Application server example

    The following Python code shows you how an application server verifies a signature. Before you run the code, the M2Crypto library must be installed.

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

    The following table describes the code in other programming languages used to verify a signature on the server.

    |Programming language|Description|
    |--------------------|-----------|
    |Java|    -   Download link: [Java](https://gosspublic.alicdn.com/images/AppCallbackServer.zip)
    -   Running method: Decompress the package and run `java -jar oss-callback-server-demo.jar 9000`. 9000 is the port number and can be customized. |
    |Python|    -   Download link: [Python](https://gosspublic.alicdn.com/images/callback_app_server.py.zip)
    -   Running method: Decompress the package and run `python callback_app_server.py`. Before you run the code, RSA dependencies must be installed. |
    |Go|    -   Download link: [Go](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048745465/callback-server-go.zip)
    -   Running method: Decompress the package and follow `README.md`. |
    |PHP|    -   Download link: [PHP](https://gosspublic.alicdn.com/callback-php-demo.zip)
    -   Running method: Deploy the code to an Apache environment because some headers in the PHP code depend on the environment. You can modify the example code based on the environment. |
    |.NET|    -   Download link: [.NET](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/131396/intl_en/1597211267799/callback-server-dotnet-20200810.zip)
    -   Running method: Decompress the package and follow `README.md`. |
    |Node.js|    -   Download link: [Node.js](http://gosspublic.alicdn.com/doc/oss-doc/callback-nodejs-demo.zip)
    -   Running method: Decompress the package and directly run `node example.js`. |
    |Ruby|    -   Download link: [Ruby](https://github.com/rockuw/oss-callback-server)
    -   Running method: Run the ruby aliyun\_oss\_callback\_server.rb command. |


## Step 5: Return the callback result

The application server returns the response to OSS.

The following response is the response to the callback request:

```
HTTP/1.0 200 OK
Server: BaseHTTP/0.3 Python/2.7.6
Date: Mon, 14 Sep 2015 12:37:27 GMT
Content-Type: application/json
Content-Length: 9
{"a":"b"}
```

**Note:** The response returned by the application server to OSS must contain the Content-Length header. The size of the response body cannot exceed 1 MB.

## Step 6: Return the upload result

OSS returns the information that is returned by the application server to the user.

The following example shows a returned response:

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

**Note:**

-   The body of responses for some requests such as CompleteMultipartUpload requests contains content such as information in the XML format. If you use the upload callback function, the original body content such as `{"a":"b"}` is overwritten. Exercise caution when you implement upload callback.
-   If the upload callback fails, HTTP status code 203 and the CallbackFailed error code are returned. This response indicates that the object is uploaded to OSS. However, the callback fails. A callback failure only indicates that OSS does not receive the expected callback response. The failure does not indicate that the application server does not receive a callback request. For example, if the response returned by the application server is not in the JSON format, the callback fails.

