# 安装并登录ossbrowser

ossbrowser是阿里云官方提供的OSS图形化管理工具，提供类似Windows资源管理器的功能。本文介绍如何快速安装并登录ossbrowser。

1.  下载并安装ossbrowser。

    |当前版本|下载地址|说明|
    |----|----|--|
    |1.15.0|[Windows x32](https://gosspublic.alicdn.com/oss-browser/1.15.0/oss-browser-win32-ia32.zip)|支持Windows 7及以上版本。|
    |[Windows x64](https://gosspublic.alicdn.com/oss-browser/1.15.0/oss-browser-win32-x64.zip)|
    |[macOS](https://gosspublic.alicdn.com/oss-browser/1.15.0/oss-browser-darwin-x64.zip)|macOS系统默认无法打开未验证的开发者应用。如果出现该问题，请参见[macOS系统无法打开ossbrowser](/cn.zh-CN/常用工具/图形化管理工具ossbrowser/常见问题.md)修改系统安全设置。|
    |[Linux x64](https://gosspublic.alicdn.com/oss-browser/1.15.0/oss-browser-linux-x64.zip)（不推荐）|如果在Linux系统安装ossbrowser遇到问题时，请自行下载[源码](https://github.com/aliyun/oss-browser)编译排查。|

2.  以Windows系统为例，按如下步骤登录ossbrowser。

    1.  双击打开`oss-browser.exe`。

    2.  选择以下任意一种方式登录ossbrowser。

        -   通过AK登录

            您可以通过阿里云账号或RAM用户的AccessKey（AK）信息登录ossbrowser。

            ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6227056161/p40359.png)

            通过AK登录时，需按如下说明完成各配置项。

            |参数|说明|
            |--|--|
            |**Endpoint**|选择登录的访问域名。             -   **默认（公共云）**：使用当前Bucket所在地域对应的Endpoint登录。

选择此种登录方式时，可选中**HTTPS加密**对传输过程加密。

            -   **自定义**：使用其他地域对应的Endpoint登录，例如`https://oss-cn-beijing.aliyuncs.com`。有关Region与Endpoint的对应关系，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。
            -   **cname**：如果您希望通过自有域名访问OSS资源，请先绑定自有域名。具体操作，请参见[绑定自定义域名](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。然后选择此项，填写绑定的自有域名。 |
            |**AccessKeyId**、**AccessKeySecret**|填写账号的AccessKey（AK）信息。获取AK的方式，请参见[创建AccessKey]()。**说明：** 为保证数据安全，推荐您使用RAM用户的AK登录ossbrowser。使用RAM用户登录之前，需要为RAM用户配置`AliyunOSSFullAccess`、`AliyunRAMFullAccess`以及`AliyunSTSAssumeRoleAccess`的权限。具体操作请参见[权限管理](/cn.zh-CN/常用工具/图形化管理工具ossbrowser/权限管理.md)。 |
            |**预设OSS路径**|如果当前账号仅拥有某个Bucket或Bucket下某个路径的权限，需填写预设OSS路径。预设OSS路径格式为oss://bucketname/path。例如授权访问存储空间examplebucket下文件夹examplefolder下的文件或子文件夹，则填写oss://examplebucket/examplefolder/。

当授权访问的Bucket开启了请求者付费模式，且您不是该Bucket拥有者的情况下，需选中**请求者付费模式**。否则，访问预设OSS路径下的指定资源时会报错`AccessDenied`。选中**请求者付费模式**后可正常访问预设OSS路径下的指定资源，且访问该Bucket产生的流量、请求次数等费用将由您支付。有关请求者付费模式的更多信息，请参见[请求者付费模式](/cn.zh-CN/开发指南/存储空间（Bucket）/请求者付费模式.md)。 |
            |**区域**|当Endpoint配置为**默认（公共云）**时，需填写预设OSS路径对应Bucket所在的**区域**。|
            |**保持登录**|选中后，ossbrowser会保持登录状态，下次打开时将自动登录。|
            |**记住密钥**|选中可保存AK密钥。再次登录时，单击**AK历史**，可选择指定密钥直接登录。警告：

为避免不必要的信息安全风险，请勿在临时使用的电脑上选中该项。 |

        -   通过授权码登录

            如果您想要授权其他人临时访问您Bucket下的指定资源，请选择授权码登录。授权码到期后会自动失效。使用授权码登录的具体步骤，请参见[使用临时授权码登录](/cn.zh-CN/常用工具/图形化管理工具ossbrowser/权限管理.md)。


