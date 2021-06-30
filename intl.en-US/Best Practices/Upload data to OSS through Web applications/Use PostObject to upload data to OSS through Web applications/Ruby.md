# Ruby

This topic describes how to calculate signatures in Ruby on the server, configure upload callback, and use form upload to upload data to OSS.

-   The domain name of the application server is accessible over the Internet.
-   The application server has Ruby 2.0 or later installed. To view the Ruby version, run the ruby -v command.
-   The browser on the PC supports JavaScript.

## Step 1: Configure the application server

1.  Download the [application server source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537974391908/aliyun-oss-appserver-ruby-master.zip?spm=a2c4g.11186623.2.13.61284c07AGniJC&file=aliyun-oss-appserver-ruby-master.zip) that is in Ruby.

2.  Ubuntu 16.04 is used in the example. Decompress the source code to the /home/aliyun/aliyun-oss-appserver-ruby directory.

3.  Go to the directory. Open the appserver.rb file that contains the source code. Modify the following snippet:

    ```
    # Enter your AccessKey ID.
    $access_key_id = '<yourAccessKeyId>'
    
    # Enter your AccessKey secret.
    $access_key_secret = '<yourAccessKeySecret>'
    
    # Set $host to a value that is in the format of bucketname.endpoint.
    $host = 'https://bucket-name.oss-cn-hangzhou.aliyuncs.com';
    
    # Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
    $callback_url = "http://88.88.88.88:8888";
    
    # Specify the prefix for the name of the object you want to upload.
    $upload_dir = 'user-dir-prefix/'
    ```

    -   $access\_key\_id: Enter your AccessKey ID.
    -   $access\_key\_secret: Enter your AccessKey secret.
    -   $host: The format is https://bucketname.endpoint. Example: https://bucket-name.oss-cn-hangzhou.aliyuncs.com. For more information about endpoints, see the "Endpoint" section in [Terms](/intl.en-US/Developer Guide/Terms.md).
    -   $callback\_url: Set the URL of the server to which an upload callback request is sent. This URL is used to communicate between the application server and OSS. After an object is uploaded, OSS sends the object upload information to the application server by using this URL. In this example, set the URL to `$callback_url="http://11.22.33.44:1234";`.
    -   $upload\_dir: Specify the prefix for the name of the object. You can also leave this parameter unspecified.

## Step 2: Configure the client

1.  Download the [client source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.61284c07AGniJC&file=aliyun-oss-appserver-js-master.zip).

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

    In the /home/aliyun/aliyun-oss-appserver-ruby directory, run the ruby appserver.rb 11.22.33.44 1234 command to start the application server.

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
    def get_token()
        expire_syncpoint = Time.now.to_i + $expire_time
    
        expire = Time.at(expire_syncpoint).utc.iso8601()
        response.headers['expire'] = expire
    
        policy_dict = {}
        condition_arrary = Array.new
        array_item = Array.new
        array_item.push('starts-with')
        array_item.push('$key')
        array_item.push($upload_dir)
        condition_arrary.push(array_item)
        policy_dict["conditions"] = condition_arrary
        policy_dict["expiration"] = expire
        policy = hash_to_jason(policy_dict)
        policy_encode = Base64.strict_encode64(policy).chomp;
        h = OpenSSL::HMAC.digest('sha1', $access_key_secret, policy_encode)
        hs = Digest::MD5.hexdigest(h)
        sign_result = Base64.strict_encode64(h).strip()
    
        callback_dict = {}
        callback_dict['callbackBodyType'] = 'application/x-www-form-urlencoded';
        callback_dict['callbackBody'] = 'filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}';
        callback_dict['callbackUrl'] = $callback_url;
        callback_param = hash_to_jason(callback_dict)
        base64_callback_body = Base64.strict_encode64(callback_param);
    
        token_dict = {}
        token_dict['accessid'] = $access_key_id
        token_dict['host'] = $host
        token_dict['policy'] = policy_encode
        token_dict['signature'] = sign_result 
        token_dict['expire'] = expire_syncpoint
        token_dict['dir'] = $upload_dir
        token_dict['callback'] = base64_callback_body
        response.headers["Access-Control-Allow-Methods"] = "POST"
        response.headers["Access-Control-Allow-Origin"] = "*"
        result = hash_to_jason(token_dict)
    
        result
    end
    
    get '/*' do
      puts "********************* GET "
      get_token()
    end
    ```

-   Upload callback

    During upload callback, the application server responds to the POST message that is sent from OSS. An example of the snippet:

    ```
    post '/*' do
      puts "********************* POST"
      pub_key_url = Base64.decode64(get_header('x-oss-pub-key-url'))
      pub_key = get_public_key(pub_key_url)
      rsa = OpenSSL::PKey::RSA.new(pub_key)
    
      authorization = Base64.decode64(get_header('authorization'))
      req_body = request.body.read
      if request.query_string.empty? then
        auth_str = CGI.unescape(request.path) + "\n" + req_body
      else
        auth_str = CGI.unescape(request.path) + '?' + request.query_string + "\n" + req_body
      end
    
      valid = rsa.public_key.verify(
        OpenSSL::Digest::MD5.new, authorization, auth_str)
    
      if valid
        #body({'Status' => 'OK'}.to_json)
        body(hash_to_jason({'Status' => 'OK'}))
      else
        halt 400, "Authorization failed!"
      end
    end
    ```

    For more information, see the "\(Optional\) Step 4: Sign the callback request" section in [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) of the OSS API Reference.


