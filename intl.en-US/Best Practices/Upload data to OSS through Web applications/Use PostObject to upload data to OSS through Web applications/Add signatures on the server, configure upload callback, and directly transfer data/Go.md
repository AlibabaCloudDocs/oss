# Go

This topic describes how to calculate signatures in Go on the server, configure upload callback, and use form upload to upload data to OSS.

-   The domain name of the application server is accessible over the Internet.
-   The application server has Go 1.6 or later installed. To verify the Go version, run the `go version` command.
-   The browser on the PC supports JavaScript.

## Step 1: Configure the application server

1.  Download the [application server source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/181705/cn_zh/1598870585874/aliyun-oss-appserver-go-master.zip) that is in Go.

2.  Ubuntu 16.04 is used in the example. Download the source code to the `/home/aliyun/aliyun-oss-appserver-go` directory.

3.  Go to the directory. Open the `appserver.go` file that contains the source code. Modify the following snippet:

    ```
    // Enter your AccessKey ID.
    var accessKeyId string = "<yourAccessKeyId>"
    
    // Enter your AccessKey secret.
    var accessKeySecret string = "<yourAccessKeySecret>"
    
    // Set host to a value that is in the format of bucketname.endpoint.
    var host string = "https://bucket-name.oss-cn-hangzhou.aliyuncs.com'"
    
    // Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
    var callbackUrl string = "http://88.88.88.88:8888";
    
    // Specify the prefix for the name of the object you want to upload.
    var upload_dir string = "user-dir-prefix/"
    
    // Set the validity period in seconds for the upload policy.
    var expire_time int64 = 30
    ```

    -   accessKeyId: Enter your AccessKey ID.
    -   accessKeySecret: Enter your AccessKey secret.
    -   host: The format is `https://bucketname.endpoint`. Example: `https://bucket-name.oss-cn-hangzhou.aliyuncs.com`. For more information about endpoints, see the "Endpoint" section in [Terms](/intl.en-US/Developer Guide/Terms.md).
    -   callbackUrl: Set the URL of the server to which an upload callback request is sent. This URL is used to communicate between the application server and OSS. After an object is uploaded, OSS sends the object upload information to the application server by using this URL. In this example, set the URL to `var callbackUrl string="http://11.22.33.44:1234";`.
    -   dir: Specify the prefix for the name of the object. You can also leave this parameter unspecified.

## Step 2: Configure the client

1.  Download the [client source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.76dd4c07uaPzqG&file=aliyun-oss-appserver-js-master.zip) to the local directory on the PC.

2.  Decompress the package. Open the upload.js file. Find the following code:

    ```
    // serverUrl specifies the URL of the application server used to obtain information such as the signature and policy. Replace the IP address and port number with your actual information.
    serverUrl ='http://88.88.88.88:8888'
    ```

3.  Set severUrl to the URL of the application server. In this example, specify `serverUrl ='http://11.22.33.44:1234'`.


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

    In the `/home/aliyun/aliyun-oss-appserver-go` directory, run the go run appserver.go 11.22.33.44 1234 command.

    **Note:** Replace the IP address and port number with those of the application server you configured.

2.  Start the client.

    1.  On the PC, open the index.html file in the directory that contains the client source code.

        **Note:** The index.html file may be incompatible with Internet Explorer 10 or earlier. If you encounter any problems when you use Internet Explorer 10 or earlier, you must perform debugging.

    2.  Click **Select File**. Select the file of a specified type. Click **Upload**. After the object is uploaded, the content returned by the callback server is displayed.


## Core code analysis of the application server

The source code of the application server is used to implement signature-based upload and upload callback.

-   Signature-based upload

    During signature-based upload, the application server responds to the GET message that is sent from the client. An example of the snippet:

    ```
    func handlerRequest(w http.ResponseWriter, r *http.Request) {   
            if (r.Method == "GET") {
                    response := get_policy_token()
                    w.Header().Set("Access-Control-Allow-Methods", "POST")
                    w.Header().Set("Access-Control-Allow-Origin", "*")
                    io.WriteString(w, response)
            }
    ```

-   Upload callback

    During upload callback, the application server responds to the POST message that is sent from OSS. An example of the snippet:

    ```
    if (r.Method == "POST") {
                    fmt.Println("\nHandle Post Request ... ")
    
                    // Get PublicKey bytes
                    bytePublicKey, err := getPublicKey(r)
                    if (err ! = nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get Authorization bytes : decode from Base64String
                    byteAuthorization, err := getAuthorization(r)
                    if (err ! = nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get MD5 bytes from Newly Constructed Authorization String. 
                    byteMD5, err := getMD5FromNewAuthString(r)
                    if (err ! = nil) {
                            responseFailed(w)
                            return
                    }
    
                    // verifySignature and response to client 
                    if (verifySignature(bytePublicKey, byteMD5, byteAuthorization)) {
                            // do something you want according to callback_body ...
    
                            responseSuccess(w)  // response OK : 200  
                    } else {
                            responseFailed(w)   // response FAILED : 400 
                    }
            }
    ```

    For more information, see the "\(Optional\) Step 4: Sign the callback request" section in [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) of the OSS API Reference.


