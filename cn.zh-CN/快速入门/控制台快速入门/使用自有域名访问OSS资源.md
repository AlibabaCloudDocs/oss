# 使用自有域名访问OSS资源

文件（Object）上传至存储空间（Bucket）后，OSS会自动生成文件URL，您可以直接通过文件URL（即Bucket外网访问域名）访问该文件。若您希望通过自定义域名（自有域名）访问这些文件，需要将自定义域名绑定至文件所在的Bucket。

已完成域名备案，详情请参见[备案](https://beian.aliyun.com/order/selfBaIndex.htm)。

1.  绑定自定义域名。

    1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

    2.  单击**Bucket列表**，之后单击目标Bucket名称。

    3.  单击**传输管理** \> **域名管理**。

    4.  单击**绑定域名**。

    5.  在绑定域名面板，输入要绑定的**域名**。

        若提示域名冲突，表示该域名已绑定至其他Bucket。此时，您可以更换域名或通过验证域名所有权强制绑定域名。验证域名所有权会解除域名与其他Bucket的绑定关系。详情请参见[验证域名所有权](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。

2.  添加CNAME记录。

    -   如果添加的域名为当前账号下管理的域名，开启自动添加CNAME记录。
        1.  在绑定域名面板，打开**自动添加CNAME记录**开关。

            **说明：** 若您绑定的域名已配置过CNAME，则自动添加的CNAME记录会覆盖原有的CNAME记录。

        2.  单击**提交**。
    -   如果添加的域名为非当前账号下的域名，手动添加CNAME记录。

        若您的域名为非阿里云托管的域名，需在对应的域名解析商处配置云解析，如腾讯云解析（原DNSPod）或新网，详情请参见 [DNSPod配置CNAME流程](https://help.aliyun.com/document_detail/27145.html)或[新网配置CNAME流程](https://help.aliyun.com/document_detail/27144.html#title-6nk-0q4-2w1)。

        此处以非当前账号下阿里云托管的域名为例，手动添加CNAME记录步骤如下：

        1.  登录[云解析DNS控制台](https://dns.console.aliyun.com/#/dns/domainList)。
        2.  在域名解析列表中，单击目标域名右侧的**解析设置**。
        3.  单击**添加记录**，填写域名解析信息。

            |参数|说明|
            |:-|:-|
            |**记录类型**|选择域名指向的类型。 此处选择**CNAME**。|
            |**主机记录**|根据域名前缀填写主机记录。             -   如果是顶级域名，例如`aliyun.com`，输入**@**。
            -   如果是二级域名，输入二级域名的前缀。例如域名为`abc.aliyun.com`，输入**abc**。
            -   如果需要所有的二级域名都指向Bucket外网访问域名，输入**\***。 |
            |**解析线路**|解析域名时使用的线路。 建议选择**默认**，系统将自动选择最佳线路。|
            |**记录值**|填写Bucket外网访问域名。Bucket外网访问域名结构为`BucketName.Endpoint`，例如华东1（杭州）地域创建了名为examplebucket的存储空间，外网Endpoint为`oss-cn-hangzhou.aliyuncs.com`，则填写为`examplebucket.oss-cn-hangzhou.aliyuncs.com`。|
            |**TTL**|域名的更新周期，保留默认值即可。|

        4.  单击**确定**。

            新增CNAME记录实时生效，修改CNAME记录最多72小时内生效。

3.  通过自有域名访问OSS资源。

    绑定自定义域名后，文件URL的格式为`https://YourDomainName/ObjectName`。

    例如您在华东1（杭州）地域下创建了目标存储空间examplebucket，并在examplebucket中存放了exampleobject.jpg的文件，自定义域名为`img.example.com`，此时您可以使用`https://img.example.com/exampleobject.jpg`访问目标文件。


