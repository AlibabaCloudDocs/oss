# PutBucketWebsite

You can call this operation to set a bucket to the static website hosting mode and configure redirection rules.

## Usage notes

Static websites are websites where all web pages consist of only static content, including scripts such as JavaScript code running on the client. A bucket that is set to the static website hosting mode does not support content that needs to be processed by the server, such as PHP, JSP, and ASP.NET content.

-   Supported features

    The PutBucketWebsite operation is used to configure the default home page, default 404 page, and RoutingRule of the bucket that is set to the static website hosting mode. RoutingRule is used to specify redirection-based back-to-origin rules and mirroring-based back-to-origin rules. Mirroring-based back-to-origin supports Alibaba Cloud and Finance Cloud.

-   Access through custom domain names

    To use a custom domain name to access bucket-based static websites, you can use CNAME. For more information, see [Bind custom domain names](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md).

-   Index page and error page

    When you set a bucket to the static website hosting mode, you can specify the index page and the error page of the static website. The specified index page and error page must be objects in the bucket.

-   Anonymous access to the root domain name

    After a bucket is set to the static website hosting mode, OSS returns the index page for an anonymous access to the root domain name of the static website, For a signed access to the root domain name of the static website, OSS returns the result of the GetBucket \(ListObjects\) operation.


## Request structure

```
PUT /? website HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
Content-Type: application/xml
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Authorization: SignatureValue

<? xml version="1.0" encoding="UTF-8"? >
<WebsiteConfiguration>
    <IndexDocument>
        <Suffix>index.html</Suffix>
    </IndexDocument>
    <ErrorDocument>
        <Key>errorDocument.html</Key>
    </ErrorDocument>
</WebsiteConfiguration>
```

## Request parameters

-   The following table describes the parameters that you can configure in the WebsiteConfiguration field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |WebsiteConfiguration|Container|Yes|The root node. Parent nodes: none |

-   The following table describes the parameters that you can configure in the IndexDocument field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |IndexDocument|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container for the default homepage. Parent nodes: WebsiteConfiguration |
    |Suffix|String|ConditionalThis parameter must be specified if IndexDocument is specified.

|The default homepage. If this parameter is set, the default homepage is returned for access to objects whose names end with a forward slash \(/\).

Parent nodes: IndexDocument |
    |SupportSubDir|String|No|Specifies whether a request sent to access a subfolder is redirected to the default homepage of the subfolder. Default value: false. Valid values:

    -   true: indicates that the request is redirected to the default homepage of the subfolder.
    -   false: indicates that the request is redirected to the default homepage of the root folder instead of that of the subfolder.
If the default homepage for access to `bucket.oss-cn-hangzhou.aliyuncs.com/subdir/` is set to index.html. A value of false indicates that the request is redirected to `bucket.oss-cn-hangzhou.aliyuncs.com/index.html`, and a value of true indicates that the request is redirected to `bucket.oss-cn-hangzhou.aliyuncs.com/subdir/index.html`.

Parent nodes: IndexDocument |
    |Type|Enumeration|No|Specifies the operation to perform when the default homepage is set, the name of the accessed object does not end with a forward slash \(/\), and the object does not exist. **Note:** This parameter takes effect only when SupportSubDir is set to true. It takes effect after RoutingRule and before ErrorFile.

If the default homepage for access to bucket.oss-cn-hangzhou.aliyuncs.com/abc is set to index.html and the abc object does not exist. The valid values of Type correspond to the following operations. The default value of Type is 0.

    -   0: OSS checks whether the object named abc/index.html, which is in the `object + slash (/) + homepage` format, exists. If the object exists, OSS returns 302 and the Location header `/abc/`, which is in the `forward slash (/) + object + forward slash (/)` format, in the response is URL-encoded. If the object does not exist, OSS returns 404 and continues to check ErrorFile.
    -   1: OSS returns 404 and NoSuchKey and continues to check ErrorFile.
    -   2: OSS checks whether abc/index.html exists. If it exists, the content of the object is returned. If it does not exist, OSS returns 404 and continues to check ErrorFile.
Parent nodes: IndexDocument |

-   The following table describes the parameters that you can configure in the ErrorDocument field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |ErrorDocument|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container used to store the default 404 page. Parent nodes: WebsiteConfiguration |
    |Key|Container|ConditionalThis parameter must be specified if ErrorDocument is specified.

|The default 404 page.If this parameter is specified, the 404 page is returned when the target object does not exist.

Parent nodes: ErrorDocument |

-   The following table describes the parameters that you can configure in the RoutingRules\|RoutingRule\|RuleNumber field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |RoutingRules|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container used to store RoutingRule. Parent nodes: WebsiteConfiguration |
    |RoutingRule|Container|No|Specifies redirection-based back-to-origin rules or mirroring-based back-to-origin rules. You can specify a maximum of 20 rules. Parent nodes: RoutingRules |
    |RuleNumber|Positive integer|ConditionalThis parameter must be specified if RoutingRule is specified.

|The sequence number used to match and execute redirection rules. Redirection rules are matched based on this parameter. If a match succeeds, the rule is executed and the subsequent rules are not executed. Parent nodes: RoutingRule |

