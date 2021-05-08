# PutBucketWebsite

You can call this operation to set a bucket to the static website hosting mode and configure redirection rules.

## Usage notes

Static websites are websites where all web pages consist only of static content, including scripts such as JavaScript code that runs on the client. If you want to specify an OSS object as a static page, the static page cannot contain content that needs to be processed by the server, such as PHP, JSP, and ASP.NET content.

-   Supported features

    The PutBucketWebsite operation is used to configure the default homepage, default 404 page, and RoutingRule of the bucket that is set to the static website hosting mode. RoutingRule is used to specify redirection rules and mirroring-based back-to-origin rules. Mirroring-based back-to-origin supports the Alibaba Cloud public cloud and Finance Cloud.

-   Access by using custom domain names

    To use a custom domain name to access bucket-based static websites, you can use Canonical Names \(CNAMEs\). For more information about specific operations, see [Map custom domain names](/intl.en-US/Developer Guide/Buckets/Map custom domain names.md).

-   Index page and error page

    When you set a bucket to the static website hosting mode, you can specify the index page and the error page of the static website. The specified index page and error page must be objects in the bucket.

-   Anonymous access to the root domain name

    After a bucket is set to the static website hosting mode, Object Storage Service \(OSS\) returns the index page for anonymous access to the root domain name of the static website. If a signed request is sent to access the root domain name of the static website, OSS returns the result of the GetBucket \(ListObjects\) operation.


## Request structure

```
PUT /?website HTTP/1.1
Date: GMT Date
Content-Length: ContentLength
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
        <HttpStatus>404</HttpStatus>
    </ErrorDocument>
</WebsiteConfiguration>
```

## Request parameters

-   The following table describes the element for WebsiteConfiguration.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |WebsiteConfiguration|Container|Yes|The root node. Parent nodes: none |

-   The following table describes the elements for IndexDocument.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |IndexDocument|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container for the default homepage. Parent nodes: WebsiteConfiguration |
    |Suffix|String|ConditionalThis element must be specified if IndexDocument is specified.

|The default homepage. After the default homepage is specified, OSS returns the default homepage if an object whose name ends with a forward slash \(/\) is accessed.

Parent nodes: IndexDocument |
    |SupportSubDir|String|No|Specifies whether to redirect the access to the default homepage in the subdirectory when the subdirectory is accessed. Valid values:

    -   true: The access is redirected to the default homepage of the subdirectory.
    -   false: The access is redirected to the default homepage of the root directory instead of that of the subdirectory.
Example: The default homepage is set to index.html, and `bucket.oss-cn-hangzhou.aliyuncs.com/subdir/` is to be accessed. If SupportSubDir is set to false, the access is redirected to `bucket.oss-cn-hangzhou.aliyuncs.com/index.html`. If SupportSubDir is set to true, the access is redirected to `bucket.oss-cn-hangzhou.aliyuncs.com/subdir/index.html`.

Parent nodes: IndexDocument |
    |Type|Enumeration|No|The operation to perform when the default homepage is set, the name of the accessed object does not end with a forward slash \(/\), and the object does not exist. This parameter takes effect only when SupportSubDir is set to true. It takes effect after RoutingRule but before ErrorFile. If the default homepage for access to `bucket.oss-cn-hangzhou.aliyuncs.com/abc` is set to index.html and the abc object does not exist, the valid values of Type correspond to the following operations. The default value of Type is 0.

    -   0: OSS checks whether the object named abc/index.html, which is in the `Object + Forward slash (/) + Homepage` format, exists. If the object exists, OSS returns 302 and the Location header value that contains URL-encoded `/abc/`. The URL-encoded /abc/ is in the `Forward slash (/) + Object + Forward slash (/)` format. If the object does not exist, OSS returns 404 and continues to check ErrorFile.
    -   1: OSS returns 404 and NoSuchKey and continues to check ErrorFile.
    -   2: OSS checks whether abc/index.html exists. If abc/index.html exists, the content of the object is returned. If abc/index.html does not exist, OSS returns 404 and continues to check ErrorFile.
Parent nodes: IndexDocument |

-   The following table describes the elements for ErrorDocument.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |ErrorDocument|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container used to store the default 404 page. Parent nodes: WebsiteConfiguration |
    |Key|String|ConditionalThis element must be specified if ErrorDocument is specified.

|The error page. After the error page is specified, the error page is returned if the object to access does not exist.

Parent nodes: ErrorDocument |
    |HttpStatus|String|No|The HTTP status code returned with the error page. Default value: 404. Valid values: 200 and 404

Parent nodes: ErrorDocument |

-   The following table describes the elements for RoutingRules, RoutingRule, and RuleNumber.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |RoutingRules|Container|ConditionalYou must specify at least one of the following containers: IndexDocument, ErrorDocument, and RoutingRules.

