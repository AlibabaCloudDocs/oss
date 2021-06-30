# PHP

This topic describes how to calculate signatures in PHP on the server, configure upload callback, and use form upload to upload data to OSS.

-   A web server is deployed.
-   The domain name of the web server is accessible over the Internet.
-   The web server can parse PHP. To view the PHP version, run the `php -v` command.
-   The browser on the PC supports JavaScript.

## Step 1: Configure the web server

Ubuntu 16.04 and Apache 2.4.18 are used in the example. Environment configurations:

-   The public IP address of the web server is `11.22.33.44`. You can modify the public IP address by adding `ServerName 11.22.33.44` to the `/etc/apache2/apache2.conf` configuration file.
-   The listening port of the web server is `8080`. You can modify `Listen 8080` in the `/etc/apache2/ports.conf` configuration file.
-   Ensure that Apache can parse PHP files. To install PHP 5, run the `sudo apt-get install libapache2-mod-php5` command. If other platforms are used, install PHP 5 and modify configurations.

You can modify the IP address and listening port. After you update the configurations, you must run /etc/init.d/apache2 restart to restart the Apache server.

## Step 2: Configure the application server

1.  Download the [application server source code](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92590/APP_zh/1539603337889/aliyun-oss-appserver-php-master.zip?spm=a2c4g.11186623.2.14.40e84c07nb3YkP&file=aliyun-oss-appserver-php-master.zip) in PHP.

2.  Decompress the application server source code to the corresponding directory. The web server must be deployed to the /var/www/html/aliyun-oss-appserver-php directory of Ubuntu 16.04 in this example.

3.  Access the `http://11.22.33.44:8080/aliyun-oss-appserver-php/index.html` URL of the application server from a browser on the PC. If the content of the displayed page is the same as that of the [homepage of test sample](http://oss-demo.aliyuncs.com/oss-h5-upload-js-php-callback/index.html?spm=a2c4g.11186623.2.19.63f561e4APLM8H), the verification is passed.

4.  Enable the Apache server to capture the Authorization HTTP header.

    The callback request received by your application server may not have an Authorization header. This is because some web application servers automatically parse the Authorization header. For example, you can modify the configuration file of Apache 2 so that the Authorization header is not parsed.

    1.  Open the /etc/apache2/apache2.conf configuration file of Apache 2. Find and modify the following segment:

        ```
        <Directory /var/www/>
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>
        ```

    2.  In the /var/www/html/aliyun-oss-appserver-php directory, create a .htaccess file, and enter the following content:

        ```
        <IfModule mod_rewrite.c>
        RewriteEngine on
        RewriteCond %{HTTP:Authorization} .
        RewriteRule . * - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        </IfModule>
        ```

    If other web servers or versions of Apache are used, install and configure them.

5.  Modify the configurations for the application server

    In the /var/www/html/aliyun-oss-appserver-php/php directory, open the get.php file. Modify the following snippet:

    ```
    $id= '<yourAccessKeyId>';          // Enter your AccessKey ID.
        $key= '<yourAccessKeySecret>';     // Enter your AccessKey secret.
    
        // Set $host to a value that is in the format of https://bucketname.endpointx.
        $host = 'https://bucket-name.oss-cn-hangzhou.aliyuncs.com';  
    
        // Set the URL of the server to which an upload callback request is sent. Replace the IP address and port number with your actual information.
        $callbackUrl = 'http://88.88.88.88:8888/aliyun-oss-appserver-php/php/callback.php';
    
        $dir = 'user-dir-prefix/';          // Specify the prefix for the name of the object you want to upload.
    ```

    -   $id: Enter your AccessKey ID.
    -   $key: Enter your AccessKey secret.
    -   $host: The format is `https://BucketName.Endpoint`. Example: `https://bucket-name.oss-cn-hangzhou.aliyuncs.com`. For more information about endpoints, see the "Endpoint" section in [Terms](/intl.en-US/Developer Guide/Terms.md).
    -   $callbackUrl: Set the URL of the server to which an upload callback request is sent. This URL is used to communicate between the application server and OSS. After an object is uploaded, OSS sends the object upload information to the application server by using this URL. In this example, set the URL to `$callbackUrl='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/callback.php';`.
    -   $dir: Specify the prefix of the object to prevent overwriting an existing object with the same name. You can also leave this parameter unspecified.

## Step 3: Configure the client

In the /var/www/html/aliyun-oss-appserver-php directory of the application server, modify the upload.js file.

For the PHP application server source code, you do not need to modify the content of the upload.js file, because relative paths can also work properly. If modification is required, find the snippet: `serverUrl ='./php/get.php'`. Change `serverUrl` to the address where the server is deployed to communicate between the browser and the application server. In this example, specify `serverUrl ='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/get.php'`.

## Step 4: Modify CORS configurations

When you use form upload to upload data from the client to OSS, the client includes the `Origin` header in the request and sends the request to OSS by using the browser. OSS verifies the request message that includes the `Origin` header for cross-origin resource sharing \(CORS\) verification. To use the POST method, configure CORS rules for a bucket.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Access Control** \> **Cross-Origin Resource Sharing \(CORS\)**. In the **Cross-Origin Resource Sharing \(CORS\)** section, click **Configure**.

4.  Click **Create Rule**. The following figure shows the configurations of a rule.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9354449951/p12308.png)

    **Note:** For data security reasons, enter a required domain name in the **Sources** field. For more information about CORS configurations, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).


## Step 5: Send an upload callback request

1.  Open the web browser on the PC, and enter http:// 11.22.33.44:8080/aliyun-oss-appserver-php/index.html.

    **Note:** The index.html file may be incompatible with Internet Explorer 10 or earlier. If you encounter any problems when you use Internet Explorer 10 or earlier, you must perform debugging.

2.  Click **Select File**. Select the file of a specified type. Click **Upload**.

    After the object is uploaded, the content returned by the callback server is displayed.


## Core code analysis of the application server

The source code of the application server is used to implement signature-based upload and upload callback.

-   Signature-based upload

    During signature-based upload, the application server responds to the GET message sent from the client. The code file is aliyun-oss-appserver-php/php/get.php. An example of the snippet:

    ```
    
    $response = array();
    $response['accessid'] = $id;
    $response['host'] = $host;
    $response['policy'] = $base64_policy;
    $response['signature'] = $signature;
    $response['expire'] = $end;
    $response['callback'] = $base64_callback_body;
    $response['dir'] = $dir; 
    ```

-   Upload callback

    During upload callback, the application server responds to the POST message that is sent from OSS. The code file is aliyun-oss-appserver-php/php/callback.php.

    An example of the snippet:

    ```
    // 6. Verify the signature.
    $ok = openssl_verify($authStr, $authorization, $pubKey, OPENSSL_ALGO_MD5);
    if ($ok == 1)
    {
        header("Content-Type: application/json");
        $data = array("Status"=>"Ok");
        echo json_encode($data);
    }
    ```

    For more information, see the "\(Optional\) Step 5: Sign the callback request" section in [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) of the OSS API Reference.


