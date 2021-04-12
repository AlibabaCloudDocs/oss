# GetBucketWebsite

调用GetBucketWebsite接口查看存储空间（Bucket）的静态网站托管状态以及跳转规则。

## 请求语法

```
GET /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素

-   WebsiteConfiguration的内容

    |名称|类型|示例值|描述|
    |--|--|---|--|
    |WebsiteConfiguration|容器|不涉及|根节点。父节点：无 |

-   IndexDocument的内容

    |名称|类型|示例值|描述|
    |--|--|---|--|
    |IndexDocument|容器|不涉及|默认主页的容器。父节点：WebsiteConfiguration |
    |Suffix|字符串|index.html|默认主页。父节点：IndexDocument |

-   ErrorDocument的内容

    |名称|类型|示例值|描述|
    |--|--|---|--|
    |ErrorDocument|容器|不涉及|错误页面的容器。父节点：WebsiteConfiguration |
    |Key|字符串|error.html|错误页面。父节点：ErrorDocument |
    |HttpStatus|字符串|404|返回错误页面时的HTTP状态码。父节点：ErrorDocument |

-   RoutingRules\|RoutingRule\|RuleNumber的内容

    |名称|类型|示例值|描述|
    |--|--|---|--|
    |RoutingRules|容器|不涉及|RoutingRule的容器。父节点：WebsiteConfiguration |
    |RoutingRule|容器|不涉及|跳转规则或者镜像回源规则。父节点：RoutingRules |
    |RuleNumber|正整数|1|匹配并执行跳转规则或者镜像回源规则的序号。按照此序号顺序进行规则匹配，如果匹配成功，则执行此规则，且后续的规则不再执行。

父节点：RoutingRule |

-   RoutingRules\|RoutingRule\|Condition的内容

    |名称|类型|示例值|描述|
    |--|--|---|--|
    |Condition|容器|不涉及|匹配的条件。需满足所有指定的项才执行此规则。父节点：RoutingRule |
    |KeyPrefixEquals|字符串|abc|只有匹配此前缀的Object才能匹配此规则。父节点：Condition |
    |HttpErrorCodeReturnedEquals|HTTP状态码|404|访问指定Object时返回此状态码才能匹配此规则。当跳转规则是镜像回源类型时，此字段必须为404。父节点：Condition |
    |IncludeHeader|容器|不涉及|只有请求中包含了指定Header且值为指定值时，才能匹配此规则。此容器可以最多重复5个。父节点：IncludeHeader |
    |Key|字符串|host|只有请求中包含了此头且值为Equals指定值时，才能匹配此规则。父节点：IncludeHeader |
    |Equals|字符串|test.oss-cn-beijing-internal.aliyuncs.com|只有请求中包含了Key指定的头且值为指定的值时，才能匹配此规则。父节点：IncludeHeader |

-   RoutingRules\|RoutingRule\|Redirect的内容

    |名称|类型|示例值|描述|
    |:-|:-|---|:-|
    |Redirect|容器|不涉及|指定匹配此规则后执行的动作。父节点：RoutingRule |
    |RedirectType|字符串|Mirror|跳转类型。    -   Mirror：镜像回源。
    -   External：外部跳转，即OSS会返回一个3xx请求，指定跳转到另外一个地址。
    -   AliCDN：阿里云CDN跳转，主要用于阿里云的CDN。与External不同的是，OSS会额外添加一个Header。阿里云CDN识别到此Header后会主动跳转到指定的地址，返回给用户获取到的数据，而不是将3xx跳转请求返回给用户。
父节点：Redirect |
    |PassQueryString|bool型|false|执行跳转或者镜像回源时，是否要携带发起请求的请求参数。当用户请求OSS时携带了请求参数”?a=b&c=d”且此项设置为true，如果规则是302跳转，则在跳转的Location头中会添加此请求参数。例如请求时携带了”Location:www.test.com?a=b&c=d”，跳转类型是镜像回源，则在发起的回源请求中也会携带此请求参数。

默认值：false

父节点：Redirect |
    |MirrorURL|字符串|http://www.test.com|只有在RedirectType为Mirror时有意义，表示镜像回源的源站地址。源站地址必须以http://或https://开头，且以正斜线（/）结尾，OSS会在此地址后带上Object组成回源URL。

例如要访问的Object名称为myobject，如果指定此项为`http://www.test.com/`，则回源URL为`http://www.test.com/myobject`，如果指定此项为`http://www.test.com/dir1/`，则回源URL为`http://www.test.com/dir1/myobject`。

