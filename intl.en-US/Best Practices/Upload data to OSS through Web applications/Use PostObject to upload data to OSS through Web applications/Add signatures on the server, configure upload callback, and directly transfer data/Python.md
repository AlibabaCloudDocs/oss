# Python

This topic describes how to calculate signatures in Python on the server, configure upload callback, and use form upload to upload data to OSS.

-   The domain name of the application server is accessible over the Internet.
-   The application server has Python 2.6 or later installed. To view the Python version, run the python --version command.
-   The browser on the PC supports JavaScript.

## Step 1: Configure the application server

1.  Download the [application server source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/97721/cn_zh/1545016122500/aliyun-oss-appserver-python-master.zip?spm=a2c4g.11186623.2.13.22934c07ZwO1Fy&file=aliyun-oss-appserver-python-master.zip) that is in Python.

2.  Ubuntu 16.04 is used in the example. Decompress the source code to the /home/aliyun/aliyun-oss-appserver-python directory.

3.  Go to the directory. Open the appserver.py file that contains the source code. Modify the following snippet:

    ```
    # Enter your AccessKey ID.
    access_key_id = '<yourAccessKeyId>'
    # Enter your AccessKey secret.
    access_key_secret = '<yourAccessKeySecret>'
    # Set host to a value that is in the format of bucketname.endpoint.
    host = 'https://bucket-name.oss-cn-hangzhou.aliyuncs.com';
    # Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
    callback_url = "http://88.88.88.88:8888";
    # Specify the prefix for the name of the object you want to upload.
    upload_dir = 'user-dir-prefix/'
    ```

    -   access\_key\_id: Enter your AccessKey ID.
    -   access\_key\_secret: Enter your AccessKey secret.
    -   host: The format is https://bucketname.endpoint. Example: https://bucket-name.oss-cn-hangzhou.aliyuncs.com. For more information about endpoints, see the "Endpoint" section in [Terms](/intl.en-US/Developer Guide/Terms.md).
    -   callback\_url: Set the URL of the server to which an upload callback request is sent. This URL is used to communicate between the application server and OSS. After an object is uploaded, OSS sends the object upload information to the application server by using this URL. In this example, set the URL to `var callbackUrl string ="http://11.22.33.44:1234";`.
    -   upload\_dir: Specify the prefix for the name of the object. You can also leave this parameter unspecified.

## Step 2: Configure the client

1.  Download the [client source code](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.22934c07ZwO1Fy&file=aliyun-oss-appserver-js-master.zip).

2.  Decompress the package to a directory. The D:\\aliyun\\aliyun-oss-appserver-js directory is used in the example.

3.  Go to the directory. Open the upload.js file. Find the following code:

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

    In the /home/aliyun-oss-appserver-python directory, run the python appserver.py 11.22.33.44 1234 command to start the application server.

    **Note:** Replace the IP address and port number with those of the application server you configured.

2.  Start the client.

    On the PC, open the index.html file in the directory that contains the client source code.

    **Note:** The index.html file may be incompatible with Internet Explorer 10 or earlier. If you encounter any problems when you use Internet Explorer 10 or earlier, you must perform debugging.

3.  Upload an object.

    Click **Select File**. Select the file of a specified type. Click **Upload**. After the object is uploaded, the content returned by the callback server is displayed.


## Core code analysis of the application server

The source code of the application server is used to implement signature-based upload and upload callback.

-   Signature-based upload

    During signature-based upload, the application server responds to the GET message that is sent from the client. An example of the snippet:

    ```
    def do_GET(self):
            print "********************* do_GET "
            token = get_token()
            self.send_response(200)
            self.send_header('Access-Control-Allow-Methods', 'POST')
            self.send_header('Access-Control-Allow-Origin', '*')
            self.send_header('Content-Type', 'text/html; charset=UTF-8')
            self.end_headers()
            self.wfile.write(token)
    ```

-   Upload callback

    During upload callback, the application server responds to the POST message that is sent from OSS. An example of the snippet:

    ```
    def do_POST(self):
            print "********************* do_POST "
            # get public key
            pub_key_url = ''
            try:
                pub_key_url_base64 = self.headers['x-oss-pub-key-url']
                pub_key_url = pub_key_url_base64.decode('base64')
                url_reader = urllib2.urlopen(pub_key_url)
                pub_key = url_reader.read()
            except:
                print 'pub_key_url : ' + pub_key_url
                print 'Get pub key failed!'
                self.send_response(400)
                self.end_headers()
                return
    
            # get authorization
            authorization_base64 = self.headers['authorization']
            authorization = authorization_base64.decode('base64')
    
            # get callback body
            content_length = self.headers['content-length']
            callback_body = self.rfile.read(int(content_length))
    
            # compose authorization string
            auth_str = ''
            pos = self.path.find('?')
            if -1 == pos:
                auth_str = self.path + '\n' + callback_body
            else:
                auth_str = urllib2.unquote(self.path[0:pos]) + self.path[pos:] + '\n' + callback_body
    
            # verify authorization
            auth_md5 = md5.new(auth_str).digest()
            bio = BIO.MemoryBuffer(pub_key)
            rsa_pub = RSA.load_pub_key_bio(bio)
            try:
                result = rsa_pub.verify(auth_md5, authorization, 'md5')
            except e:
                result = False
    
            if not result:
                print 'Authorization verify failed!'
                print 'Public key : %s' % (pub_key)
                print 'Auth string : %s' % (auth_str)
                self.send_response(400)
                self.end_headers()
                return
    
            # do something accoding to callback_body
    
            # response to OSS
            resp_body = '{"Status":"OK"}'
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.send_header('Content-Length', str(len(resp_body)))
            self.end_headers()
            self.wfile.write(resp_body)
    ```

    For more information, see the "\(Optional\) Step 4: Sign the callback request" section in [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) of the OSS API Reference.


