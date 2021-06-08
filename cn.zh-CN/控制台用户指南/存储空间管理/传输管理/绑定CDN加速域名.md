# 绑定CDN加速域名

对象存储OSS与阿里云CDN服务结合，可将OSS内的文件缓存到CDN的边缘节点。当大量终端用户重复访问同一文件时，可以直接从边缘节点获取已缓存的数据，提高访问的响应速度。

若您的Bucket在中国内地，绑定的域名需在中国工信部[备案](https://beian.aliyun.com/order/selfBaIndex.htm)。

OSS结合CDN加速服务使用时，会产生CDN流出流量费用、CDN回源流出流量费用、请求费用。计费详情，请参见[OSS结合阿里云CDN场景的计费说明](/cn.zh-CN/计量计费/计费方式/特殊场景.md)。

**说明：** 建议您在上传加速、非静态热点文件下载加速等场景中使用OSS传输加速功能。更多信息，请参见[传输加速](/cn.zh-CN/开发指南/存储空间（Bucket）/传输加速.md)。

## 操作步骤

1.  绑定自定义域名。

    1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

    2.  单击**Bucket列表**，之后单击目标Bucket名称。

    3.  选择**传输管理** \> **域名管理**。

    4.  单击**绑定域名**，在绑定域名面板的**域名**文本框填写您的域名。

        请勿打开**自动添加CNAME**开关。

        若提示域名冲突，表示该域名已绑定至其他Bucket。此时，您可以更换域名或通过验证域名所有权强制绑定域名。验证域名所有权会解除域名与其他Bucket的绑定关系。更多信息，请参见[验证域名所有权](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。

    5.  单击**提交**。

2.  配置CDN加速服务。

    1.  在域名列表中，单击目标域名右侧的**未配置**。

    2.  在添加域名页面，配置各项参数。配置详情，请参见[添加加速域名](/cn.zh-CN/快速入门/添加加速域名.md)。

    3.  单击**下一步**，然后单击**返回域名列表**。

    4.  在域名列表中，记录目标域名的CNAME值。

3.  添加CNAME记录。

    若您的域名为非阿里云托管的域名，需在对应的域名解析商处配置云解析。如腾讯云解析（原DNSPod）或新网添加CNAME的步骤，请参见[DNSPod配置CNAME流程](https://help.aliyun.com/document_detail/27145.html)或[新网配置CNAME流程](https://help.aliyun.com/document_detail/27144.html#title-6nk-0q4-2w1)。此处以阿里云托管的域名为例，添加CNAME记录步骤如下：

    1.  登录[云解析DNS控制台](https://dns.console.aliyun.com/#/dns/domainList)。

    2.  单击**域名解析**，然后在域名解析列表中，单击目标域名右侧的**解析设置**。

    3.  单击**添加记录**，填写域名解析信息。

        |参数|说明|
        |:-|:-|
        |**记录类型**|选择域名指向的类型。 此处选择**CNAME**。|
        |**主机记录**|根据域名前缀填写主机记录。         -   如果是顶级域名，例如`aliyun.com`，输入**@**。
        -   如果是二级域名，输入二级域名的前缀。例如域名为`abc.aliyun.com`，输入**abc**。
        -   如果需要所有的二级域名都指向Bucket外网访问域名，输入**\***。 |
        |**解析线路**|解析域名时使用的线路。 建议选择**默认**，系统将自动选择最佳线路。|
        |**记录值**|填写[步骤2](#substep_gb2_ejz_v7n)中记录的CNAME值。|
        |**TTL**|域名的更新周期，保留默认值即可。|

    4.  单击**确定**。

        新增CNAME记录实时生效，修改CNAME记录最多72小时生效。

4.  开启CDN缓存自动刷新。

    在**域名管理**页签，打开目标域名右侧的**CDN缓存自动刷新**开关。

    如果您希望针对指定操作触发CDN缓存自动刷新，可以[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex)。开通后，您需要单击目标域名右侧**支持的操作**，然后选中指定操作类型。支持的操作类型如下：

    |参数|说明|
    |--|--|
    |PutObject|通过PutObject接口上传文件。更多信息，请参见[简单上传](/cn.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/简单上传.md)。|
    |PostObject|通过PostObject接口上传文件。更多信息，请参见[表单上传](/cn.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/表单上传.md)。|
    |CopyObject|通过CopyObject接口修改文件。更多信息，请参见[拷贝文件](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/拷贝文件.md)。|
    |AppendObject|通过AppendObject接口上传文件。更多信息，请参见[追加上传](/cn.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/追加上传.md)。|
    |CompleteMultipartUpload|通过分片上传或断点续传上传文件。更多信息，清参见[分片上传和断点续传](/cn.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/分片上传和断点续传.md)。|
    |DeleteObject|通过DeleteObject接口删除文件。更多信息，请参见[删除文件](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/删除文件.md)。|
    |DeleteObjects|通过DeleteMultipleObjects接口删除文件。更多信息，请参见[删除文件](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/删除文件.md)。|
    |PutObjectACL|通过PutObjectACL接口修改文件的权限控制。更多信息，请参见[Object ACL](/cn.zh-CN/开发指南/数据安全/访问控制/读写权限ACL.md)。|

    由生命周期触发的对象过期（Expire）、类型转换（TransitionStorageClass）操作不再支持CDN缓存刷新。使用CDN缓存自动刷新时有如下注意事项：

    -   CDN缓存自动刷新功能提交的刷新URL为`CNAME/ObjectName`，不会刷新带请求参数的URL（图片处理、视频截帧等）。例如Bucket绑定的加速域名为`example.com`，当您更新Bucket根目录的a.jpg文件，则访问`example/a.jpg`可以获取最新文件；访问`example.com/a.jpg?x-oss-process=image/w_100`可能获取的还是旧文件。
    -   CDN缓存自动刷新功能不保证一定能成功提交刷新任务，也不保证刷新任务提交的及时性。
    -   CDN缓存自动刷新功能仅支持少量文件的更新提交刷新任务。如果有大量文件的更新操作，可能会触发流控丢弃部分刷新任务。
    **说明：** 对页面或者有大量文件更新需求的用户，建议直接调用[RefreshObjectCaches](/cn.zh-CN/新版API参考/刷新预热类接口/刷新节点上的文件内容.md)刷新节点上的文件内容。


## 常见问题

-   为什么访问已绑定的CDN加速域名会出现AccessDenied错误？

    绑定域名后，您可以使用域名加上具体的资源路径来访问OSS上的资源，例如`http://example.com/test/1.jpg`。如果您直接访问该域名，例如`http://example.com`，则会出现AccessDenied错误。

-   已完成CDN加速服务的配置，为什么刷新OSS控制台的域名管理页签会出现CDN服务已配置和未配置交替出现的情况？

    CDN服务配置完成后约10分钟才会生效，所以您在域名管理页签查看CDN服务配置状态时会出现以上情况，建议您等待10分钟后刷新页面查看。


## 更多参考

-   如果您希望使用HTTPS的方式访问CDN加速域名，请进行证书托管。具体操作，请参见[证书托管](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/证书托管.md)。
-   如果您开启了CDN加速，并且需要进行跨域CORS访问，您需要在CDN控制台配置跨域规则。具体操作，请参见[CDN如何配置跨域资源共享（CORS）](https://help.aliyun.com/knowledge_detail/40183.html)。
-   有关OSS结合CDN加速服务的更多介绍，请参见[使用CDN加速OSS访问](https://help.aliyun.com/document_detail/173722.html)。

