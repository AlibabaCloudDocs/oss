# GetBucketWebsite

You can call this operation to query the static website hosting status and redirection rules configured for a bucket.

## Request structure

```
GET /? website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements

|Element|Type|Description|
|:------|:---|:----------|
|WebsiteConfiguration|Container|The root node.

Parent nodes: none |
|IndexDocument|Container|The container for the default homepage.

Parent nodes: WebsiteConfiguration |
|Suffix|String|The default homepage.

Parent nodes: IndexDocument |
|ErrorDocument|Container|The container for the default 404 page.

Parent nodes: WebsiteConfiguration |
|Key|Container|The default 404 page.

Parent nodes: ErrorDocument |
|RoutingRules|Container|The container for RoutingRule.

Parent nodes: WebsiteConfiguration |
|RoutingRule|Container|Redirection rules or mirroring-based back-to-origin rules

Parent nodes: RoutingRules |
|RuleNumber|Positive integer|The sequence number used to match and execute redirection rules.

Redirection rules are matched based on the value of this parameter. If a match succeeds, the rule is executed and the subsequent rules are not executed.

Parent nodes: RoutingRule |
|Condition|Container|The matching conditions.

If all of the specified conditions are met, the rule is executed. The nodes in the container are in the AND relationship. A request must meet all the conditions to be considered a match.

Parent nodes: RoutingRule |
|KeyPrefixEquals|String|Indicates that only objects whose names contain the specified prefix match the rule.

Parent nodes: Condition |
|HttpErrorCodeReturnedEquals|HTTP status code|Indicates that the rule is matched only when the specified object is accessed and the specified status code is returned. If the redirection rule is the mirroring-based back-to-origin rule, the parameter value must be 404.

Parent nodes: Condition |
|IncludeHeader|Container|Indicates that the rule is matched only when the specified header is included in the request and the header value is equal to the specified value. Up to five containers can be specified.

Parent nodes: Condition |
|Key|String|Indicates that the rule is matched only when the specified header is included in the request and the header value equals the value specified by Equals.

Parent nodes: IncludeHeader |
|Equals|String|Indicates that the rule is matched only when the header specified by Key is included in the request and the header value equals the specified value.

Parent nodes: IncludeHeader |
|Redirect|Container|The operation to perform after the rule is matched.

Parent nodes: RoutingRule |
|RedirectType|String|The redirection type. Valid values:

-   Mirror: back-to-origin
-   External: external redirection. OSS returns the 3xx HTTP redirect code and the Location header for your to redirect the request to another IP address.
-   Internal: internal redirection. OSS redirects the access from object1 to object2 based on the rule. In this case, the user accesses object2 instead of object1.
-   AliCDN: Alibaba Cloud CDN-based redirection. OSS adds an additional header to the request, which is different from the External type. After identifying the header, CDN redirects the access to the specified IP address and returns the obtained data instead of the 3xx redirecting request to the user.

Parent nodes: Redirect |
|PassQueryString|Boolean|Indicates whether the request parameter is carried when the redirection- or mirroring-based back-to-origin is performed.

If the PassQueryString parameter is set to true and "?a=b&c=d" is carried in a request sent to OSS, this parameter is added to the Location header when the rule is 302 redirection. For example, if the request contains "Location: www.test.com?a=b&c=d" and the value of RedirectType is Mirror, the parameter "a=b&c=d" is carried in the back-to-origin request.

Default value: false

Parent nodes: Redirect |
|MirrorURL|String|This element is valid only when the value of RedirectType is Mirror.

URLs that start with http:// or https:// must end with a forward slash \(/\). OSS adds the object name to the string to form the back-to-origin URL. For example, if MirrorURL is set to `http://www.test.com/` and the name of the object to access is myobject, the back-to-origin URL is `http://www.test.com/dir1/myobject`. If MirrorURL is set to `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

Parent nodes: Redirect |
|MirrorPassQueryString|Boolean|This element plays the same role as PassQueryString and has a higher priority than PassQueryString. However, this element take effects only when RedirectType is set to Mirror.

Default value: false

Parent nodes: Redirect |
|MirrorFollowRedirect|Boolean|Indicates whether the access is redirected to the specified Location if the origin returns a 3xx HTTP status code and when the origin receives a mirroring-based back-to-origin request.

For example, when a mirroring-based back-to-origin request is initiated, the origin returns 302, and Location is specified. In this case, if the value of MirrorFollowRedirect is true, OSS continues to send requests to the IP address specified by Location. A request can be redirected for a maximum of 10 times. If the request is redirected for more than 10 times, a mirroring back-to-origin failure message is returned. If the value of MirrorFollowRedirect is false, OSS returns 302 and includes Location in the response. This parameter takes effect only when the value of RedirectType is Mirror.

Default value: true

Parent nodes: Redirect |
|MirrorCheckMd5|Boolean|Indicates whether OSS checks the MD5 hash of the body of the response returned by the origin.

When the value of this parameter is true and the response returned by the origin includes the Content-Md5 header, OSS checks whether the MD5 hash of the obtained data matches the header value. If it is not matched, OSS does not store the data. This parameter takes effect only when the value of RedirectType is Mirror.

Default value: false

 Parent nodes: Redirect|
