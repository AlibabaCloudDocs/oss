# PutBucketWebsite

PutBucketWebsite接口用于将存储空间（Bucket）设置成静态网站托管模式并设置跳转规则（RoutingRule）。

## 注意事项

静态网站是指所有的网页都由静态内容构成，包括客户端执行的脚本，例如JavaScript。OSS不支持需要服务器端处理的内容，例如PHP、JSP、ASP.NET等。

-   功能支持：

    此接口主要用于设置默认主页、默认404页和RoutingRule。RoutingRule用来指定3xx跳转规则以及镜像回源规则。其中，镜像回源支持公共云和金融云。

-   使用自有域名访问静态网站

    如果您想使用自有域名来访问基于Bucket的静态网站，可以通过域名CNAME来实现。具体配置方法请参见[绑定自定义域名](/intl.zh-CN/开发指南/存储空间（Bucket）/绑定自定义域名.md)。

-   索引页面和错误页面

    将一个Bucket设置成静态网站托管模式时，若指定了索引页面或错误页面，则指定的索引页面和错误页面是该Bucket内的某个Object。

-   对静态网站根域名的匿名访问

    将一个Bucket设置成静态网站托管模式后，对静态网站根域名的匿名访问，OSS将返回索引页面。对静态网站根域名的签名访问，OSS将返回GetBucket \(ListObjects\)的结果。


## 请求语法

```
PUT /?website HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>errorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

## 请求参数

-   WebsiteConfiguration的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |WebsiteConfiguration|容器|是|根节点 父节点：无 |

-   IndexDocument的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |IndexDocument|容器|有条件至少指定IndexDocument、ErrorDocument、RoutingRules三个容器中的一个。

|默认主页的容器 父节点：WebsiteConfiguration |
    |Suffix|字符串|有条件若指定了父节点IndexDocument，则必须指定此项。

|默认主页 如果设置，则访问以正斜线（/）结尾的Object时都会返回此默认主页。

父节点：IndexDocument |
    |SupportSubDir|字符串|否|表示是否支持访问子目录时转至子目录下的默认主页。 取值：

    -   true：表示转至子目录下的默认主页。
    -   false（默认值）：表示不转至子目录下的默认主页，而是转至根目录下的默认主页。
假设访问`bucket.oss-cn-hangzhou.aliyuncs.com/subdir/`时，默认主页设置为index.html，则false表示转到`bucket.oss-cn-hangzhou.aliyuncs.com/index.html`，true表示转到`bucket.oss-cn-hangzhou.aliyuncs.com/subdir/index.html`。

父节点：IndexDocument |
    |Type|枚举值|否|表示设置了默认主页后，访问以非正斜线（/）结尾的Object，且该Object不存在时的行为。 **说明：** 只有在SupportSubDir为true时生效，且生效的顺序在RoutingRule之后、ErrorFile之前。

假设默认主页设置为index.html，访问的文件路径是bucket.oss-cn-hangzhou.aliyuncs.com/abc，且abc这个Object不存在，此时type三种取值类型对应的行为如下：

    -   0（默认值）：检查abc/index.html是否存在（即`Object + 正斜线（/）+ 主页`的形式），如果存在则返回302，Location头为`/abc/`的URL编码（即`正斜线（/） + Object + 正斜线（/）`的形式），如果不存在则返回404，继续检查ErrorFile。
    -   1：直接返回404，报错NoSuchKey，继续检查ErrorFile。
    -   2：检查abc/index.html是否存在，如果存在则返回该Object的内容；如果不存在则返回404，继续检查ErrorFile。
父节点：IndexDocument |

-   ErrorDocument的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |ErrorDocument|容器|有条件至少指定IndexDocument、ErrorDocument、RoutingRules三个容器中的一个。

|404页面的容器 父节点：WebsiteConfiguration |
    |Key|容器|有条件若指定了父节点ErrorDocument，则必须指定此项。

|404页面如果指定，则访问的Object不存在时则返回此404页面。

父节点：ErrorDocument |

-   RoutingRules\|RoutingRule\|RuleNumber的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |RoutingRules|容器|有条件至少指定IndexDocument、ErrorDocument、RoutingRules三个容器中的一个。

|RoutingRule的容器 父节点：WebsiteConfiguration |
    |RoutingRule|容器|否|指定跳转规则或者镜像回源规则，最多指定20个RoutingRule。 父节点：RoutingRules |
    |RuleNumber|正整数|有条件若指定了父节点RoutingRule，则必须指定此项。

|匹配和执行RoutingRule的序号，OSS将按照此序号依次匹配规则。如果匹配成功，则执行此规则，后续的规则不再执行。 父节点：RoutingRule |

-   RoutingRules\|RoutingRule\|Condition的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |Condition|容器|有条件若指定了父节点RoutingRule，则必须指定此项。

|匹配的条件 如果指定的项都满足，则执行此规则。满足此容器下的各个节点的所有条件才算匹配。

父节点：RoutingRule |
    |KeyPrefixEquals|字符串|否|只有匹配此前缀的Object才能匹配此规则。 父节点：Condition |
    |HttpErrorCodeReturnedEquals|HTTP状态码|否|访问指定Object时返回此status才能匹配此规则。当跳转规则是镜像回源类型时，此字段必须为404。 父节点：Condition |
    |IncludeHeader|容器|否|只有请求中包含了指定Header，且值为指定值时，才能匹配此规则。该容器最多可指定10个。 父节点：Condition |
    |Key|字符串|是|只有请求中包含了此Header，且值为Equals的指定值时，才能匹配此规则。 父节点：IncludeHeader |
    |Equals|字符串|否|只有请求中包含了Key指定的Header，且值为指定值时，才能匹配此规则。 父节点：IncludeHeader |
    |KeySuffixEquals|字符串|否|只有匹配此字段指定的后缀才能匹配此规则。 默认值为空，表示不匹配指定的后缀

父节点：Condition |

-   RoutingRules\|RoutingRule\|Redirect的内容

    |名称|类型|是否必选|描述|
    |--|--|----|--|
    |Redirect|容器|有条件若指定了父节点RoutingRule，则必须指定此项。

|指定匹配此规则后执行的动作。 父节点：RoutingRule |
    |RedirectType|字符串|有条件若指定了父节点Redirect，则必须指定此项。

|指定跳转的类型。取值：

    -   Mirror：镜像回源
    -   External：外部跳转，即OSS会返回一个3xx请求，指定跳转到另外一个地址。
    -   AliCDN：阿里云CDN跳转，主要用于阿里云的CDN。与External不同的是，OSS会额外添加一个Header。阿里云CDN识别到此Header后会主动跳转到指定的地址，返回给用户获取到的数据，而不是将3xx跳转请求返回给用户。
父节点：Redirect |
    |PassQueryString|布尔|否|执行跳转或者镜像回源规则时，是否携带请求参数。 用户请求OSS时携带了请求参数`?a=b&c=d`，并且此项设置为true，当规则为302跳转，则跳转的Location头中会添加此请求参数。例如`Location:www.test.com?a=b&c=d`，跳转类型是镜像回源，则在发起的回源请求中也会携带此请求参数。

取值：true、false

默认值：false

父节点：Redirect |
    |MirrorURL|字符串|有条件若RedirectType指定为Mirror，则必须指定此项。

|仅在RedirectType为Mirror时生效，表示镜像回源的源站地址。 假设以http://或者https://开头，必须以正斜线（/）结尾，OSS会在此字符串的基础上加上Object名称构成回源URL。

例如指定为`http://www.test.com/`，访问的Object名称是myobject，则回源的URL为`http://www.test.com/myobject`，如果指定为`http://www.test.com/dir1/`，那么回源的URL为`http://www.test.com/dir1/myobject`。