父节点：Redirect |
    |MirrorPassQueryString|bool型|false|与PassQueryString作用相同，优先级比PassQueryString高，仅在RedirectType为Mirror时生效。默认值：false

父节点：Redirect |
    |MirrorFollowRedirect|bool型|true|如果镜像回源获取的结果为3xx，是否要继续跳转到指定的Location获取数据。仅在RedirectType为Mirror时生效。例如发起镜像回源请求时，源站返回了302，并且指定了Location。

    -   true：OSS会继续请求Location对应的地址。

最多能跳转10次，如果跳转超过10次，则无法返回镜像回源请求。

    -   false：OSS返回302，并透传Location。
默认值：true

父节点：Redirect |
    |MirrorCheckMd5|bool型|false|是否要检查回源主体的MD5。仅在RedirectType为Mirror时生效。当设置此项为true且源站返回的response中含有Content-MD5头时，OSS会检查拉取的数据MD5是否与此头匹配，如果不匹配，则不保存在OSS上。

默认值：false

 父节点：Redirect|
    |MirrorHeaders|容器|不涉及|用于指定镜像回源时携带的头。仅在在RedirectType为Mirror时生效。父节点：Redirect |
    |PassAll|bool型|true|是否透传请求中所有的Header（除了保留的几个Header以及以`oss-/x-oss-/x-drs-`开头的Header）到源站。仅在RedirectType为Mirror时生效。默认值：false

父节点：MirrorHeaders |
    |Pass|字符串|myheader-key1|透传指定的Header到源站。仅在RedirectType为Mirror时生效。每个Header长度最多为1024个字节，字符集为0~9、A~Z、a~z以及短划线（-）。

此字段最多可指定10个。

父节点：MirrorHeaders |
    |Remove|字符串|myheader-key3|禁止透传指定的Header到源站，此字段可以重复，最多10个，一般与PassAll一起使用。每个Header长度最多1024个字节，字符集与Pass相同。仅在RedirectType为Mirror时生效。父节点：MirrorHeaders |
    |Set|容器|不涉及|设置一个Header传到源站，不管请求中是否携带这些指定的Header，回源时都会设置这些Header。该容器可以重复，最多10组。仅在RedirectType为Mirror时生效。父节点：MirrorHeaders |
    |Key|字符串|myheader-key5|设置Header的key，最多1024个字节，字符集与Pass相同。仅在RedirectType为Mirror时生效。父节点：Set |
    |Value|字符串|myheader-value5|设置Header的value，最多1024个字节，且不能出现”\\r\\n” 。仅在RedirectType为Mirror时生效。父节点：Set |
    |Protocol|字符串|http|跳转协议。仅在RedirectType为External或AliCDN时生效。例如访问的文件为test，设置跳转到`www.test.com`，且设置Protocol为https，则Location的头为`https://www.test.com/test`。

取值：http、https

父节点：Redirect |
    |HostName|字符串|www.test.com|跳转时的域名，域名需符合域名规范。仅在RedirectType为External或AliCDN时生效。例如访问的Object为test，Protocol为https，Hostname指定为`www.test.com`，则Location的头为`https://www.test.com/test`。

父节点：Redirect |
    |HttpRedirectCode|HTTP状态码|301|跳转时返回的状态码。仅在RedirectType为External或者AliCDN时生效。取值：301、302、307