-   The following table describes the parameters that you can configure in the RoutingRules\|RoutingRule\|Condition field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Condition|Container|ConditionalThis parameter must be specified if RoutingRule is specified.

|The matching conditions. If all of the specified conditions are met, the rule is executed. A rule is determined matched only when it meets the conditions specified by all nodes in Condition.

Parent nodes: RoutingRule |
    |KeyPrefixEquals|String|No|Specifies that only objects whose names contain the specified prefix match the rule. Parent nodes: Condition |
    |HttpErrorCodeReturnedEquals|HTTP status code|No|Specifies that the rule is matched only when the specified object is accessed and the specified status code is returned. If the redirection rule is a mirroring-based back-to-origin rule, the parameter value must be 404. Parent nodes: Condition |
    |IncludeHeader|Container|No|Specifies that the rule is matched only when the specified header is included in the request and the header value is equal to the specified value. You can specify up to 10 headers. Parent nodes: Condition |
    |Key|String|ConditionalThis parameter must be specified if IncludeHeader is specified.

|Specifies that the rule is matched only when the specified header is included in the request and the header value is equal to the value specified by Equals. Parent nodes: IncludeHeader |
    |Equals|String|ConditionalThis parameter must be specified if IncludeHeader is specified.

|Specifies that the rule is matched only when the header specified by Key is included in the request and the header value is equal to the specified value. Parent nodes: IncludeHeader |
    |KeySuffixEquals|String|No|Specifies that only objects whose names contain the specified suffix match the rule. The default value is null, indicating that no suffix is specified.

Parent nodes: Condition |

-   The following table describes the parameters that you can configure in the RoutingRules\|RoutingRule\|Redirect field in a request.

    |Parameter|Type|Required|Description|
    |---------|----|--------|-----------|
    |Redirect|Container|ConditionalThis parameter must be specified if RoutingRule is specified.

|The operation to perform after the rule is matched. Parent nodes: RoutingRule |
    |RedirectType|String|ConditionalThis parameter must be specified if Redirect is specified.

|The redirection type.Valid values:

    -   Mirror: mirroring-based back-to-origin
    -   External: external redirection. OSS returns the 3xx HTTP redirect code and the Location header for your to redirect the request to another IP address.
    -   AliCDN: redirection based Alibaba Cloud CDN. OSS adds an additional header to the request, which is different from the External type. After CDN identifies the header, CDN redirects the access to the specified IP address and returns the obtained data instead of the redirection request to the user.
Parent nodes: Redirect |
    |PassQueryString|Boolean|No|Specifies whether parameters in the original request are contained in the redirecting request when redirection-based or mirroring-based back-to-origin is performed. For example, if the PassQueryString parameter is set to true and the `? a=b&c=d` parameter is contained in the request sent to OSS, this parameter is added to the Location header in the redirection request when the HTTP redirect code is 302. For example, if the request is `Location:www.test.com? a=b&c=d` and the redirecting type is mirroring-based back-to-origin, the ?a=b&c=d parameter is also contained in the back-to-origin request.

Valid values: true and false

Default value: false

Parent nodes: Redirect |
    |MirrorURL|String|ConditionalThis parameter must be specified if RedirectType is set to Mirror.

|Specifies the IP address of the origin in mirroring-based back-to-origin. This parameter takes effect only when the value of RedirectType is Mirror. URLs that start with http:// or https:// must end with a forward slash \(/\). OSS adds the object name to the string to form the back-to-origin URL.

