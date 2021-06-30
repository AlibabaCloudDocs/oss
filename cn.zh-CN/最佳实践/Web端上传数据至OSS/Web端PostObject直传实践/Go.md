# Go

本文以Go语言为例，讲解在服务端通过Go代码完成签名，并且设置上传回调，然后通过表单直传数据到OSS。

-   应用服务器对应的域名可通过公网访问。
-   应用服务器已经安装Go 1.6以上版本（执行命令`go version`进行验证）。
-   PC端浏览器支持JavaScript。

## 步骤1：配置应用服务器

1.  下载[应用服务器源码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/181705/cn_zh/1598870585874/aliyun-oss-appserver-go-master.zip)（Go版本）到应用服务器的目录下。

2.  以Ubuntu 16.04为例，将源码放置到`/home/aliyun/aliyun-oss-appserver-go`目录下。

3.  进入该目录，打开源码文件`appserver.go`，修改以下代码片段：

    ```
    // 请填写您的AccessKeyId。
    var accessKeyId string = "<yourAccessKeyId>"
    
    // 请填写您的AccessKeySecret。
    var accessKeySecret string = "<yourAccessKeySecret>"
    
    // host的格式为bucketname.endpoint，请替换为您的真实信息。
    var host string = "https://bucket-name.oss-cn-hangzhou.aliyuncs.com'"
    
    // callbackUrl为上传回调服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    var callbackUrl string = "http://88.88.88.88:8888";
    
    // 上传文件时指定的前缀。
    var upload_dir string = "user-dir-prefix/"
    
    // 上传策略Policy的失效时间，单位为秒。
    var expire_time int64 = 30
    ```

    -   accessKeyId：设置您的AccessKeyId。
    -   accessKeySecret：设置您的AessKeySecret。
    -   host：格式为`https://bucketname.endpoint`，例如`https://bucket-name.oss-cn-hangzhou.aliyuncs.com`。关于Endpoint的介绍，请参见[Endpoint访问域名](/cn.zh-CN/开发指南/基本概念.md)。
    -   callbackUrl：设置上传回调URL，即回调服务器地址，用于处理应用服务器与OSS之间的通信。OSS会在文件上传完成后，把文件上传信息通过此回调URL发送给应用服务器。本例中修改为`var callbackUrl string="http://11.22.33.44:1234";`。
    -   dir：若要设置上传到OSS文件的前缀则需要配置此项，否则置空即可。

## 步骤2：配置客户端

1.  下载[客户端源码](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.15.76dd4c07uaPzqG&file=aliyun-oss-appserver-js-master.zip)到PC端的本地目录。

2.  将以文件解压，并打开upload.js文件，找到下面的代码语句：

    ```
    // serverUrl是用户获取签名和Policy等信息的应用服务器的URL，请将下面的IP和Port配置为您自己的真实信息。
    serverUrl ='http://88.88.88.88:8888'
    ```

3.  将severUrl修改为应用服务器的地址。本示例中修改为`serverUrl ='http://11.22.33.44:1234'`。


## 步骤3： 修改CORS

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证。因此需要为Bucket设置跨域规则以支持Post方法。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**权限管理** \> **跨域设置**，在**跨域设置**区域单击**设置**。

4.  单击**创建规则**，配置如下图所示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p12308.png)

    **说明：** 为了您的数据安全，实际使用时，**来源**建议填写实际允许访问的域名。更多配置信息请参见[设置跨域访问](/cn.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)。


## 步骤4：体验上传回调

1.  启动应用服务器。

    在`/home/aliyun/aliyun-oss-appserver-go`目录下，执行Go命令：go run appserver.go 11.22.33.44 1234。

    **说明：** 请将IP和端口改成您配置的应用服务器的IP和端口。

2.  启动客户端。

    1.  在PC端的客户端源码目录中，打开index.html文件。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3423029951/p12306.png)

        **说明：** index.html文件不保证兼容IE 10以下版本浏览器，若使用IE 10以下版本浏览器出现问题时，您需要自行调试。

    2.  单击**选择文件**，选择指定类型的文件之后，单击**开始上传**。上传成功后，显示回调服务器返回的内容。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3423029951/p12309.png)


## 应用服务器核心代码解析

应用服务器源码包含了签名直传服务和上传回调服务两个功能。

-   签名直传服务

    签名直传服务响应客户端发送给应用服务器的GET消息，代码片段如下。

    ```
    func handlerRequest(w http.ResponseWriter, r *http.Request) {   
            if (r.Method == "GET") {
                    response := get_policy_token()
                    w.Header().Set("Access-Control-Allow-Methods", "POST")
                    w.Header().Set("Access-Control-Allow-Origin", "*")
                    io.WriteString(w, response)
            }
    ```

-   上传回调服务

    上传回调服务响应OSS发送给应用服务器的POST消息，代码片段如下。

    ```
    if (r.Method == "POST") {
                    fmt.Println("\nHandle Post Request ... ")
    
                    // Get PublicKey bytes
                    bytePublicKey, err := getPublicKey(r)
                    if (err != nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get Authorization bytes : decode from Base64String
                    byteAuthorization, err := getAuthorization(r)
                    if (err != nil) {
                            responseFailed(w)
                            return
                    }
    
                    // Get MD5 bytes from Newly Constructed Authorization String. 
                    byteMD5, err := getMD5FromNewAuthString(r)
                    if (err != nil) {
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

    更多详情请参见API文档[Callback](/cn.zh-CN/API 参考/关于Object操作/基础操作/Callback.md)。