|The container used to store RoutingRule. Parent nodes: WebsiteConfiguration |
    |RoutingRule|Container|No|The redirection rule or mirroring-based back-to-origin rule. You can specify a maximum of 20 rules. Parent nodes: RoutingRules |
    |RuleNumber|Positive integer|ConditionalThis element must be specified if RoutingRule is specified.

|The sequence number used to match and run the redirection rules. Redirection rules are matched based on this element. If a match succeeds, only the rule is run and the subsequent rules are not run. Parent nodes: RoutingRule |

-   The following table describes the elements for RoutingRules, RoutingRule, and Condition.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |Condition|Container|ConditionalThis element must be specified if RoutingRule is specified.

|The matching condition. If all of the specified conditions are met, the rule is run. A rule is considered matched only when the rule meets the conditions specified by all nodes in Condition.

Parent nodes: RoutingRule |
    |KeyPrefixEquals|String|No|The prefix of object names. Only objects whose names contain the specified prefix match the rule. Parent nodes: Condition |
    |HttpErrorCodeReturnedEquals|HTTP status code|No|The HTTP status code. The rule is matched only when the specified object is accessed and the specified HTTP status code is returned. If the redirection type is mirroring-based back-to-origin, the value of this element is 404. Parent nodes: Condition |
    |IncludeHeader|Container|No|The header specified in the request. The rule is matched only when the specified header is included in the request and the header value is equal to the specified value. You can specify up to 10 IncludeHeader containers. Parent nodes: Condition |
    |Key|String|Yes|The key of the header. The rule is matched only when the specified header is included in the request and the header value equals the value specified by Equals. Parent nodes: IncludeHeader |
    |Equals|String|No|The value of the header. The rule is matched only when the header specified by Key is included in the request and the header value equals the specified value. Parent nodes: IncludeHeader |
    |KeySuffixEquals|String|No|The suffix of object names. Only objects whose names contain the specified suffix match the rule. The default value is empty, which indicates that no suffix is specified.

Parent nodes: Condition |

-   The following table describes the elements for RoutingRules, RoutingRule, and Redirect.

    |Element|Type|Required|Description|
    |-------|----|--------|-----------|
    |Redirect|Container|ConditionalThis element must be specified if RoutingRule is specified.

|The operation to perform after the rule is matched. Parent nodes: RoutingRule |
    |RedirectType|String|ConditionalThis parameter must be specified if Redirect is specified.

|The redirection type. Valid values:

    -   Mirror: mirroring-based back-to-origin.
    -   External: external redirection. OSS returns the 3xx HTTP redirect code and the Location header for you to redirect the access to another IP address.
    -   AliCDN: redirection based on Alibaba Cloud Content Delivery Network \(CDN\). OSS adds an additional header to the request, which is different from the External type. After CDN identifies the header, CDN redirects the access to the specified IP address and returns the obtained data instead of the redirect request to the user.
Parent nodes: Redirect |
    |PassQueryString|Boolean|No|Specifies whether to include parameters of the original request in the redirect request when the redirection rule or mirroring-based back-to-origin rule is to run. For example, if the PassQueryString parameter is set to true and the `?a=b&c=d` parameter is included in a request sent to OSS, PassQueryString is set to true, and the redirection mode is 302, this parameter is added to the Location header. For example, if the request is `Location:www.test.com?a=b&c=d` and the redirection type is mirroring-based back-to-origin, the ?a=b&c=d parameter is also included in the back-to-origin request. Default value: false

Valid values: true and false

Parent nodes: Redirect |
    |MirrorURL|String|ConditionalThis element must be specified if RedirectType is set to Mirror.

|The origin URL for mirroring-based back-to-origin. This element takes effect only when the value of RedirectType is Mirror. The origin URL must start with http:// or https:// and end with a forward slash \(/\). OSS adds an object name to the end of the URL to generate a back-to-origin URL.