父节点：Redirect |
    |MirrorPassQueryString|布尔|否|与PassQueryString作用相同，优先级高于PassQueryString。仅在RedirectType为Mirror时生效。

默认值：false

父节点：Redirect |
    |MirrorFollowRedirect|布尔|否|如果镜像回源获取的结果是3xx，是否要继续跳转到指定的Location获取数据。 例如发起镜像回源请求时，如果源站返回了302，并且指定了Location。

取值如下：

    -   true（默认值）：OSS会继续请求Location指定的地址（最多跳转10次，如果超过10次，则无法返回镜像回源请求）。
    -   false：OSS返回302，并透传Location透传。只有在RedirectType为Mirror时生效。
父节点：Redirect |
    |MirrorCheckMd5|布尔|否|是否检查回源body的MD5。 当此项为true且源站返回的response中含有Content-Md5头时，OSS检查拉取的数据MD5是否与此Header匹配，如果不匹配，则不保存在OSS上。仅在RedirectType为Mirror时生效。

默认值：false

 父节点：Redirect|
    |MirrorHeaders|容器|否|用于指定镜像回源时携带的Header。只有在RedirectType为Mirror时生效。 父节点：Redirect |
    |PassAll|布尔|否|是否透传除以下Header之外的其他Header到源站。    -   content-length、authorization2、authorization、range、date等Header
    -   以oss-/x-oss-/x-drs-开头的Header
只有在RedirectType为Mirror时生效。

默认值：false

父节点：MirrorHeaders |
    |Pass|字符串|否|透传指定的Header到源站。每个Header长度最多1024个字节，字符集为0~9、A~Z、a~z以及短划线（-）。只有在RedirectType为Mirror时生效。

此字段最多可指定10个。

父节点：MirrorHeaders |
    |Remove|字符串|否|禁止透传指定的Header到源站。每个Header长度最多1024个字节，字符集与Pass相同。只有在RedirectType为Mirror时生效。

此字段最多可指定10个，通常与PassAll一起使用。

父节点：MirrorHeaders |
    |Set|容器|否|设置一个Header传到源站，不管请求中是否携带这些指定的Header，回源时都会设置这些Header。此容器最多可指定10组，仅在RedirectType为Mirror时生效。

父节点：MirrorHeaders |
    |Key|字符串|有条件若指定了父节点Set，则必须指定此项。

