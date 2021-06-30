# 基于Windows的ECS实例实现OSS反向代理

阿里云OSS的存储空间（Bucket）访问地址会随机变换，您可以通过在ECS实例上配置OSS的反向代理，实现通过固定IP地址访问OSS的存储空间。

阿里云OSS为用户提供OSS默认域名或者绑定的自定义域名方式访问，但是在某些场景下，用户需要通过固定的IP地址访问OSS：

-   某些企业由于安全机制，需要在出口防火墙配置策略，以限制内部员工和业务系统只能访问指定的公网IP，但是OSS的Bucket访问IP会随机变换，导致需要经常修改防火墙策略。
-   金融云环境下，因金融云网络架构限制，金融云的Bucket只能在金融云内部访问。

以上问题可以通过在ECS实例上搭建反向代理的方式访问OSS。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1554449951/p38572.png)

## 配置步骤

1.  创建一个和指定Bucket相同地域的Windows Server系统的ECS实例并登录。

    本文演示系统为Windows Server 2019数据中心版64位中文版系统，创建和登录过程请参见[Windows系统实例快速入门](/cn.zh-CN/快速入门/通过控制台使用ECS实例（详细版）/Windows系统实例快速入门.md)。

2.  下载Nginx并解压。

    本文以Nginx-1.19.2版本为例，下载地址请参见[Nginx](http://nginx.org/en/download.html)。

3.  修改配置文件。

    1.  进入conf文件夹，使用记事本打开nginx.conf文件。

    2.  修改配置文件内容。

        ```
        worker_processes  1;
        events {
            worker_connections  1024;
        }
        
        http {
            include       mime.types;
            default_type  application/octet-stream;
        
            keepalive_timeout  65;
            server {
                listen       80;
                server_name  47.**.**.43;
        
                error_page   500 502 503 504  /50x.html;
                location = /50x.html {
                    root   html;
                }
        
               location / {
                    proxy_pass  http://bucketname.oss-cn-hangzhou-internal.aliyuncs.com;
                    #proxy_set_header Host $host;
                }
           }
        }
        ```

        -   server\_name：对外提供反向代理服务的IP，即ECS实例的外网地址。
        -   proxy\_pass：填写跳转的域名。
            -   当ECS实例与Bucket在同一地域时，填写目标Bucket的内网访问域名。访问域名介绍请参见[OSS访问域名使用规则](/cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md)。
            -   当ECS实例与Bucket不在同一地域时，填写目标Bucket的外网访问域名。
            -   因OSS的安全设置，当使用默认域名通过浏览器访问OSS中的图片或网页文件时，会直接下载。所以，若您的用户需通过浏览器预览Bucket中的图片或网页文件，需为Bucket绑定自定义域名，并在此项中添加已绑定的域名。绑定自定义域名操作请参见[绑定自定义域名](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。
        -   proxy\_set\_header Host $host：添加此项时，Nginx会在向OSS请求的时候，将host替换为ECS的访问地址。遇到以下情况时，您需要添加此项。
            -   遇到签名错误问题。
            -   如果您的域名已解析到ECS实例的外网上，且您的用户需要通过浏览器预览Bucket中的图片或网页文件。您可以将您的域名绑定到ECS实例代理的Bucket上，不配置CNAME。这种情况下，proxy\_pass项可直接配置Bucket的内网或外网访问地址。绑定自定义域名操作请参见[绑定自定义域名](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。
        **说明：**

        -   本文为演示环境，实际环境中，为了您的数据安全，建议配置HTTPS模块。
        -   上述配置方式只能代理访问一个Bucket。
4.  返回Nginx主程序文件夹，双击nginx.exe启动Nginx。

5.  开放ECS实例的TCP 80端口。

    Nginx默认使用TCP 80端口，所以需在ECS的安全组配置中，允许用户访问TCP 80端口。配置方式请参见[添加安全组规则](/cn.zh-CN/安全/安全组/添加安全组规则.md)。

6.  使用ECS外网地址加文件访问路径访问OSS资源。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1554449951/p38588.png)


## 更多参考

-   [基于Ubuntu的ECS实例实现OSS反向代理](/cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于Ubuntu的ECS实例实现OSS反向代理.md)
-   [基于CentOS的ECS实例实现OSS反向代理](/cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于CentOS的ECS实例实现OSS反向代理.md)

