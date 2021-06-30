# Java

本文以Java语言为例，讲解在服务端通过Java代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

-   应用服务器对应的域名可通过公网访问。
-   确保应用服务器已经安装`Java 1.6`以上版本（执行命令`java -version`进行查看）。
-   确保PC端浏览器支持JavaScript。

## 步骤1：配置应用服务器

1.  下载[应用服务器源码](https://gosspublic.alicdn.com/doc/91868/aliyun-oss-appserver-java-master.zip)（Java版本）。

2.  本示例中以`Ubuntu 16.04`为例，将下载的文件解压到/home/aliyun/aliyun-oss-appserver-java目录下。

3.  进入该目录，找到并打开源码文件CallbackServer.java，修改如下的代码片段：

    ```
    String accessId = "<yourAccessKeyId>";      
    String accessKey = "<yourAccessKeySecret>"; 
    String endpoint = "oss-cn-hangzhou.aliyuncs.com"; 
    String bucket = "bucket-name";                    
    String host = "https://" + bucket + "." + endpoint; 
    String callbackUrl = "http://88.88.88.88:8888";
    String dir = "user-dir-prefix/"; 
    ```

    -   accessId ： 设置您的AccessKeyId。
    -   accessKey ： 设置您的AessKeySecret。
    -   host： 格式为https://bucketname.endpoint，例如https://bucket-name.oss-cn-hangzhou.aliyuncs.com。关于Endpoint的介绍，请参见[Endpoint访问域名](/intl.zh-CN/开发指南/基本概念.md)。
    -   callbackUrl： 设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之间的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：`String callbackUrl ="http://11.22.33.44:1234";`。
    -   dir：若要设置上传到OSS文件的前缀则需要配置此项，否则置空即可。

## 步骤2：配置客户端

1.  下载[客户端源码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.45fa4c07Y7xS8I&file=aliyun-oss-appserver-js-master.zip)。

2.  将文件解压，本例中以解压到`D:\aliyun\aliyun-oss-appserver-js`目录为例。

3.  进入该目录，打开`upload.js`文件，找到下面的代码语句：

    ```
    // serverUrl是 用户获取 '签名和Policy' 等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    serverUrl = 'http://88.88.88.88:8888'
    ```

4.  将`severUrl`改成应用服务器的地址，客户端可以通过它获取签名直传Policy等信息。如本例中可修改为：`serverUrl = 'http://11.22.33.44:1234'`。


## 步骤3：修改CORS

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证。因此需要为Bucket设置跨域规则以支持Post方法。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**权限管理** \> **跨域设置**，在**跨域设置**区域单击**设置**。

4.  单击**创建规则**，配置如下图所示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p12308.png)

    **说明：** 为了您的数据安全，实际使用时，**来源**建议填写实际允许访问的域名。更多配置信息请参见[设置跨域访问](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)。


## 步骤4：体验上传回调

1.  启动应用服务器。

    在/home/aliyun/aliyun-oss-appserver-java目录下，执行mvn package命令编译打包，然后执行java -jar target/appservermaven-1.0.0.jar 1234命令启动应用服务器。

    **说明：** 请将IP和端口改成您配置的应用服务器的IP和端口。

    您也可以在PC端使用Eclipse/Intellij IDEA等IDE工具导出jar包，然后将jar包拷贝到应用服务器，再执行jar包启动应用服务器。

2.  启动客户端。

    1.  在PC端的客户端源码目录中，打开index.html文件。

        **说明：** index.html文件不保证兼容IE 10以下版本浏览器，若使用IE 10以下版本浏览器出现问题时，您需要自行调试。

    2.  单击**选择文件**，选择指定类型的文件，单击**开始上传**。

        上传成功后，显示回调服务器返回的内容。


## 应用服务器核心代码解析

应用服务器源码包含了签名直传服务和上传回调服务两个功能。

