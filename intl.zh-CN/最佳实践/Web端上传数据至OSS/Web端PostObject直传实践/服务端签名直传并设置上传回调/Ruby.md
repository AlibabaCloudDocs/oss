# Ruby

本文以Ruby语言为例，讲解在服务端通过Ruby代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

-   应用服务器对应的域名可通过公网访问。
-   确保应用服务器已经安装Ruby 2.0以上版本（执行ruby -v命令进行查看）。
-   确保PC端浏览器支持JavaScript。

## 步骤1：配置应用服务器

1.  下载[应用服务器源码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537974391908/aliyun-oss-appserver-ruby-master.zip?spm=a2c4g.11186623.2.13.61284c07AGniJC&file=aliyun-oss-appserver-ruby-master.zip)（Ruby版本）。

2.  以Ubuntu 16.04为例，将文件解压到/home/aliyun/aliyun-oss-appserver-ruby目录下。

3.  进入该目录，打开源码文件appserver.rb，修改如下代码片段：

    ```
    # 请填写您的AccessKeyId。
    $access_key_id = '<yourAccessKeyId>'
    
    # 请填写您的AccessKeySecret。
    $access_key_secret = '<yourAccessKeySecret>'
    
    # $host的格式为bucketname.endpoint，请替换为您的真实信息。
    $host = 'https://bucket-name.oss-cn-hangzhou.aliyuncs.com';
    
    # $callbackUrl为上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    $callback_url = "http://88.88.88.88:8888";
    
    # 用户上传文件时指定的前缀。
    $upload_dir = 'user-dir-prefix/'
    ```

    -   $access\_key\_id：设置您的AccessKeyId。
    -   $access\_key\_secret：设置您的AessKeySecret。
    -   $host：格式为https://bucketname.endpoint，例如https://bucket-name.oss-cn-hangzhou.aliyuncs.com。关于Endpoint的介绍，请参见[Endpoint访问域名](/intl.zh-CN/开发指南/基本概念.md)。
    -   $callback\_url：设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之间的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为：`$callback_url="http://11.22.33.44:1234";`。
    -   $upload\_dir：若要设置上传到OSS文件的前缀则需要配置此项，否则置空即可。

## 步骤2：配置客户端

1.  下载[客户端源码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.61284c07AGniJC&file=aliyun-oss-appserver-js-master.zip)。

2.  将文件解压，本示例解压至D:\\aliyun\\aliyun-oss-appserver-js目录。

3.  进入该目录，打开upload.js文件，找到下面的代码语句：

    ```
    // serverUrl是用户获取签名和Policy等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
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


## 步骤 4：体验上传回调

1.  启动应用服务器。

    在/home/aliyun/aliyun-oss-appserver-ruby目录下，执行ruby appserver.rb 11.22.33.44 1234命令启动应用服务器。

    **说明：** 请将IP和端口改成您配置的应用服务器的IP和端口。

2.  启动客户端。

    在PC端的客户端源码目录中，打开index.html文件。

    **说明：** index.html文件不保证兼容IE 10以下版本浏览器，若使用IE 10以下版本浏览器出现问题时，您需要自行调试。

3.  上传文件。

    单击**选择文件**，选择指定类型的文件后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。


## 应用服务器核心代码解析

应用服务器源码包含了签名直传服务和上传回调服务两个功能。

-   签名直传服务

    签名直传服务响应客户端发送给应用服务器的GET消息，代码片段如下：

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

-   上传回调服务

    上传回调服务响应OSS发送给应用服务器的POST消息，代码片段如下：

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

    更多详情请参见API文档[Callback](/intl.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)。


