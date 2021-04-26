# 基于CentOS的ECS实例实现OSS反向代理

阿里云OSS的存储空间（Bucket）访问地址会随机变换，您可以通过在ECS实例上配置OSS的反向代理，实现通过固定IP地址访问OSS的存储空间。

阿里云OSS通过Restful API方式对外提供服务。最终用户通过OSS默认域名或者绑定的自定义域名方式访问，但是在某些场景下，用户需要通过固定的IP地址访问OSS：

-   某些企业由于安全机制，需要在出口防火墙配置策略，以限制内部员工和业务系统只能访问指定的公网IP，但是OSS的Bucket访问IP会随机变换，导致需要经常修改防火墙策略。
-   金融云环境下，因金融云网络架构限制，金融云内网类型的Bucket只能在金融云内部访问，不支持在互联网上直接访问金融云内网类型Bucket。

以上问题可以通过在ECS实例上搭建反向代理的方式访问OSS。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1554449951/p38572.png)

## 配置步骤

1.  创建一个和对应Bucket相同地域的CentOS系统的ECS实例。

    本文演示系统为CentOS 7.6 64位系统。创建过程请参见[创建ECS实例]()。

2.  使用root用户登录ECS实例并安装Nginx。

    ```
    root@test:~# yum install -y nginx
    ```

    **说明：** Nginx默认安装位置：

    ```
     /usr/sbin/nginx       主程序 
     /etc/nginx            存放配置文件 
     /usr/share/nginx      存放静态文件 
     /var/log/nginx        存放日志
    ```

3.  打开Nginx配置文件。

    ```
    root@test:~# vi /etc/nginx/nginx.conf
    ```

4.  在config文件中的HTTP模块中，修改配置如下。

    ```
    server {
    listen 80 default_server;
    listen [::]:80 default_server;
    server_name 47.**.**.43; 
    root /usr/share/nginx/html;
    
    
    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;
    
    
    location / {
    proxy_pass https://bucketname.oss-cn-beijing-internal.aliyuncs.com; 
    proxy_set_header Head $host; 
    }
    ```

    -   server\_name：对外提供反向代理服务的IP，即ECS实例的外网地址。
    -   proxy\_pass：填写跳转的域名。
        -   当ECS实例与Bucket在同一地域时，填写目标Bucket的内网访问域名。访问域名介绍请参见[OSS访问域名使用规则](/intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md)。
        -   当ECS实例与Bucket不在同一地域时，填写目标Bucket的外网访问域名。
        -   因OSS的安全设置，当使用默认域名通过浏览器访问OSS中的图片或网页文件时，会直接下载。所以，若您的用户需通过浏览器预览Bucket中的图片或网页文件，需为Bucket绑定自定义域名，并在此项中添加已绑定的域名。绑定自定义域名操作请参见[绑定自定义域名](/intl.zh-CN/控制台用户指南/存储空间管理/管理域名/绑定自定义域名.md)。
    -   proxy\_set\_header Head $host：添加此项时，Nginx会在向OSS请求的时候，将host替换为ECS的访问地址。遇到以下情况时，您需要添加此项。
        -   遇到签名错误问题。
        -   如果您的域名已解析到ECS实例的外网上，且您的用户需要通过浏览器预览Bucket中的图片或网页文件。您可以将您的域名绑定到ECS实例代理的Bucket上，不配置CNAME。这种情况下，proxy\_pass项可直接配置Bucket的内网或外网访问地址。绑定自定义域名操作请参见[绑定自定义域名](/intl.zh-CN/控制台用户指南/存储空间管理/管理域名/绑定自定义域名.md)。
    **说明：** 本文为演示环境，实际环境中，为了您的数据安全，建议配置HTTPS模块，配置方法请参见[反向代理配置](https://www.alibabacloud.com/help/zh/faq-detail/39544.htm)。

5.  进入Nginx主程序文件夹，启动Nginx。

    ```
    root@test:~# cd /usr/sbin/
    root@test:~# ./nginx
    ```

6.  开放ECS实例的TCP 80端口。

    Nginx默认使用80端口，需在ECS的安全组配置中，允许用户访问TCP 80端口。配置方式请参见[添加安全组规则](/intl.zh-CN/安全/安全组/添加安全组规则.md)。

7.  测试使用ECS外网地址加文件访问路径访问OSS资源。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1554449951/p38588.png)

    如果访问的文件读写权限为私有，文件URL中还需要包含签名信息。详情请参见[在URL中包含签名](/intl.zh-CN/API 参考/访问控制/在URL中包含签名.md)。


## 更多参考

[基于Ubuntu的ECS实例实现OSS反向代理](/intl.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于Ubuntu的ECS实例实现OSS反向代理.md)

