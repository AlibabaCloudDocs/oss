# 通过Bucket Policy授权用户访问指定资源

Bucket Policy是阿里云OSS推出的针对Bucket的授权策略，您可以通过Bucket Policy授权其他用户访问您指定的OSS资源。

-   Bucket拥有者可以在OSS控制台通过图形化和策略语法两种方式配置Bucket Policy。通过策略语法的方式配置Bucket Policy前，您需要先了解OSS Action、Resource以及Condition分类信息。详情请参见[RAM Policy概述](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/RAM Policy概述.md)。
-   您可以添加多条Bucket Policy，但所有Bucket Policy的大小不允许超过16 KB。

## 方式一：图形化配置Bucket Policy

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，然后单击目标Bucket名称。

3.  单击**文件管理**页签，然后单击**授权**。

    您也可以通过单击**权限管理** \> **Bucket授权策略** \> **设置**，添加授权策略。

4.  在图形设置页面，单击**新增授权**。

5.  在新增授权页面配置各项参数，然后单击**确定**。

    |配置项|说明|
    |---|--|
    |**授权资源**|授权整个Bucket或Bucket内的部分资源供其他用户访问。    -   **整个Bucket**：授权策略针对整个Bucket生效。
    -   **指定资源**：授权策略只针对指定的资源生效。您可以配置多条针对指定资源的授权策略。
        -   针对目录级别授权

授权访问目录下的所有子目录和文件时，需在目录结尾处加上星号（\*）。例如授权访问abc目录下的所有子目录和文件，则填写为abc/\*。

        -   针对指定文件授权

授权访问目录下的指定文件时，需填写不包含Bucket名称在内的文件的完整路径，例如授权访问abc目录下的myphoto.png文件，则填写为abc/myphoto.png。 |
    |**授权用户**|通过选择不同类型的账号将资源授权给不同用户进行访问。    -   **匿名账号（\*）**：如果您需要给所有用户授权访问指定资源，请选中此项。
    -   **子账号**：如果您需要给当前账号下的RAM用户授权访问指定资源，请选中此项，并从下拉菜单中选择目标RAM用户。若需要授权的RAM用户较多时，建议直接在搜索框输入RAM用户名称关键字进行模糊匹配。

**说明：** 您的账号必须是阿里云账号，或拥有此Bucket管理权限及RAM控制台ListUsers权限的RAM用户，否则无法查看当前账号的RAM用户列表。给RAM用户授予ListUsers权限的具体操作请参见[为RAM用户授权](/intl.zh-CN/用户管理/授权管理/为RAM用户授权.md)。

    -   **其他账号**：如果您需要给其他阿里云账号、RAM用户以及通过STS生成的临时用户授予访问权限，请选中此项。

        -   当您需要给其他阿里云账号或RAM用户授权时，请输入被授权账号的UID。
        -   当您需要给STS临时用户授权时，输入格式为`arn:sts::{RoleOwnerUid}:assumed-role/{RoleName}/{RoleSessionName}`。例如生成临时用户时使用的角色为testrole，角色拥有者的阿里云账号UID为12345，生成临时用户时指定的RoleSessionName为testsession。此时应填写`arn:sts::12345:assumed-role/testrole/testsession`。当您需要给所有临时用户授权时，请使用通配符星号（\*）。例如配置为`arn:sts::*:*/*/*`。生成临时授权用户的操作请参见[使用STS临时访问凭证访问OSS](/intl.zh-CN/开发指南/数据安全/使用STS临时访问凭证访问OSS.md)。
**说明：** 当被授权的用户是STS临时用户时，该账号无法通过OSS控制台访问授权资源，您可以通过命令行工具ossutil、OSS SDK、OSS API访问授权资源。 |
    |**授权操作**|您可以通过**简单设置**和**高级设置**两种方式进行授权操作。    -   简单设置

选中此项后，您可以结合实际场景按照如下说明配置相应的访问权限。将鼠标悬停在每一种访问权限右侧对应的![mark](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0984616261/p259026.jpg)，可获取各访问权限对应的Action列表。

        -   **只读**：对相关资源拥有查看、列举及下载权限。
        -   **读/写**：对相关资源有读和写权限。
        -   **完全控制**：对相关资源有读、写、删除等所有操作权限。
        -   **拒绝访问**：拒绝对相关资源的所有操作。
**说明：** 若针对某用户同时配置了多条Bucket Policy规则，则该用户所拥有的权限是所有Policy规则的叠加。当这些Bucket Policy中包含**拒绝访问**权限时，遵循**拒绝访问**权限优先原则。例如针对某用户第一次设置了**只读**权限，第二次设置了**读/写**权限，则该用户最终的权限为**读/写**。如果第三次设置了**拒绝访问**权限，则该用户最终的权限为**拒绝访问**。

    -   高级设置

选中此项后，您需要根据以下说明完成相关配置。

        -   **效力**：包含允许（Allow）和拒绝（Deny）两种授权效力。
        -   **操作**：支持配置所有OSS支持的Action。有关Action分类的更多信息，请参见[RAM Policy概述](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/RAM Policy概述.md)。 |
    |**条件**（可选）|您还可以在基础设置和高级设置模式下选中此项，用于限定只有满足条件的用户能够访问OSS资源。     -   **访问方式**：当您希望授权用户通过HTTPS的方式来访问OSS资源时，请选中**HTTPS**。当您希望授权用户通过HTTP的方式来访问OSS资源时，请选中**HTTP**。相比HTTP，HTTPS具有更高的安全性。
    -   **IP =**：设置IP等于某个IP地址或IP地址段。如有多个IP地址，各个IP地址之间用英文逗号（,）分隔。
    -   **IP ≠**：设置IP不等于某个IP地址或IP地址段。如有多个IP地址，各个IP地址之间用英文逗号（,）分隔 。 |