|MirrorHeaders|Container|Indicates the header carried when mirroring the back-to-origin. This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: Redirect |
|PassAll|Boolean|Indicates whether OSS passes through all request headers \(except for reserved headers and headers that start with oss-, x-oss-, and x-drs-\) to the origin. This parameter takes effect only when the value of RedirectType is Mirror.

Default value: false

Parent nodes: MirrorHeaders |
|Pass|String|Indicates the headers to pass through to the origin. Up to 10 headers can be specified. The header can be a maximum of 1,024 bytes in length. The element can contain only letters, digits, and hyphens \(-\). This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: MirrorHeaders |
|Remove|String|Indicates the headers that are not allowed to be passed through to the origin. Up to 10 headers can be specified, including repeated headers. This element is used together with PassAll. The header can be up to 1,024 bytes in length. The character set of this element is the same as that of Pass. This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: MirrorHeaders |
|Set|Container|Indicates the headers that are sent to the origin. The specified headers are configured in the data returned by the origin regardless of whether they are carried in the request. Up to 10 containers can be specified. This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: MirrorHeaders |
|Key|String|Indicates the key of the header. The key can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass. This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: Set |
|Value|String|Indicates the value of the header. The value can be up to 1,024 bytes in length and cannot contain "\\r\\n". This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: Set |
|Protocol|String|Indicates the protocol used for redirection. For example, if you access the object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header is `https://www.test.com/test`.

This parameter takes effect only when the value of RedirectType is External or AliCDN.

Valid values: http and https.

Parent nodes: Redirect |
|HostName|String|Indicates the domain name used in redirections, which must comply with the naming conventions for domain names. For example, if you access the object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header is `https://www.test.com/test`. This parameter takes effect only when the value of RedirectType is External or AliCDN.

Parent nodes: Redirect |
|HttpRedirectCode|HTTP status code|Indicates the returned status code in redirections. This parameter takes effect only when the value of RedirectType is External or AliCDN.

Valid values: 301, 302, and 307

Parent nodes: Redirect |
|ReplaceKeyPrefixWith|String|Indicates the string used to replace the prefix of the object name in the redirect request. If the prefix of the object is empty, this string is added before the object name. The ReplaceKeyWith and ReplaceKeyPrefixWith nodes cannot be set at the same time.

If KeyPrefixEquals is set to abc/ and ReplaceKeyPrefixWith is set to def/, when you access the abc/test.txt object, the Location header is `http://www.test.com/def/test.txt`.

This parameter takes effect only when the value of RedirectType is Internal, External or AliCDN.

Parent nodes: Redirect |
|ReplaceKeyWith|String|Indicates the string used to replace the prefix of the object name in the redirection request. The $\{key\} variable that indicates the object name in the request is supported. The ReplaceKeyWith and ReplaceKeyPrefixWith nodes cannot be set at the same time.

For example, if ReplaceKeyWith is set to prefix/$\{key\}.suffix, when you access the object named test, the Location header is `http://www.test.com/prefix/test.suffix`.

Conditional. This element must be specified if the value of RedirectType is Internal, External, or AliCDN.

Parent nodes: Redirect |

## Examples

Sample requests

```
Get /? website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqx******k53otfjbyc: BuG4rRK+zNh******1NNHD39zXw=
            
```

Sample responses

-   Sample response when static website hosting rules are configured

    ```
    HTTP/1.1 200
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 218  
    Server: AliyunOSS
    
    <? xml version="1.0" encoding="UTF-8"? >
    <WebsiteConfiguration xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <IndexDocument>
    <Suffix>index.html</Suffix>
        </IndexDocument>
        <ErrorDocument>
            <Key>error.html</Key>
        </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   Sample response when static website hosting rules are not configured

    ```
    HTTP/1.1 404 
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:56:46 GMT
    Connection: keep-alive
    Content-Length: 308  
    Server: AliyunOSS
    
    <? xml version="1.0" encoding="UTF-8"? >
    <Error xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Code>NoSuchWebsiteConfiguration</Code>
        <Message>The specified bucket does not have a website configuration. </Message>
        <BucketName>oss-example</BucketName>
        <RequestId>505191BEC4689A033D00236F</RequestId>
        <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```


Complete code

```
GET /? website HTTP/1.1
Date: Fri, 27 Jul 2018 09:07:41 GMT
Host: test.oss-cn-hangzhou-internal.aliyuncs.com
Authorization: OSS a1nBN******QMf8u:0Jzamofmy******sU9HUWomxsus=
User-Agent: aliyun-sdk-python-test/0.4.0

HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 27 Jul 2018 09:07:41 GMT
Content-Type: application/xml
Content-Length: 2102
Connection: keep-alive
x-oss-request-id: 5B5AE0DD2F7938C45FCED4BA
x-oss-server-time: 47

<? xml version="1.0" encoding="UTF-8"? >
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
```

## SDK

You can use OSS SDKs for the following programming languages to call the GetBucketWebsite operation:

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Static website hosting.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Static website hosting.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Static website hosting.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Static website hosting.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Static website hosting.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Static website hosting.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Static website hosting.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Static website hosting.md)
-   [Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Static website hosting.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|NoSuchBucket|404|The error message returned because the specified bucket does not exist.|
|AccessDenied|403|The error message returned because you are not authorized to perform this operation. Only the bucket owner can query the static website hosting status configured for a bucket.|
|NoSuchWebsiteConfiguration|404|Static website hosting is not enabled for the bucket.|

