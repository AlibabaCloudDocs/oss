# PHP

本文以PHP语言为例，讲解在服务端通过PHP代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

-   Web服务器已部署。
-   Web服务器对应的域名可通过公网访问。
-   Web服务器能够解析PHP（执行命令`php -v`进行查看）。
-   PC端浏览器支持JavaScript。

## 步骤1：配置Web服务器

本文档以Ubuntu16.04和Apache2.4.18为例，环境配置如下。

-   Web服务器外网IP地址为`11.22.33.44`。您可以在配置文件`/etc/apache2/apache2.conf`中增加`ServerName 11.22.33.44`来进行修改。
-   Web服务器的监听端口为`8080`。您可以在配置文件`/etc/apache2/ports.conf`中进行修改相关内容`Listen 8080`。
-   确保Apache能够解析PHP文件：`sudo apt-get install libapache2-mod-php5`（其他平台请根据实际情况进行安装配置）。

您可以根据自己的实际环境修改IP地址和监听端口。更新配置后，需要重启Apache服务器，重启命令为/etc/init.d/apache2 restart。

## 步骤2：配置应用服务器

1.  下载[应用服务器源码](https://gosspublic.alicdn.com/doc/91771/aliyun-oss-appserver-php-master-new.rar)（PHP版本）。

2.  将应用服务器源码解压到应用服务器的相应目录。本文示例中需要部署到Ubuntu16.04的/var/www/html/aliyun-oss-appserver-php目录下。

3.  PC端浏览器中访问应用服务器URL `http://11.22.33.44:8080/aliyun-oss-appserver-php/index.html`， 显示的页面内容与[测试样例主页](http://oss-demo.aliyuncs.com/oss-h5-upload-js-php-callback/index.html?spm=a2c4g.11186623.2.19.63f561e4APLM8H)相同则验证通过。

4.  开启Apache捕获HTTP头部Authorization字段的功能。

    您的应用服务器收到的回调请求有可能没有Authotization头，这是因为有些Web应用服务器会将Authorization头自行解析掉。例如Apache2，需要设置成不解析头部。

    1.  打开Apache2配置文件/etc/apache2/apache2.conf，找到如下片段进行相应修改。

        ```
        <Directory /var/www/>
                Options Indexes FollowSymLinks
                AllowOverride All
                Require all granted
        </Directory>
        ```

    2.  在/var/www/html/aliyun-oss-appserver-php目录下，新建一个.htaccess文件，填写如下内容。

        ```
        <IfModule mod_rewrite.c>
        RewriteEngine on
        RewriteCond %{HTTP:Authorization} .
        RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]
        </IfModule>
        ```

    其他Web服务器或其他Apache版本，请根据实际情况进行配置。

5.  修改应用服务器配置。

    在/var/www/html/aliyun-oss-appserver-php/php目录下打开文件get.php， 修改如下代码片段。

    ```
    $id= '<yourAccessKeyId>';          // 请填写您的AccessKeyId。
        $key= '<yourAccessKeySecret>';     // 请填写您的AccessKeySecret。
    
        // $host的格式为https://bucketname.endpointx，请替换为您的真实信息。
        $host = 'https://bucket-name.oss-cn-hangzhou.aliyuncs.com';  
    
        // $callbackUrl为上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实URL信息。
        $callbackUrl = 'http://88.88.88.88:8888/aliyun-oss-appserver-php/php/callback.php';
    
        $dir = 'user-dir-prefix/';          // 上传文件时指定的前缀。
    ```

    -   id：设置您的AccessKeyId。
    -   key：设置您的AccessKeySecret。
    -   host：格式为`https://BucketName.Endpoint`，例如`https://bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](/intl.zh-CN/开发指南/基本概念.md)。
    -   callbackUrl：设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之间的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：`$callbackUrl='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/callback.php';`。
    -   dir：若要设置上传到OSS文件的前缀则需要配置此项，否则置空即可。

## 步骤3：配置客户端

在应用服务器的/var/www/html/aliyun-oss-appserver-php目录下修改文件upload.js。

对于PHP版本的应用服务器源码，一般不需要修改文件upload.js内容，因为相对路径也是可以正常工作的。如果确实需要修改，请找到此段代码片段：`serverUrl ='./php/get.php'`，将`serverUrl`改成服务器部署的地址，用于处理浏览器和应用服务器之间的通信。例如本示例中可以修改为：`serverUrl ='http://11.22.33.44:8080/aliyun-oss-appserver-php/php/get.php'`。

## 步骤4：修改CORS

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证。因此需要为Bucket设置跨域规则以支持Post方法。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**权限管理** \> **跨域设置**，在**跨域设置**区域单击**设置**。

4.  单击**创建规则**，配置如下图所示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p12308.png)

    **说明：** 为了您的数据安全，实际使用时，**来源**建议填写实际允许访问的域名。更多配置信息请参见[设置跨域访问](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)。


## 步骤5：体验上传回调

1.  在PC端的Web浏览器中输入http://11.22.33.44:8080/aliyun-oss-appserver-php/index.html。

    **说明：** index.html文件不保证兼容IE 10以下版本浏览器，若使用IE 10以下版本浏览器出现问题时，您需要自行调试。

2.  单击**选择文件**，选择指定类型的文件后，单击**开始上传**。

    上传成功后，显示回调服务器返回的内容。


## 应用服务器核心代码解析

应用服务器源码包含了签名直传服务和上传回调服务两个功能。

-   签名直传服务

    签名直传服务响应客户端发送给应用服务器的GET消息，代码文件是aliyun-oss-appserver-php/php/get.php。代码片段如下：

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

-   上传回调服务

    上传回调服务响应OSS发送给应用服务器的POST消息，代码文件是aliyun-oss-appserver-php/php/callback.php。

    代码片段如下：

    ```
    // 6.验证签名
    $ok = openssl_verify($authStr, $authorization, $pubKey, OPENSSL_ALGO_MD5);
    if ($ok == 1)
    {
        header("Content-Type: application/json");
        $data = array("Status"=>"Ok");
        echo json_encode($data);
    }
    ```

    更多详情请参见API文档[Callback](/intl.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)。


