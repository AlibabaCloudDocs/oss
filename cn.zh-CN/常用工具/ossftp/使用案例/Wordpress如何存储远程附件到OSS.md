# Wordpress如何存储远程附件到OSS

本文介绍如何基于Wordpress个人网站存储远程附件。

-   已开通OSS服务，并创建了一个公共读权限的存储空间（Bucket）。
    -   开通OSS服务请参见[开通OSS服务](/cn.zh-CN/控制台用户指南/开通OSS服务.md)。
    -   创建Bucket的步骤请参见[创建存储空间](/cn.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。
-   已搭建Wordpress个人网站。

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

wordpress本身不支持远程附件功能，但是可以通过第三方的插件来做远程附件。本文档示例中所用wordpress版本为4.3.1，所用插件为Hacklog Remote Attachment。

## 配置步骤

1.  使用管理员账号登录wordpress站点。

2.  单击**插件**，之后在**关键词**栏输入FTP并按回车键。

3.  找到Hacklog Remote Attachment，单击**现在安装**。

4.  插件安装完成后单击**设置** \> **Hacklog远程附件**。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9224459951/p60862.png)

5.  在弹出的**Hacklog远程附件选项**对话框设置FTP服务信息。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9224459951/p60863.png)

    |配置项|说明|
    |---|--|
    |**Ftp服务器**|即运行ossftp工具的地址，通常填写127.0.0.1即可。|
    |**Ftp服务器端口**|默认为2048。|
    |**Ftp用户名**|格式为AccessKeyID/BukcetName。注意这里的正斜线（/）不是或的意思。|
    |**Ftp密码**|即AccessKeySecret。|
    |**FTP超时**|默认设置为30秒即可。|
    |**远程基本URL**|填写Bucket的外网访问域名，格式为http://BucketName.Endpoint。测试所用Bucket名为test-hz-jh-002，属于杭州地域。所以这里填写的是http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/wp。关于访问域名的详情请参见[OSS访问域名使用规则](/cn.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md) 。|
    |**FTP远程路径**|设置附件在Bucket的存储路径。示例中填写wp表示所有附件都会存储在Bucket的wp目录下。**远程基本URL**须与**FTP远程路径**对应。|
    |**HTTP远程路径**|填半角句号（.）即可。|

6.  单击**保存**。

    单击**保存**的同时会测试配置，测试结果会在页面上方显示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9224459951/p60864.png)

7.  发布新文章验证配置是否成功。

    1.  撰写新文章时单击**添加媒体**来上传附件。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0324459951/p60866.png)

        上传附件如下图所示。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0324459951/p2826.png)

    2.  单击**发布**，即可看到刚撰写的文章。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0324459951/p2827.png)

    3.  在图片上右键单击，选择**在新标签页中打开链接**。

        通过图中的URL，我们可以判断图片已经上传到OSS的test-hz-jh-002 Bucket。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0324459951/p2829.png)


