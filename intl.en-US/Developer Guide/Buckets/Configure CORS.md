# Configure CORS

Cross-origin resource sharing \(CORS\) is a standard cross-origin solution provided by HTML5 to allow web application servers to control cross-origin access, which ensures data transmission security.

## Background information

The same-origin policy is used to keep the website content secure. When Website A uses JavaScript to access Website B of another origin, the browser rejects the request. In this case, you can configure CORS rules to allow cross-origin requests.

For example, two different origins run on the same browser, such as www.example.com and www.test.com. The origins access the same resource from another origin. If the server first receives the request from www.example.com, the server includes the Access-Control-Allow-Origin header in the response to the corresponding user. When the request from www.test.com is sent, the browser returns the last cached response to the user. The header content does not match that of CORS rules. As a result, the request from www.test.com fails.

The same-origin policy is a key security mechanism that isolates potential malicious files by preventing scripts and documents loaded from one origin from interacting with content from another origin. Origins that use the same protocol, domain name or IP address, and port number are considered to be the same origin. The following table lists examples and checks whether the examples and http://www.aliyun.com/org/test.html are from the same origin.

|URL|Access result|Description|
|---|-------------|-----------|
|http://www.aliyun.com/org/other.html|Successful|Same protocol, domain name, and port|
|http://www.aliyun.com/org/internal/page.html|Successful|Same protocol, domain name, and port|
|https://www.aliyun.com/page.html|Failed|Different protocols \(HTTPS and HTTP\)|
|http://www.aliyun.com:22/dir/page.html|Failed|Different port numbers \(22 and 80\)|
|http://www.alibabacloud.com/help/other.html|Failed|Different domain names|

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md)|A user-friendly and intuitive web application|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/CORS.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/CORS.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/CORS.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/CORS.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/CORS.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/CORS.md)|

## CORS rules

OSS allows you to configure CORS rules to allow or deny cross-origin requests. You can configure CORS rules to decide whether to add CORS-related headers. The browser decides whether to reject cross-origin requests. For more information, see [PutBucketCORS](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md).

If both CORS and non-CORS requests exist, or if the Origin header has multiple possible values, we recommend that you configure the `Vary: Origin` header to avoid errors in the local cache.

**Note:** If `Vary: Origin` is configured, access by using the browser or the CDN back-to-origin requests may increase.

## Examples

The website of a user is www.example.com. OSS is used in the backend. On the web page, objects can be uploaded by using JavaScript. However, upload requests from the web page can be sent only to www.example.com. The browser rejects all requests sent to other websites. As a result, data must be uploaded by using www.example.com.

If CORS rules are configured, you can directly upload data to OSS without the need for www.example.com. Implementation process:

1.  After CORS rules are configured, the Origin header is included in an HTTP request to specify the origin. Example header value: www.example.com.
2.  After the server receives the request, the server determines whether to allow the request from the origin based on the CORS rules. If the request is allowed, the server includes header Access-Control-Allow-Origin and header value www.example in the response, which indicates that this cross-origin access is allowed. If the server allows all cross-origin requests, the server includes the Access-Control-Allow-Origin header whose value is an asterisk \(\*\) in the response header.
3.  The browser checks whether the Access-Control-Allow-Origin header is returned in the response to determine whether the cross-origin request is allowed. If this header is not returned, the browser rejects the request.

For more information about CORS configurations, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).

## FAQ

-   What do I do if common errors of CORS configuration items occur?

    For more information about common errors of CORS configuration items, see the "Troubleshooting" section in [OSS CORS]().

-   What do I do if CORS errors occur when I use an accelerated domain name to access OSS?

    If a CORS error occurs when you use an accelerated domain name to access OSS, you must configure CORS rules in the CDN console. For more information, see [Configure CORS for Alibaba Cloud CDN](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm).

-   What do I do if "`Response to preflight request doesn't pass access control check: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'.`" is reported when I send a cross-origin request?

    We recommend that you set `xhr.withCredentials` to false to resolve this problem.


