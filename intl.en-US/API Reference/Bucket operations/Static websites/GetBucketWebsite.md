# GetBucketWebsite

You can call this operation to query the static website hosting status and redirection rules configured for a bucket.

## Request structure

```
GET /?website HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements

-   The following table describes the element for WebsiteConfiguration in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |-------|----|-------|-----------|
    |WebsiteConfiguration|Container|N/A|The root node. Parent nodes: none |

-   The following table describes the elements for IndexDocument in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |-------|----|-------|-----------|
    |IndexDocument|Container|N/A|The container used to store the default homepage. Parent nodes: WebsiteConfiguration |
    |Suffix|String|index.html|The default homepage. Parent nodes: IndexDocument |

-   The following table describes the elements for ErrorDocument in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |-------|----|-------|-----------|
    |ErrorDocument|Container|N/A|The container used to store the error page. Parent nodes: WebsiteConfiguration |
    |Key|String|error.html|The error page. Parent nodes: ErrorDocument |
    |HttpStatus|String|404|The HTTP status code returned with the error page. Parent nodes: ErrorDocument |

-   The following table describes the elements for RoutingRules, RoutingRule, and RuleNumber in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |-------|----|-------|-----------|
    |RoutingRules|Container|N/A|The container used to store RoutingRule. Parent nodes: WebsiteConfiguration |
    |RoutingRule|Container|N/A|The redirection rule or mirroring-based back-to-origin rule. Parent nodes: RoutingRules |
    |RuleNumber|Positive integer|1|The sequence number used to match and run redirection rules or mirroring-based back-to-origin rules. Redirection rules are matched based on this element. If a match succeeds, only the rule is run and the subsequent rules are not run.

Parent nodes: RoutingRule |

-   The following table describes the elements for RoutingRules, RoutingRule, and Condition in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |-------|----|-------|-----------|
    |Condition|Container|N/A|The matching condition. The rule is run only when all the conditions are met. Parent nodes: RoutingRule |
    |KeyPrefixEquals|String|abc|The prefix of object names. Only objects whose names contain the specified prefix match the rule. Parent nodes: Condition |
    |HttpErrorCodeReturnedEquals|HTTP status code|404|The returned HTTP status code. The rule is matched only when the specified object is accessed and the specified status code is returned. If the redirection type is mirroring-based back-to-origin, the value of this element is 404. Parent nodes: Condition |
    |IncludeHeader|Container|N/A|The header specified in the request. The rule is matched only when the specified header is included in the request and the header value is equal to the specified value. Up to five IncludeHeader containers can be specified. Parent nodes: IncludeHeader |
    |Key|String|host|The key of the header. The rule is matched only when the specified header is included in the request and the header value equals the value specified by Equals. Parent nodes: IncludeHeader |
    |Equals|String|test.oss-cn-beijing-internal.aliyuncs.com|The value of the header. The rule is matched only when the header specified by Key is included in the request and the header value equals the specified value. Parent nodes: IncludeHeader |

-   The following table describes the elements for RoutingRules, RoutingRule, and Redirect in the response to a GetBucketWebsite request.

    |Element|Type|Example|Description|
    |:------|:---|-------|:----------|
    |Redirect|Container|N/A|The operation to perform after the rule is matched. Parent nodes: RoutingRule |
    |RedirectType|String|Mirror|The redirection type.     -   Mirror: mirroring-based back-to-origin.
    -   External: external redirection. Object Storage Service \(OSS\) returns the 3xx HTTP redirect code and the Location header for you to redirect the access to another IP address.
    -   AliCDN: redirection based on Alibaba Cloud Content Delivery Network \(CDN\). OSS adds an additional header to the request, which is different from the External type. After CDN identifies the header, CDN redirects the access to the specified IP address and returns the obtained data instead of the redirect request to the user.
Parent nodes: Redirect |
    |PassQueryString|Boolean|false|Indicates whether the request parameters of the original request is included in the redirect request when the system runs the redirection rule or mirroring-based back-to-origin rule. If the PassQueryString parameter is set to true and"?a=b&c=d" is included in a request sent to OSS, this parameter is added to the Location header when the redirection mode is 302. For example, if the request contains"Location: www.test.com?a=b&c=d" and the value of RedirectType is Mirror, the a=b&c=d parameter is included in the back-to-origin request.

Default value: false

Parent nodes: Redirect |
    |MirrorURL|String|http://www.test.com|The origin URL for mirroring-based back-to-origin. This element is returned only when the value of RedirectType is Mirror. The origin URL must start with http:// or https:// and end with a forward slash \(/\). OSS adds an object name to the end of the URL to generate a back-to-origin URL.

For example, if MirrorURL is set to `http://www.test.com/` and the name of the object to access is myobject, the back-to-origin URL is `http://www.test.com/dir1/myobject`. If MirrorURL is set to `http://www.test.com/dir1/`, the back-to-origin URL is `http://www.test.com/dir1/myobject`.