For example, the name of the object to access is myobject. If MirrorURL is set to `http://www.test.com/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`. If MirrorURL is set to `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

Parent nodes: Redirect |
    |MirrorPassQueryString|Boolean|No|This element plays the same role as PassQueryString and has a higher priority than PassQueryString. This element takes effect only when the value of RedirectType is Mirror. Default value: false

Parent nodes: Redirect |
    |MirrorFollowRedirect|Boolean|No|Specifies whether to redirect the access to the address specified by Location if the origin returns a 3xx HTTP status code. This element takes effect only when the value of RedirectType is Mirror. For example, when a mirroring-based back-to-origin request is initiated, the origin returns 302 and Location is specified.

    -   If you set MirrorFollowRedirect to true, OSS continues to request the address specified by Location.

The access can be redirected for up to 10 times. If the access is redirected for more than 10 times, the mirroring-based back-to-origin request fails.

    -   If you set MirrorFollowRedirect to false, OSS returns 302 and passes through Location.
Default value: true

Parent nodes: Redirect |
    |MirrorCheckMd5|Boolean|No|Specifies whether to check the MD5 hash of the body of the response returned by the origin. This element takes effect only when the value of RedirectType is Mirror. When the MirrorFollowRedirect value is true and the response returned by the origin includes the Content-Md5 header, OSS checks whether the MD5 hash of the obtained data matches the header value. If the MD5 hash of the obtained data does not match the header value, OSS does not store the data.

Default value: false

 Parent nodes: Redirect|
    |MirrorHeaders|Container|No|The headers contained in the response that is returned when you use mirroring-based back-to-origin. This element takes effect only when the value of RedirectType is Mirror. Parent nodes: Redirect |
    |PassAll|Boolean|No|Specifies whether to pass through all request headers to the origin. This element takes effect only when the value of RedirectType is Mirror. The request headers exclude the following headers:     -   Headers such as content-length, authorization2, authorization, range, and date
    -   Headers that start with oss-, x-oss-, and x-drs-
Default value: false

Parent nodes: MirrorHeaders |
    |Pass|String|No|The header to pass through to the origin. This element takes effect only when the value of RedirectType is Mirror. The header can be up to 1,024 bytes in length and can contain only letters, digits, and hyphens \(-\).

You can specify up to 10 Pass headers.

Parent nodes: MirrorHeaders |
    |Remove|String|No|The header that is not allowed to pass through to the origin. This element takes effect only when the value of RedirectType is Mirror. The header can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass.

You can specify up to 10 Remove headers. This parameter is used together with PassAll.

Parent nodes: MirrorHeaders |
    |Set|Container|No|The header that is sent to the origin. The header is configured in the data returned by the origin regardless of whether the header is included in the request. This element takes effect only when the value of RedirectType is Mirror. You can specify up to 10 Set headers.

Parent nodes: MirrorHeaders |
    |Key|String|ConditionalThis element must be specified if Set is specified.

|The key of the header. The key can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass. This element takes effect only when the value of RedirectType is Mirror. Parent nodes: Set |
    |Value|String|ConditionalThis element must be specified if Set is specified.

|The value of the header. The value can be up to 1,024 bytes in length and cannot contain `\r\n`. This element takes effect only when the value of RedirectType is Mirror. Parent nodes: Set |
    |Protocol|String|No|The protocol used for redirection. This element takes effect only when the value of RedirectType is External or AliCDN. For example, if you access an object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header value is `https://www.test.com/test`.

Valid values: http and https.

Parent nodes: Redirect |
    |HostName|String|No|The domain name used for redirection, which must comply with the naming conventions for domain names. For example, if you access an object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header value is `https://www.test.com/test`.

Parent nodes: Redirect |
    |ReplaceKeyPrefixWith|String|No|The string that is used to replace the prefix of the object name during redirection. If the prefix of the object is empty, this string is added to the front of the object name. **Note:** You can specify only one of the ReplaceKeyWith and ReplaceKeyPrefixWith nodes in a rule.

For example, the object to be accessed is abc/test.txt. If you set KeyPrefixEquals to abc/ and ReplaceKeyPrefixWith to def/, the Location header value is `http://www.test.com/def/test.txt`.

Parent nodes: Redirect |
    |EnableReplacePrefix|Boolean|No|If this element is set to true, the prefix of object names is replaced with the value specified by ReplaceKeyPrefixWith. If this parameter is not specified or empty, the prefix of object names is truncated. **Note:** When the ReplaceKeyWith element is not empty, the EnableReplacePrefix element cannot be set to true.

Default value: false

Parent nodes: Redirect |
    |ReplaceKeyWith|String|No|The string that is used to replace the requested object name when the request is redirected. This element can be set to a variable. The $\{key\} variable that indicates the object name in the request is supported. For example, the name of the object to access is test. If ReplaceKeyWith is set to `prefix/${key}.suffix`, the address specified by the Location header is `http://www.test.com/prefix/test.suffix`.

Parent nodes: Redirect |
    |HttpRedirectCode|HTTP status code|No|The HTTP redirect code in the response. This element takes effect only when the value of RedirectType is External or AliCDN. Default value: 301. Valid values: 301, 302, and 307

Parent nodes: Redirect |


For more information about other common request elements involved in this API operation, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

For more information about the response elements involved in this API operation, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Examples

-   Sample requests

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
        <HttpStatus>404</HttpStatus>
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


## SDKs

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
|InvalidDigest|400|The error message returned because the Content-MD5 value of the message body calculated by OSS is inconsistent with the Content-MD5 value in the request header.|