-   签名直传服务

    签名直传服务响应客户端发送给应用服务器的GET消息，代码片段如下：

    ```
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
    
            String accessId = "<yourAccessKeyId>"; // 请填写您的AccessKeyId。
            String accessKey = "<yourAccessKeySecret>"; // 请填写您的AccessKeySecret。
            String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // 请填写您的 endpoint。
            String bucket = "bucket-name"; // 请填写您的 bucketname 。
            String host = "https://" + bucket + "." + endpoint; // host的格式为 bucketname.endpoint
            // callbackUrl为 上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
            String callbackUrl = "http://88.88.88.88:8888";
            String dir = "user-dir-prefix/"; // 用户上传文件时指定的前缀。
    
            // 创建OSSClient实例。
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessId, accessKey);
            try {
                long expireTime = 30;
                long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
                Date expiration = new Date(expireEndTime);
                // PostObject请求最大可支持的文件大小为5 GB，即CONTENT_LENGTH_RANGE为5*1024*1024*1024。
                PolicyConditions policyConds = new PolicyConditions();
                policyConds.addConditionItem(PolicyConditions.COND_CONTENT_LENGTH_RANGE, 0, 1048576000);
                policyConds.addConditionItem(MatchMode.StartWith, PolicyConditions.COND_KEY, dir);
    
                String postPolicy = ossClient.generatePostPolicy(expiration, policyConds);
                byte[] binaryData = postPolicy.getBytes("utf-8");
                String encodedPolicy = BinaryUtil.toBase64String(binaryData);
                String postSignature = ossClient.calculatePostSignature(postPolicy);
    
                Map<String, String> respMap = new LinkedHashMap<String, String>();
                respMap.put("accessid", accessId);
                respMap.put("policy", encodedPolicy);
                respMap.put("signature", postSignature);
                respMap.put("dir", dir);
                respMap.put("host", host);
                respMap.put("expire", String.valueOf(expireEndTime / 1000));
                // respMap.put("expire", formatISO8601Date(expiration));
    
                JSONObject jasonCallback = new JSONObject();
                jasonCallback.put("callbackUrl", callbackUrl);
                jasonCallback.put("callbackBody",
                        "filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}");
                jasonCallback.put("callbackBodyType", "application/x-www-form-urlencoded");
                String base64CallbackBody = BinaryUtil.toBase64String(jasonCallback.toString().getBytes());
                respMap.put("callback", base64CallbackBody);
    
                JSONObject ja1 = JSONObject.fromObject(respMap);
                // System.out.println(ja1.toString());
                response.setHeader("Access-Control-Allow-Origin", "*");
                response.setHeader("Access-Control-Allow-Methods", "GET, POST");
                response(request, response, ja1.toString());
    
            } catch (Exception e) {
                // Assert.fail(e.getMessage());
                System.out.println(e.getMessage());
            } finally { 
                ossClient.shutdown();
            }
        }
    ```

-   上传回调服务

    上传回调服务响应OSS发送给应用服务器的POST消息，代码片段如下：

    ```
    protected boolean VerifyOSSCallbackRequest(HttpServletRequest request, String ossCallbackBody)
                throws NumberFormatException, IOException {
            boolean ret = false;
            String autorizationInput = new String(request.getHeader("Authorization"));
            String pubKeyInput = request.getHeader("x-oss-pub-key-url");
            byte[] authorization = BinaryUtil.fromBase64String(autorizationInput);
            byte[] pubKey = BinaryUtil.fromBase64String(pubKeyInput);
            String pubKeyAddr = new String(pubKey);
            if (!pubKeyAddr.startsWith("https://gosspublic.alicdn.com/")
                    && !pubKeyAddr.startsWith("https://gosspublic.alicdn.com/")) {
                System.out.println("pub key addr must be oss addrss");
                return false;
            }
            String retString = executeGet(pubKeyAddr);
            retString = retString.replace("-----BEGIN PUBLIC KEY-----", "");
            retString = retString.replace("-----END PUBLIC KEY-----", "");
            String queryString = request.getQueryString();
            String uri = request.getRequestURI();
            String decodeUri = java.net.URLDecoder.decode(uri, "UTF-8");
            String authStr = decodeUri;
            if (queryString != null && !queryString.equals("")) {
                authStr += "?" + queryString;
            }
            authStr += "\n" + ossCallbackBody;
            ret = doCheck(authStr, authorization, retString);
            return ret;
        }
    
        protected void doPost(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
            String ossCallbackBody = GetPostBody(request.getInputStream(),
                    Integer.parseInt(request.getHeader("content-length")));
            boolean ret = VerifyOSSCallbackRequest(request, ossCallbackBody);
            System.out.println("verify result : " + ret);
            // System.out.println("OSS Callback Body:" + ossCallbackBody);
            if (ret) {
                response(request, response, "{\"Status\":\"OK\"}", HttpServletResponse.SC_OK);
            } else {
                response(request, response, "{\"Status\":\"verify not ok\"}", HttpServletResponse.SC_BAD_REQUEST);
            }
        }
    ```

    更多详情请参见API文档[Callback](/intl.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)。


