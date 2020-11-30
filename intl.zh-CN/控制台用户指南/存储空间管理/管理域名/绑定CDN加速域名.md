# 绑定CDN加速域名

对象存储OSS与阿里云CDN服务结合，可优化静态热点文件下载加速的场景（即同一地区大量用户同时下载同一个静态文件的场景）。您可以将OSS的存储空间（Bucket）作为源站，利用阿里云CDN将源内容发布到边缘节点。当大量终端用户重复访问同一文件时，可以直接从边缘节点获取已缓存的数据，提高访问的响应速度。

已绑定自定义域名。具体步骤请参见[绑定自定义域名](/intl.zh-CN/控制台用户指南/存储空间管理/管理域名/绑定自定义域名.md)。

要启用CDN加速服务，需要将您的用户域名指向阿里云CDN分配的CDN加速域名，这样访问用户域名的请求才能转发到CDN节点上，达到加速效果。

**说明：**

-   建议您在上传加速、非静态热点文件下载加速等场景中使用OSS传输加速功能。详情请参见[传输加速](/intl.zh-CN/开发指南/存储空间（Bucket）/传输加速.md)。

## 步骤1：绑定CDN加速域名

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**传输管理** \> **域名管理**。

4.  在域名列表中找到需要绑定阿里云CDN加速的域名，单击**阿里云CDN加速**列的**未配置**，系统会跳转至CDN控制台。

5.  在CDN控制台的添加域名页面，对下表中的参数进行配置。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3494459951/p32707.png)

    |参数|说明|
    |:-|:-|
    |**加速域名**|根据OSS控制台已绑定的域名自动填写，不要修改。|
    |**资源分组**|选择默认资源组。|
    |**业务类型**|不同的业务类型有不同的流量分配，按照您存储的内容及使用情况选择合适的业务类型。|
    |**源站信息**|选择您需要加速的OSS域名。|
    |**端口**|根据您的需求选择访问端口。|
    |**加速区域**|根据您的情况选择需要加速的区域。|

6.  单击**下一步**。

    成功添加加速域名后，会生成一个CNAME值，您还需要将这个CNAME值添加到域名解析内，CDN加速服务才会生效。


## 步骤2：添加CNAME记录

您需要在您的域名解析商处添加域名解析，这里以阿里云的域名添加域名解析为例，配置步骤如下：

1.  登录[CDN管理控制台](https://cdn.console.aliyun.com/overview)。

2.  单击**域名管理**，在域名列表中找到需要添加解析的域名，复制对应的CNAME值。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3494459951/p34242.png)

3.  登录[云解析DNS控制台](https://dns.console.aliyun.com/#/dns/domainList)。

4.  在域名解析列表中，单击目标域名右侧的**解析设置**。

5.  单击**添加记录**，填写域名解析信息。

    |参数|说明|
    |:-|:-|
    |**记录类型**|选择**CNAME**。|
    |**主机记录**|    -   如果是顶级域名，例如`aliyun.com`，输入**@**。
    -   如果是二级域名，输入二级域名的前缀。例如域名为`abc.aliyun.com`，输入**abc**。
    -   如果需要所有的二级域名都指向CDN加速域名，输入**\***。 |
    |**解析线路**|解析域名时使用的线路。建议选择**默认**，系统会自动选择最佳线路。|
    |**记录值**|填写[步骤2](#step_uf2_kam_nht)复制的CNAME值。|
    |**TTL**|域名的更新周期，保持默认即可。|

6.  单击**确定**。

    **说明：**

    -   新增CNAME记录实时生效，修改CNAME记录最多72小时生效。
    -   配置完CNAME后，由于状态更新约有10分钟延迟，阿里云CDN控制台的域名列表页可能仍提示**未配置CNAME**，请忽略。

## 步骤3：开启CDN缓存自动刷新

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**传输管理** \> **域名管理**。

4.  找到已绑定的域名，打开**CDN缓存自动刷新**的开关即可。

    CDN缓存自动刷新功能目前已支持针对指定操作触发，您可以[提交工单](https://workorder-intl.console.aliyun.com/#/ticket/createIndex)申请试用。试用申请通过之后，您可以单击指定域名右侧**支持的操作**，在下拉菜单选择自动刷新针对的操作，之后单击**确定**。

    支持的操作类型如下：

    |参数|说明|
    |--|--|
    |PutObject|通过PutObject接口上传文件。详情请参见[简单上传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/简单上传.md)。|
    |PostObject|通过PostObject接口上传文件。详情请参见[表单上传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/表单上传.md)。|
    |CopyObject|通过CopyObject接口修改文件。详情请参见[拷贝文件](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/拷贝文件.md)。|
    |AppendObject|通过AppendObject接口上传文件。详情请参见[追加上传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/追加上传.md)。|
    |CompleteMultipartUpload|通过分片上传或断点续传上传文件。详情清参见[分片上传和断点续传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/分片上传和断点续传.md)。|
    |DeleteObject|通过DeleteObject接口删除文件。详情请参见[删除文件](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/删除文件.md)。|
    |DeleteObjects|通过DeleteMultipleObjects接口删除文件。详情请参见[删除文件](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/删除文件.md)。|
    |PutObjectACL|通过PutObjectACL接口修改文件的权限控制。详情请参见[Object ACL](/intl.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。|

    由生命周期触发的对象过期（Expire）、类型转换（TransitionStorageClass）操作不再支持缓存刷新。


**说明：**

-   当您解除Bucket与用户域名之间的绑定关系后，OSS控制台将不支持CDN缓存自动刷新的操作，但您可以前往阿里云CDN控制台内进行配置。具体操作请参见[配置刷新和预热](/intl.zh-CN/服务管理/刷新预热/配置刷新和预热.md)。
-   CDN缓存自动刷新功能提交的刷新URL为`CNAME/object`，不会刷新带请求参数的URL（图片处理、视频截帧等）。例如Bucket绑定的加速域名为`test.aliyun.com`，Bucket中的a.jpg更新了，则访问`test.aliyun.com/a.jpg`时，可以获取最新文件；访问`test.aliyun.com/a.jpg?x-oss-process=image/w_100`时，可能访问到的还是旧文件。

## 常见问题：访问网站时报错AccessDenied

绑定用户域名后，您可以使用用户域名加上具体的资源路径来访问OSS上的资源，例如http://mydomain.cn/test/1.jpg。如果您直接访问用户域名，例如http://mydomain.cn，则会提示错误AccessDenied。

## 后续操作

-   如果您希望使用HTTPS的方式访问CDN加速域名，请进行证书托管。详情请参见[证书托管](/intl.zh-CN/控制台用户指南/存储空间管理/管理域名/证书托管.md)。
-   如果您开启了CDN加速，并且需要进行跨域CORS访问，您需要在CDN控制台配置跨域规则。详情请参见[CDN如何配置跨域资源共享（CORS）](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm)。