|设置Header的key，最多1024个字节，字符集与Pass相同。仅在RedirectType为Mirror时生效。

父节点：Set |
    |Value|字符串|有条件若指定了父节点Set，则必须指定此项。

|设置Header的value，最多1024个字节，不能出现`\r\n`。仅在RedirectType为Mirror时生效。

父节点：Set |
    |Protocol|字符串|否|跳转时的协议。假设访问的文件为test，设定跳转到`www.test.com`，而且Protocol字段为https，那么Location头为`https://www.test.com/test`。

仅在RedirectType为External或者AliCDN时生效。

取值：http或https。

父节点：Redirect |
    |HostName|字符串|否|跳转时的域名，需符合域名规范。例如访问的Object为test，Protocol为https，Hostname指定为`www.test.com`，那么Location头为`https://www.test.com/test`。

父节点：Redirect |
    |ReplaceKeyPrefixWith|字符串|否|Redirect时Object名称的前缀将替换成该值。如果前缀为空，则将这个字符串插入Object 名称的前面。 **说明：** 仅允许存在ReplaceKeyWith或ReplaceKeyPrefixWith节点。

假设指定了KeyPrefixEquals为abc/，指定了ReplaceKeyPrefixWith为def/，那么如果访问的Object为abc/test.txt，Location头则为`http://www.test.com/def/test.txt`。

父节点：Redirect |
    |EnableReplacePrefix|布尔|否|如果此字段设置为true，则Object的前缀将被替换为ReplaceKeyPrefixWith指定的值。如果未指定此字段或为空，则表示截断Object前缀。**说明：** ReplaceKeyWith字段不为空时，此字段不能设置为true。

默认值：false

父节点：Redirect |
    |ReplaceKeyWith|字符串|否|Redirect时Object名称将替换成ReplaceKeyWith指定的值，ReplaceKeyWith支持设置变量。目前支持的变量是$\{key\}，表示该请求中的Object名称。 假设设置ReplaceKeyWith为`prefix/${key}.suffix`，访问的Object为test，那么Location头则为`http://www.test.com/prefix/test.suffix`。

父节点：Redirect |
    |HttpRedirectCode|HTTP状态码|否|跳转时返回的状态码。仅在RedirectType为External或者AliCDN时生效。

取值：301（默认值）、302或307。

父节点：Redirect |


有关此接口的其他公共请求元素，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

有关此接口的响应元素，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
    PUT /?website HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 209
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS nxj7dtwhcyl5hp****:sNKIHT6ci/z231yIT5vYnetD****
    
    <?xml version="1.0" encoding="UTF-8"?>
    <WebsiteConfiguration>
      <IndexDocument>
        <Suffix>index.html</Suffix>
          <SupportSubDir>true</SupportSubDir>
          <Type>0</Type>
      </IndexDocument>
      <ErrorDocument>
        <Key>error.html</Key>
      </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   完整示例

    ```
    PUT /?website HTTP/1.1
    Date: Fri, 27 Jul 2018 09:03:18 GMT
    Content-Length: 2064
    Host: test.oss-cn-hangzhou-internal.aliyuncs.com
    Authorization: OSS nxj7dtwhcyl5hp****:sNKIHT6ci/z231yIT5vYnetD****
    User-Agent: aliyun-sdk-python-test/0.4.0
    
    <WebsiteConfiguration>
      <IndexDocument>
        <Suffix>index.html</Suffix>
        <SupportSubDir>true</SupportSubDir>
        <Type>0</Type>
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
        <RoutingRule>
          <Condition>
            <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
          </Condition>
          <RuleNumber>3</RuleNumber>
          <Redirect>
            <ReplaceKeyWith>prefix/${key}</ReplaceKeyWith>
            <HttpRedirectCode>302</HttpRedirectCode>
            <EnableReplacePrefix>false</EnableReplacePrefix>
            <PassQueryString>false</PassQueryString>
            <Protocol>http</Protocol>
            <HostName>www.test.com</HostName>
            <RedirectType>External</RedirectType>
          </Redirect>
        </RoutingRule>
      </RoutingRules>
    </WebsiteConfiguration>
    
    HTTP/1.1 200 OK
    Server: AliyunOSS
    Date: Fri, 27 Jul 2018 09:03:18 GMT
    Content-Length: 0
    Connection: keep-alive
    x-oss-request-id: 5B5ADFD6ED3CC49176CBE29D
    x-oss-server-time: 47
    ```


## SDK

-   [Java](/intl.zh-CN/SDK 示例/Java/存储空间/静态网站托管.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/存储空间/静态网站托管.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/存储空间/静态网站托管.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/存储空间/静态网站托管.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/存储空间/静态网站托管.md)
-   [C](/intl.zh-CN/SDK 示例/C/存储空间/静态网站托管.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/存储空间/静态网站托管.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/存储空间/静态网站托管.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|InvalidDigest|400|上传了Content-MD5请求头后，OSS会计算消息体的Content-MD5并检查一致性，如果不一致则返回此错误码。|

