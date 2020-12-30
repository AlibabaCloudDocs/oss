# Phpwind如何存储远程附件到OSS

本文介绍如何基于Phpwind论坛存储远程附件。

-   已开通OSS服务，并创建了一个公共读权限的存储空间（Bucket）。
    -   开通OSS服务请参见[开通OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)。
    -   创建Bucket的步骤请参见[创建存储空间](/cn.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。
-   已搭建phpwind论坛。

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

本文档测试所用phpwind版本为phpwind8.7。

## 配置步骤

1.  使用管理员账号登录phpwind站点。

2.  在管理界面单击**全局** \> **附件设置** \> **附件设置**。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7224459951/p2813.png)

3.  单击**FTP设置**，设置FTP选项。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8224459951/p2815.png)

    |配置项|说明|
    |---|--|
    |**使用FTP上传**|选择**开启**。|
    |**站点附件地址**|填写Bucket的外网访问域名，格式为http://BucketName.Endpoint。测试所用Bucket名为test-hz-jh-002，属于杭州地域。所以这里填写的是http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com。关于访问域名的详情请参见[OSS访问域名使用规则](/cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md) 。|
    |**FTP服务器地址**|即运行ossftp工具的地址，通常填写127.0.0.1即可。|
    |**FTP服务器端口**|默认为2048。|
    |**FTP上传目录**|填半角句号（.）即可，表示在Bucket的根目录开始创建附件目录。|
    |**FTP帐号**|格式为AccessKeyID/BukcetName。注意这里的正斜线（/）不是或的意思。|
    |**FTP密码**|即AccessKeySecret。|
    |**超时时间\[秒\]**|设置为10，如果10秒内没有返回请求结果，表示系统将会超时返回。|

4.  发帖验证配置是否成功。

    1.  发贴时上传图片附件。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8224459951/p2817.png)

    2.  在图片上右键单击，之后单击**在新标签页中打开链接**。

        通过图中的URL，我们可以判断图片已经上传到OSS的test-hz-jh-002 Bucket。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8224459951/p2818.png)


