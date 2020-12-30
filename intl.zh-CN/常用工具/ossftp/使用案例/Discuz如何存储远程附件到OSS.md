# Discuz如何存储远程附件到OSS

网站远程附件功能是指通过FTP方式将用户上传的附件直接存储到远程的FTP服务器，目前Discuz论坛、PHPWind论坛、WordPress个人网站等都支持远程附件功能。本文介绍如何基于Discuz论坛存储远程附件。

-   已创建一个公共读权限的存储空间（Bucket）。

    配置步骤，请参见[创建存储空间](/intl.zh-CN/控制台用户指南/存储空间管理/创建存储空间.md)。

-   已搭建Discuz论坛。

## 配置步骤

本文测试使用了华东1（杭州）名为`test-hz-jh-002`的Bucket，Discuz版本为Discuz! X3.1。

1.  使用管理员账号登录Discuz站点。

2.  在管理界面单击**全局**，之后单击**上传设置**。

3.  单击**远程附件**，之后设置远程附件各参数。

    |配置项|配置方法|
    |---|----|
    |**启用远程附件**|选择**是**。|
    |**启用SSL连接**|选择**否**。|
    |**FTP服务器地址**|运行ossftp工具的地址，通常填写`127.0.0.1`。|
    |**FTP服务器端口**|默认为2048。|
    |**FTP帐号**|格式为`AccessKey ID/Bucket名称`。AccessKey ID获取方式，请参见[创建AccessKey]()。|
    |**FTP密码**|即AccessKey Secret。获取方式，请参见[创建AccessKey]()。|
    |**被动模式（PASV）连接**|选择**是**。|
    |**远程附件目录**|填写后，FTP服务将在OSS的指定路径创建远程附件的上传目录。本示例填写为`半角句号（.）`，表示在Bucket的根目录下创建上传目录。|
    |**远程访问URL**|填写Bucket的外网访问域名，格式为`https://BucketName.Endpoint`。本示例填写为`https://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com`。关于访问域名的详情，请参见[OSS访问域名使用规则](/intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md) 。|
    |**FTP传输超时时间**|设置为0，表示使用服务器默认超时时间。|

4.  单击**测试远程附件**确认配置是否正常。

5.  发帖验证配置是否成功。

    1.  发贴时上传图片附件。

    2.  在图片上右键单击，然后选择**在新标签页中打开链接**。

        如果图片的链接格式为`http(s)://BucketName.Endpoint/path/filename`，则表示附件已正常上传。例如本示例中，图片URL为`https://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/forum/201512/18/171012mzvkku2z3na2w2wa.jpg`