For example, the object to access is myobject. If the specified URL is `http://www.test.com/`, the back-to-origin URL is `http://www.test.com/myobject`. If the specified URL is `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

Parent nodes: Redirect |
    |MirrorPassQueryString|Boolean|No|This parameter plays the same role as PassQueryString and has a higher priority than PassQueryString.This parameter takes effect only when the value of RedirectType is Mirror.

Default value: false

Parent nodes: Redirect |
    |MirrorFollowRedirect|Boolean|No|Specifies whether the access is redirected to the specified Location if the origin returns a 3xx HTTP status code and when the origin receives a mirroring-based back-to-origin request. For example, when a mirroring-based back-to-origin request is initiated, the origin returns 302, and Location is specified. Default value: true.

Valid values:

    -   true: OSS continues to send requests to the IP address specified by Location. A request can be redirected for a maximum of 10 times. If a request is redirected for more than 10 times, a mirroring-based back-to-origin failure is returned.
    -   false: OSS returns 302 and passes through Location. This parameter takes effect only when the value of RedirectType is Mirror.
Parent nodes: Redirect |
    |MirrorCheckMd5|Boolean|No|Specifies whether OSS checks the MD5 hash of the body of the response returned by the origin. When the value of this parameter is true and the response returned by the origin includes the Content-Md5 header, OSS checks whether the MD5 hash of the obtained data matches the header value. If it is not matched, OSS does not store the data. This parameter takes effect only when the value of RedirectType is Mirror.

Default value: false

 Parent nodes: Redirect|
    |MirrorHeaders|Container|No|Specifies the headers contained in the response that is returned when you use mirroring-based back-to-origin. This parameter takes effect only when the value of RedirectType is Mirror. Parent nodes: Redirect |
    |PassAll|Boolean|No|Specifies whether OSS passes through all request headers    -   except for reserved headers and headers that start with oss-, x-oss-, and x-drs-, such as content-length, authorization2, authorization, range, and date,
    -   to the origin.
This parameter takes effect only when the value of RedirectType is Mirror.

Default value: false

Parent nodes: MirrorHeaders |
    |Pass|String|No|Specifies the headers to pass through to the origin.The header can be up to 1,024 bytes in length and can contain only letters, digits, and hyphens \(-\). This parameter takes effect only when the value of RedirectType is Mirror.

You can specify up to 10 headers.

Parent nodes: MirrorHeaders |
    |Remove|String|No|Specifies the headers that are not allowed to be passed through to the origin.The header can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass. This parameter takes effect only when the value of RedirectType is Mirror.

You can specify up to 10 headers. This parameter is used together with PassAll.

Parent nodes: MirrorHeaders |
    |Set|Container|No|Specifies the headers that are sent to the origin. The specified headers are configured in the data returned by the origin regardless of whether they are contained in the request.You can specify up to 10 header sets. This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: MirrorHeaders |
    |Key|String|ConditionalThis parameter must be specified if Set is specified.

|Specifies the key of the header. The key can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass.This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: Set |
    |Value|String|ConditionalThis parameter must be specified if Set is specified.

|Specifies the value of the header. The value can be up to 1,024 bytes in length and cannot contain `\r\n`.This parameter takes effect only when the value of RedirectType is Mirror.

Parent nodes: Set |
    |Protocol|String|ConditionalThis parameter must be specified if the value of RedirectType is External or AliCDN.

|The protocol used for redirection.For example, if you access the object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header is `https://www.test.com/test`.

This parameter takes effect only when the value of RedirectType is External or AliCDN.

Valid values: http and https.

Parent nodes: Redirect |
    |HostName|String|ConditionalThis parameter must be specified if the value of RedirectType is External or AliCDN.

|The domain name used for redirection. It must comply with the naming conventions for domain names.For example, if you access the object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header is `https://www.test.com/test`.

Parent nodes: Redirect |
    |ReplaceKeyPrefixWith|String|No|Specifies the string that is used to replace the prefix of the object name in the redirection request. If the prefix of the object is empty, this string is added before the object name. **Note:** You can specify only one of the ReplaceKeyWith and ReplaceKeyPrefixWith parameters in a rule.

If KeyPrefixEquals is set to abc/ and ReplaceKeyPrefixWith is set to def/, when you access the abc/test.txt object, the Location header is `http://www.test.com/def/test.txt`.

Parent nodes: Redirect |
    |EnableReplacePrefix|Boolean|No|If this parameter is set to true, the prefix of object names is replaced with the value specified by ReplaceKeyPrefixWith. If this parameter is not specified or empty, the prefix of object names is truncated.**Note:** When the ReplaceKeyWith parameter is not empty, this parameter cannot be set to true.

Default value: false

Parent nodes: Redirect |
    |ReplaceKeyWith|String|No|Specifies the string that is used to replace the requested object name when the request is redirected. This parameter can be a variable. The $\{key\} variable that indicates the object name in the request is supported. If ReplaceKeyWith is set to `prefix/${key}.suffix`, when you access the object named test, the Location header is `http://www.test.com/prefix/test.suffix`.

Parent nodes: Redirect |
    |HttpRedirectCode|HTTP status code|ConditionalThis parameter must be specified if the value of RedirectType is External or AliCDN.

|The HTTP redirect code in the response.This parameter takes effect only when the value of RedirectType is External or AliCDN.

Valid values: 301, 302, and 307

Parent nodes: Redirect |


For more information about other common request parameters of PutBucketWebsite, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

For more information about the response elements of PutBucketWebsite, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

    ```
    PUT /? website HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Content-Length: 209
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS nxj7dtwhcyl5hp****:sNKIHT6ci/z231yIT5vYnetD****
    
    <? xml version="1.0" encoding="UTF-8"? >
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

-   Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Fri, 04 May 2012 03:21:12 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   Complete sample code

    ```
    PUT /? website HTTP/1.1
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

-   [Java](/intl.en-US/SDK Reference/Java/Buckets/Static website hosting.md)
-   [Python](/intl.en-US/SDK Reference/Python/Buckets/Static website hosting.md)
-   [Go](/intl.en-US/SDK Reference/Go/Buckets/Static website hosting.md)
-   [C++](/intl.en-US/SDK Reference/C++/Buckets/Static website hosting.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/Buckets/Static website hosting.md)
-   [C](/intl.en-US/SDK Reference/C/Buckets/Static website hosting.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/Buckets/Static website hosting.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Static website hosting.md)

## Error codes

|Error code|HTTP status code|Description|
|:---------|:---------------|:----------|
|InvalidDigest|400|The error message returned because the Content-MD5 value of the message body is inconsistent with the Content-MD5 value in the request header.|

