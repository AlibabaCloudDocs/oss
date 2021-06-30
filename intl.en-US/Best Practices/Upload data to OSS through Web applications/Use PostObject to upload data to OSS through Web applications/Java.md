# Java

This topic describes how to calculate signatures in Java on the server, configure upload callback, and use form upload to upload data to OSS.

-   The domain name of the application server is accessible over the Internet.
-   The application server has `Java 1.6` or later installed. To view the Java version, run the `java -version` command.
-   The browser on the PC supports JavaScript.

## Step 1: Configure the application server

1.  Download the [application server source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537973714934/aliyun-oss-appserver-java-master.zip?spm=a2c4g.11186623.2.13.45fa4c07Y7xS8I&file=aliyun-oss-appserver-java-master.zip) that is in Java.

2.  `Ubuntu 16.04` is used in the example. Decompress the source code to the /home/aliyun/aliyun-oss-appserver-java directory.

3.  Go to the directory. Open the CallbackServer.java file that contains the source code. Modify the following snippet:

    ```
    String accessId = "<yourAccessKeyId>";      // Enter your AccessKey ID.
    String accessKey = "<yourAccessKeySecret>"; // Enter your AccessKey secret.
    String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // Enter your endpoint.
    String bucket = "bucket-name";                    // Enter your bucket name.
    String host = "https://" + bucket + "." + endpoint; // Set host to a value in the format of bucketname.endpoint.
    
    // Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
    String callbackUrl = "http://88.88.88.88:8888";
    String dir = "user-dir-prefix/"; // Specify the prefix for the name of the object to be uploaded.
    ```

    -   accessId: Enter your AccessKey ID.
    -   accessKeySecret: Enter your AccessKey secret.
    -   host: The format is https://bucketname.endpoint. Example: https://bucket-name.oss-cn-hangzhou.aliyuncs.com. For more information about endpoints, see the "Endpoint" section in [Terms](/intl.en-US/Developer Guide/Terms.md).
    -   callbackUrl: Set the URL of the server to which an upload callback request is sent. This URL is used to communicate between the application server and OSS. After an object is uploaded, OSS sends the object upload information to the application server by using this URL. In this example, set the URL to `String callbackUrl ="http://11.22.33.44:1234";`.
    -   dir: Specify the prefix for the name of the object. You can also leave this parameter unspecified.

## Step 2: Configure the client

1.  Download the [client source code](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.45fa4c07Y7xS8I&file=aliyun-oss-appserver-js-master.zip).

2.  Decompress the package to a directory. The `D:\aliyun\aliyun-oss-appserver-js` directory is used in the example.

3.  Go to the directory. Open the `upload.js` file. Find the following code:

    ```
    // serverUrl specifies the URL of the application server used to obtain information such as the signature and policy. Replace the IP address and port number with your actual information.
    serverUrl = 'http://88.88.88.88:8888'
    ```

4.  Set `severUrl` to the URL of the application server. In this example, specify `serverUrl = 'http://11.22.33.44:1234'`.


## Step 3: Modify CORS configurations

When you use form upload to upload data from the client to OSS, the client includes the `Origin` header in the request and sends the request to OSS by using the browser. OSS verifies the request message that includes the `Origin` header for cross-origin resource sharing \(CORS\) verification. To use the POST method, configure CORS rules for a bucket.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Access Control** \> **Cross-Origin Resource Sharing \(CORS\)**. In the **Cross-Origin Resource Sharing \(CORS\)** section, click **Configure**.

4.  Click **Create Rule**. The following figure shows the configurations of a rule.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9354449951/p12308.png)

    **Note:** For data security reasons, enter a required domain name in the **Sources** field. For more information about CORS configurations, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).


## Step 4: Send an upload callback request

1.  Start the application server.

    In the /home/aliyun/aliyun-oss-appserver-java directory, run the mvn package command to compile and package the code. Run the java -jar target/appservermaven-1.0.0.jar 1234 command to start the application server.

    **Note:** Replace the IP address and port number with those of the application server you configured.

    You can export the JAR package on the PC by using an integrated development environment \(IDE\), such as Eclipse or IntelliJ IDEA, copy the JAR package to the application server with an IDE, and run the JAR package to start the application server.

2.  Start the client.

    1.  On the PC, open the index.html file in the directory that contains the client source code.

        **Note:** The index.html file may be incompatible with Internet Explorer 10 or earlier. If you encounter any problems when you use Internet Explorer 10 or earlier, you must perform debugging.

    2.  Click **Select File**. Select the file of a specified type. Click **Upload**.

        After the object is uploaded, the content returned by the callback server is displayed.


## Core code analysis of the application server

The source code of the application server is used to implement signature-based upload and upload callback.

-   Signature-based upload

    During signature-based upload, the application server responds to the GET message that is sent from the client. An example of the snippet:

    ```
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
    
            String accessId = "<yourAccessKeyId>"; // Enter your AccessKey ID.
            String accessKey = "<yourAccessKeySecret>"; // Enter your AccessKey secret.
            String endpoint = "oss-cn-hangzhou.aliyuncs.com"; // Enter your endpoint.
            String bucket = "bucket-name"; // Enter your bucket name.
            String host = "https://" + bucket + "." + endpoint; // Set host to a value in the format of bucketname.endpoint.
            // Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
            String callbackUrl = "http://88.88.88.88:8888";
            String dir = "user-dir-prefix/"; // Specify the prefix for the name of the object to be uploaded.
    
            // Create an OSSClient instance.
            OSS ossClient = new OSSClientBuilder().build(endpoint, accessId, accessKey);
            try {
                long expireTime = 30;
                long expireEndTime = System.currentTimeMillis() + expireTime * 1000;
                Date expiration = new Date(expireEndTime);
                // You can upload an object up to 5 GB in size by using PostObject. To upload an object that is 5 GB in size, set CONTENT_LENGTH_RANGE to 5*1024*1024*1024.
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

-   Upload callback

    During upload callback, the application server responds to the POST message that is sent from OSS. An example of the snippet:

    ```
    protected boolean VerifyOSSCallbackRequest(HttpServletRequest request, String ossCallbackBody)
                throws NumberFormatException, IOException {
            boolean ret = false;
            String autorizationInput = new String(request.getHeader("Authorization"));
            String pubKeyInput = request.getHeader("x-oss-pub-key-url");
            byte[] authorization = BinaryUtil.fromBase64String(autorizationInput);
            byte[] pubKey = BinaryUtil.fromBase64String(pubKeyInput);
            String pubKeyAddr = new String(pubKey);
            if (! pubKeyAddr.startsWith("https://gosspublic.alicdn.com/")
                    && ! pubKeyAddr.startsWith("https://gosspublic.alicdn.com/")) {
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
            if (queryString ! = null && ! queryString.equals("")) {
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

    For more information, see the "\(Optional\) Step 4: Sign the callback request" section in [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) of the OSS API Reference.