父节点：Redirect |
    |ReplaceKeyPrefixWith|字符串|def/|执行Redirect时文件名前缀将替换成此值。仅在RedirectType为External或者AliCDN时生效。例如ReplaceKeyPrefixWith指定为`def/`，访问的文件名称为`abc/test.txt`，根据KeyPrefixEquals是否为空，Location头变化如下：

    -   当KeyPrefixEquals指定为`abc/`，则Location的头为`http://www.test.com/def/test.txt`。
    -   当KeyPrefixEquals指定为空，则Location的头为`http://www.test.com/def/abc/test.txt`。
父节点：Redirect |
    |ReplaceKeyWith|字符串|prefix/$\{key\}.suffix|执行Redirect时文件名将替换成此值。仅在RedirectType为External或者AliCDN时生效。此参数支持变量$\{key\}，$\{key\}表示该请求中的文件名称。例如ReplaceKeyWith为`prefix/${key}.suffix`，访问的文件名称为test，则Location的头为`http://www.test.com/prefix/test.suffix`。

父节点：Redirect |


## 示例

请求示例

```
Get /?website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqx******k53otfjbyc: BuG4rRK+zNh******1NNHD39zXw=            
```

返回示例

-   已设置静态网站规则返回示例

    ```
    HTTP/1.1 200
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 218  
    Server: AliyunOSS
    
    <?xml version="1.0" encoding="UTF-8"?>
    <WebsiteConfiguration xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <IndexDocument>
    <Suffix>index.html</Suffix>
        </IndexDocument>
        <ErrorDocument>
           <Key>error.html</Key>
           <HttpStatus>404</HttpStatus>
        </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   未设置静态网站规则的返回示例

    ```
    HTTP/1.1 404 
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:56:46 GMT
    Connection: keep-alive
    Content-Length: 308  
    Server: AliyunOSS
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
        <Code>NoSuchWebsiteConfiguration</Code>
        <Message>The specified bucket does not have a website configuration.</Message>
        <BucketName>oss-example</BucketName>
        <RequestId>505191BEC4689A033D00236F</RequestId>
        <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```


完整代码

```
GET /?website HTTP/1.1
Date: Fri, 27 Jul 2018 09:07:41 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBN******QMf8u:0Jzamofmy******sU9HUWomxsus=
User-Agent: aliyun-sdk-python-test/0.4.0


<?xml version="1.0" encoding="UTF-8"?>
<WebsiteConfiguration>
  <IndexDocument>
    <Suffix>index.html</Suffix>
  </IndexDocument>
  <ErrorDocument>
    <Key>error.html</Key>
    <HttpStatus>404</HttpStatus>
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
        <IncludeHeader>
          <Key>host</Key>
          <Equals>test.oss-cn-beijing-internal.aliyuncs.com</Equals>
        </IncludeHeader>
        <KeyPrefixEquals>abc/</KeyPrefixEquals>
        <HttpErrorCodeReturnedEquals>404</HttpErrorCodeReturnedEquals>
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

HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:07:41 GMT
Content-Type: application/xml
Content-Length: 2102
Connection: keep-alive
x-oss-request-id: 5B5AE0DD2F7938C45FCED4BA
x-oss-server-time: 47
```

## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/存储空间/静态网站托管.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/存储空间/静态网站托管.md)
-   [PHP](/intl.zh-CN/SDK 示例/PHP/存储空间/静态网站托管.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/存储空间/静态网站托管.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/存储空间/静态网站托管.md)
-   [C](/intl.zh-CN/SDK 示例/C/存储空间/静态网站托管.md)
-   [.NET](/intl.zh-CN/SDK 示例/.NET/存储空间/静态网站托管.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/存储空间/静态网站托管.md)
-   [Ruby](/intl.zh-CN/SDK 示例/Ruby/存储空间/静态网站托管.md)

## 错误码

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有相应的操作权限。只有Bucket的拥有者才可以查看Bucket的静态网站托管状态。|
|NoSuchWebsiteConfiguration|404|目标Bucket未设置静态网站托管功能。|