6.  单击**确定**。


## 方式二：通过策略语法配置Bucket Policy

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，然后单击目标Bucket名称。

3.  单击**文件管理**页签，然后单击**授权**。

4.  在策略语法页面，单击**编辑**。

    您可以根据实际使用场景，编辑不同的策略语法，用于实现更精细的权限管理。以下为资源拥有者（UID为`174649585760xxxx`）为不同授权场景配置的Bucket Policy示例。

    -   允许匿名用户列举存储空间examplebucket下所有文件的权限。

        ```
        {
            "Statement": [
                {
                    "Action": [
                        "oss:ListObjects",
                        "oss:ListObjectVersions"
        
                    ],
                    "Effect": "Allow",            
                    "Principal": [
                        "*"
                    ],            
                    "Resource": [
                        "acs:oss:*:174649585760xxxx:examplebucket"
                    ]
                },
        
            ],
            "Version": "1"
        }
        ```

    -   示例2：拒绝源IP地址不在`192.168.0.0/16`范围内的匿名用户对存储空间examplebucket执行任何操作。

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Effect": "Deny",
                    "Action": "oss:*",
                    
                    "Principal": [
                        "*"
                    ],            
                    "Resource": [
                        "acs:oss:*:174649585760xxxx:examplebucket"
                    ],
                    "Condition":{
                        "NotIpAddress": {
                            "acs:SourceIp": ["192.168.0.0/16"]
                        }
                    }
                }
            ]
        }
        ```

    -   示例3：允许指定的RAM用户（UID为`20214760404935xxxx`）拥有目标存储空间examplebucket下`hangzhou/2020`和`hangzhou/2015`目录的只读权限。

        ```
        {
            "Statement": [
                {
                    "Action": [
                        "oss:GetObject",
                        "oss:GetObjectAcl",
                        "oss:GetObjectVersion",
                        "oss:GetObjectVersionAcl"
        
                    ],
                    "Effect": "Allow",             
                    "Principal": [
                        "20214760404935xxxx"
                    ],            
                    "Resource": [
                        "acs:oss:*:174649585760xxxx:examplebucket/hangzhou/2020/*",
                        "acs:oss:*:174649585760xxxx:examplebucket/hangzhou/2015/*"
                    ]
                },
                {
                    "Action": [
                        "oss:ListObjects",
                        "oss:ListObjectVersions"
                    ],
                    "Condition": {
                        "StringLike": {
                            "oss:Prefix": [
                                "hangzhou/2020/*",
                                "hangzhou/2015/*"
                            ]
                        }
                    },
                    "Effect": "Allow",
                    "Principal": [
                        "20214760404935xxxx"
                    ],
                    "Resource": [
                        "acs:oss:*:174649585760xxxx:examplebucket"
                    ]
                }
            ],
            "Version": "1"
        }
        ```

5.  单击**保存**。


## 访问授权资源

Bucket Policy配置完成后，您可以通过以下方式访问授权资源：

-   文件URL（仅当授权对象为匿名用户时）

    在浏览器上，使用Bucket默认域名或自有域名加文件路径进行访问。例如`http://mybucket.oss-cn-beijing.aliyuncs.com/file/myphoto.png`。详情请参见[OSS访问域名使用规则](/intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md)。

-   控制台

    登录OSS控制台，在左边菜单栏单击**我的访问路径**后的加号（+），添加授权访问的Bucket和文件路径。具体操作，请参见[设置我的访问路径](/intl.zh-CN/控制台用户指南/OSS管理控制台/设置我的访问路径.md)。

-   命令行工具ossutil

    使用被授权的账号通过ossutil访问授权资源。具体操作，请参见[ossutil](/intl.zh-CN/常用工具/命令行工具ossutil/概述.md)。

-   图形化工具ossbrowser

    使用被授权的账号登录ossbrowser，登录时在**预设OSS路径**栏输入被授权访问的文件目录。具体操作，请参见[ossbrowser](/intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速使用ossbrowser.md)。

-   OSS SDK

    支持通过[Java](/intl.zh-CN/SDK 示例/Java/授权访问.md)、[PHP](/intl.zh-CN/SDK 示例/PHP/授权访问.md)、[Node.js](/intl.zh-CN/SDK 示例/Node.js/授权访问.md)、[Python](/intl.zh-CN/SDK 示例/Python/授权访问.md)、[Browser.js](/intl.zh-CN/SDK 示例/Browser.js/授权访问.md)、[.NET](/intl.zh-CN/SDK 示例/.NET/授权访问.md)、[Android](/intl.zh-CN/SDK 示例/Android/授权访问.md)、[Go](/intl.zh-CN/SDK 示例/Go/授权访问.md)、[iOS](/intl.zh-CN/SDK 示例/iOS/授权访问.md)、[C++](/intl.zh-CN/SDK 示例/C++/授权访问.md)、[C](/intl.zh-CN/SDK 示例/C/授权访问.md) SDK访问授权资源。