Parent nodes: Redirect |
    |MirrorPassQueryString|Boolean|false|This element plays the same role as PassQueryString and has a higher priority than PassQueryString. However, this element takes effect only when RedirectType is set to Mirror. Default value: false

Parent nodes: Redirect |
    |MirrorFollowRedirect|Boolean|true|Indicates whether the access is redirected to the address specified by Location if the origin returns a 3xx HTTP status code. This element is returned only when the value of RedirectType is Mirror. For example, when a mirroring-based back-to-origin request is initiated, the origin returns 302 and Location is specified.

    -   true: OSS continues to request the address specified by Location.

The access can be redirected for up to 10 times. If the access is redirected for more than 10 times, the mirroring-based back-to-origin request cannot be returned.

    -   false: OSS returns 302 and passes through Location.
Default value: true

Parent nodes: Redirect |
    |MirrorCheckMd5|Boolean|false|Indicates whether OSS checks the MD5 hash of the body of the response returned by the origin. This element is returned only when the value of RedirectType is Mirror. When the value of this parameter is true and the response returned by the origin includes the Content-Md5 header, OSS checks whether the MD5 hash of the obtained data matches the header value. If the MD5 hash of the obtained data does not match the header value, OSS does not store the data.

Default value: false

 Parent nodes: Redirect|
    |MirrorHeaders|Container|N/A|The headers that are included when a mirroring-based back-to-origin rule is specified for the bucket. This element is returned only when the value of RedirectType is Mirror. Parent nodes: Redirect |
    |PassAll|Boolean|true|Indicates whether OSS passes through all request headers to the origin. The request headers exclude reserved headers and headers that start with `oss-, x-oss-, and x-drs-`. This element is returned only when the value of RedirectType is Mirror. Default value: false

Parent nodes: MirrorHeaders |
    |Pass|String|myheader-key1|The header to pass through to the origin. This element is returned only when the value of RedirectType is Mirror. The header can be up to 1,024 bytes in length and can contain only letters, digits, and hyphens \(-\).

You can specify up to 10 Pass headers.

Parent nodes: MirrorHeaders |
    |Remove|String|myheader-key3|The header that is not allowed to pass through to the origin. Up to 10 Remove headers can be specified. This element is used together with PassAll. The header can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass. This element is returned only when the value of RedirectType is Mirror. Parent nodes: MirrorHeaders |
    |Set|Container|N/A|The header that is sent to the origin. The header is configured in the data returned by the origin regardless of whether the header is included in the request. Up to 10 Set containers can be specified. This element is returned only when the value of RedirectType is Mirror. Parent nodes: MirrorHeaders |
    |Key|String|myheader-key5|The key of the header. The key can be up to 1,024 bytes in length. The character set of this parameter is the same as that of Pass. This element is returned only when the value of RedirectType is Mirror. Parent nodes: Set |
    |Value|String|myheader-value5|The value of the header. The value can be up to 1,024 bytes in length and cannot contain "\\r\\n". This element is returned only when the value of RedirectType is Mirror. Parent nodes: Set |
    |Protocol|String|http|The protocol used to redirect the access. This parameter is returned only when the value of RedirectType is External or AliCDN. For example, if you access an object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header value is `https://www.test.com/test`.

