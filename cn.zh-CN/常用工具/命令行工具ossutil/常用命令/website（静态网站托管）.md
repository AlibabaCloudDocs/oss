# website（静态网站托管）

website命令用于添加、修改、查询、删除存储空间（Bucket）的静态网站托管配置、重定向配置、镜像回源配置。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   静态网站托管功能介绍请参见[静态网站托管介绍](/cn.zh-CN/开发指南/静态网站托管/静态网站托管介绍.md)。
-   回源功能介绍请参见[管理回源设置](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/管理回源设置.md)。

## 命令格式

-   添加/修改website配置

    ```
    ./ossutil website --method put oss://bucket  local_xml_file
    ```

    ossutil从配置文件local\_xml\_file中读取website配置，并在Bucket中添加对应规则。若Bucket已有website配置，新的配置将覆盖原有配置。

    local\_xml\_file是一个xml格式的文件，其中：

    -   IndexDocument字段用来配置静态页面默认首页。
    -   ErrorDocument字段用来配置静态页面默认404页。
    -   RoutingRules字段用来配置重定向和镜像回源规则。各参数配置详情请参见[设置回源规则](/cn.zh-CN/控制台用户指南/存储空间管理/基础设置/回源规则/概述.md)。
    您可以选择部分规则进行配置，完整配置举例如下：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
     <WebsiteConfiguration>
         <IndexDocument>
             <Suffix>index.html</Suffix>
         </IndexDocument>
         <ErrorDocument>
             <Key>error.html</Key>
         </ErrorDocument>
         <RoutingRules>
             <RoutingRule>
                 <RuleNumber>1</RuleNumber>
                 <Condition>
                     <KeyPrefixEquals>abc/</KeyPrefixEquals>
                     <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
                 </Condition>
                 <Redirect>
                     <RedirectType>Mirror</RedirectType>
                     <PassQueryString>true</PassQueryString>
                     <MirrorURL>http://www.test.com/</MirrorURL>
                     <MirrorPassQueryString>true</MirrorPassQueryString>
                     <MirrorFollowRedirect>true</MirrorFollowRedirect>
                     <MirrorCheckMd5>false</MirrorCheckMd5>
                     <MirrorHeaders>
                       <PassAll>true</PassAll>
                       <Pass>myheader-key1</Pass>
                       <Pass>myheader-key2</Pass>
                       <Remove>myheader-key3</Remove>
                       <Remove>myheader-key4</Remove>
                       <Set>
                         <Key>myheader-key5</Key>
                         <Value>myheader-value5</Value>
                       </Set>
                     </MirrorHeaders>
                 </Redirect>
             </RoutingRule>
             <RoutingRule>
                 <RuleNumber>2</RuleNumber>
                 <Condition>
                   <KeyPrefixEquals>abc/</KeyPrefixEquals>
                   <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
                   <IncludeHeader>
                     <Key>host</Key>
                     <Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
                   </IncludeHeader>
                 </Condition>
                 <Redirect>
                   <RedirectType>AliCDN</RedirectType>
                   <Protocol>http</Protocol>
                   <HostName>www.test.com</HostName>
                   <PassQueryString>false</PassQueryString>
                   <ReplaceKeyWith>prefix/${key}.suffix</ReplaceKeyWith>
                   <HttpRedirectCode>301</HttpRedirectCode>
                 </Redirect>
             </RoutingRule>
         </RoutingRules>
     </WebsiteConfiguration>
    ```

-   获取website配置

    ```
    ./ossutil website --method get oss://bucket  [local_xml_file]
    ```

    local\_xml\_file为文件路径参数。若填写，则将website的配置保存为本地文件；若置空，则将website的配置输出到屏幕上。

-   删除website配置

    ```
    ./ossutil website --method delete oss://bucket
    ```


## 使用示例

-   添加静态网站托管的默认首页和默认404页配置

    ```
    ./ossutil website --method put oss://bucket1  /file/website.xml
    ```

    website.xml文件内容为：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
     <WebsiteConfiguration>
         <IndexDocument>
             <Suffix>test.html</Suffix>
         </IndexDocument>
         <ErrorDocument>
             <Key>errortest.html</Key>
         </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   添加镜像回源规则配置

    ```
    ./ossutil website --method put oss://bucket1  /file/website.xml
    ```

    website.xml文件内容为：

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <WebsiteConfiguration>
    <RoutingRules>
             <RoutingRule>
                 <RuleNumber>1</RuleNumber>
                 <Condition>
                     <KeyPrefixEquals>abc/</KeyPrefixEquals>
                     <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
                 </Condition>
                 <Redirect>
                     <RedirectType>Mirror</RedirectType>
                     <PassQueryString>true</PassQueryString>
                     <MirrorURL>http://www.aliyun.com/</MirrorURL>
                     <MirrorPassQueryString>true</MirrorPassQueryString>
                     <MirrorFollowRedirect>true</MirrorFollowRedirect>
                     <MirrorCheckMd5>false</MirrorCheckMd5>
                     <MirrorHeaders>
                       <PassAll>true</PassAll>
                       <Pass>myheader1</Pass>
                       <Pass>myheader2</Pass>
                       <Remove>myheader3</Remove>
                       <Remove>myheader4</Remove>
                       <Set>
                         <Key>myheader5</Key>
                         <Value>myheader5</Value>
                       </Set>
                     </MirrorHeaders>
                 </Redirect>
             </RoutingRule>
    <RoutingRules>
    </WebsiteConfiguration>
    ```

-   获取website配置

    ```
    ./ossutil website --method get oss://bucket1  /file/website.xml
    ```

-   删除website配置

    ```
    ./ossutil website --method delete oss://bucket1  
    ```


## 常用选项

您可以在使用website命令时附加如下选项：

|选项名称|描述|
|----|--|
|--method|表示http的请求类型。取值： -   put：添加或修改配置。
-   get：获取配置。
-   delete：删除配置。 |
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |
|--proxy-host|网络代理服务器的url地址，支持http、https、socks5。例如http://120.79.\*\*.\*\*:3128、 socks5://120.79.\*\*.\*\*:1080。|
|--proxy-user|网络代理服务器的用户名，默认为空。|
|--proxy-pwd|网络代理服务器的密码，默认为空。|

**说明：** 更多通用选项请参见[查看选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