Valid values: http and https.

Parent nodes: Redirect |
    |HostName|String|www.test.com|The domain name used to redirect the access, which must comply with the naming conventions for domain names. This parameter is returned only when the value of RedirectType is External or AliCDN. For example, if you access an object named test, Protocol is set to https, and Hostname is set to `www.test.com`, the Location header value is `https://www.test.com/test`.

Parent nodes: Redirect |
    |HttpRedirectCode|HTTP status code|301|The HTTP redirect code in the response. This parameter takes effect only when the value of RedirectType is External or AliCDN. Valid values: 301, 302, and 307

Parent nodes: Redirect |
    |ReplaceKeyPrefixWith|String|def/|The string used to replace the prefix of the object name in the redirect request. This parameter takes effect only when the value of RedirectType is External or AliCDN. For example, if you access an object named `abc/test.txt` and ReplaceKeyPrefixWith is set to `def/`, the value of the Location header varies based on whether the value of KeyPrefixEquals is empty.

    -   If the value of KeyPrefixEquals is set to `abc/`, the Location header value is `http://www.test.com/def/test.txt`.
    -   If KeyPrefixEquals is empty, the Location header value is `http://www.test.com/def/abc/test.txt`.
Parent nodes: Redirect |
    |ReplaceKeyWith|String|prefix/$\{key\}.suffix|The string used to replace the object name in the redirect request. This parameter takes effect only when the value of RedirectType is External or AliCDN. This element supports the $\{key\} variable, which indicates the object name in the request. For example, if you access an object named test and ReplaceKeyWith is set to `prefix/${key}.suffix`, the Location header value is `http://www.test.com/prefix/test.suffix`.

Parent nodes: Redirect |


## Examples

Sample requests

```
Get /?website HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com   
Date: Thu, 13 Sep 2012 07:51:28 GMT
Authorization: OSS qn6qrrqx******k53otfjbyc: BuG4rRK+zNh******1NNHD39zXw=            
```

Sample responses

-   Sample responses when static website hosting rules are configured

    ```
    HTTP/1.1 200
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 218  
    Server: AliyunOSS
    
    <?xml version="1.0" encoding="UTF-8"?>
    <WebsiteConfiguration xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
    <IndexDocument>
    <Suffix>index.html</Suffix>
        </IndexDocument>
        <ErrorDocument>
           <Key>error.html</Key>
           <HttpStatus>404</HttpStatus>
        </ErrorDocument>
    </WebsiteConfiguration>
    ```

-   Sample responses when static website hosting rules are not configured

    ```
    HTTP/1.1 404 
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Thu, 13 Sep 2012 07:56:46 GMT
    Connection: keep-alive
    Content-Length: 308  
    Server: AliyunOSS
    
    <?xml version="1.0" encoding="UTF-8"?>
    <Error xmlns="http://doc.oss-cn-hangzhou.aliyuncs.com">
        <Code>NoSuchWebsiteConfiguration</Code>
        <Message>The specified bucket does not have a website configuration.</Message>
        <BucketName>oss-example</BucketName>
        <RequestId>505191BEC4689A033D00236F</RequestId>
        <HostId>oss-example.oss-cn-hangzhou.aliyuncs.com</HostId>
    </Error>
    ```


Complete code

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

## SDKs

You can use OSS SDKs in the following programming languages to call the GetBucketWebsite operation:

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
|AccessDenied|403|The error message returned because you are not authorized to perform this operation. Only the bucket owner can query the static website hosting status configurations of a bucket.|
|NoSuchWebsiteConfiguration|404|The error message returned because static website hosting is not configured for the specified bucket.|

