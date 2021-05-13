# Configure CORS

Cross-origin resource sharing \(CORS\) is a standard cross-origin solution provided by HTML5 to allow web application servers to control cross-origin access, which ensures the security of data transmission across origins.

## Background information

Browsers check cross-origin requests based on the same-origin policy to keep the website content secure. When a request is sent from Website A by using JavaScript to access Website B of another origin, the browser rejects the request. In this case, you can configure CORS rules to allow cross-origin requests.

For example, two different origins run on the same browser, such as www.example.com and www.test.com. The origins access the same resource from another origin. If the server first receives the request from www.example.com, the server includes the Access-Control-Allow-Origin header in the response to the corresponding user. When the request from www.test.com is sent, the browser returns the last cached response to the user. The header content does not match that of CORS rules. As a result, the request from www.test.com fails.

The same-origin policy is a key security mechanism that isolates potential malicious files. It prevents scripts and documents loaded from different origins from interacting with each other. Origins that use the same protocol, domain name or IP address, and port number are considered to be the same origin. The following table lists examples and checks whether the examples and http://www.aliyun.com/org/test.html are from the same origin.

|URL|Access result|Cause|
|---|-------------|-----|
|http://www.aliyun.com/org/other.html|Successful|Same protocol, domain name, and port number|
|http://www.aliyun.com/org/internal/page.html|Successful|Same protocol, domain name, and port|
|https://www.aliyun.com/page.html|No|Different protocols \(HTTP and HTTPS\)|
|http://www.aliyun.com:22/dir/page.html|No|Different port numbers \(22 and 80\)|
|http://www.alibabacloud.com/help/other.html|No|Different domain names|

## Implementation methods

|Operating method|Limit|
|----------------|-----|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md)|A user-friendly and intuitive web application|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/CORS.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/CORS.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/CORS.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/CORS.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/CORS.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/CORS.md)|

## CORS rules

OSS allows you to configure CORS rules to allow or deny cross-origin requests. You can configure CORS rules to decide whether to add CORS-related headers. The browser decides whether to reject cross-origin requests. For more information, see [PutBucketCORS](/intl.en-US/API Reference/Bucket operations/CORS/PutBucketCors.md).

You must set the `ResponseVary` header to true in the following scenarios to avoid the confusion of local cache.

**Note:** If you set the `ResponseVary` header to true, the number of access by using the browser or Content Delivery Network \(CDN\) back-to-origin requests may increase.

-   CORS and non-CORS requests are sent at the same time

    For example, in the following code, a non-CORS request is created in the <img\> field and a CORS request is created by the fetch method.

    ```
    <!doctype html>
    <html>
    <head>
      <meta charset="UTF-8">
      <title>CORS Test</title>
    </head>
    <body>
    // Create a non-CORS request. 
    <img src="https://examplebucket.oss-cn-beijing.aliyuncs.com/exampleobject.txt" alt="">
    <script>
      // Create a CORS request. 
      fetch("https://examplebucket.oss-cn-beijing.aliyuncs.com/exampleobject.txt").then(console.log)
    </script>
    </body>
    </html>
    ```

-   The Origin header has multiple possible values

    For example, you can set the Origin header to both `http://www.example.com` and `https://www.example.org` to allow CORS requests from these origins.


## Example

The website of a user is `www.example.com`. OSS is used as the backend storage of this website. On the web page, objects can be uploaded by using JavaScript. However, upload requests from the web page can be sent only to `www.example.com`. The browser rejects all requests sent to other websites. As a result, data must be uploaded through `www.example.com`.

If you configure a CORS rule, you can directly upload data to OSS without the need for `www.example.com`. The CORS rule is implemented in the following steps:

1.  After the CORS rule is configured, the Origin header is included in HTTP requests to specify the origin from which the requests are sent. In this example, the value of the Origin header is `www.example.com`.
2.  After the server receives the request, the server determines whether to allow the request based on the CORS rule. If the request is allowed, the server includes the Access-Control-Allow-Origin header and sets its value to `www.example.com` in the response, which indicates that this cross-origin request is allowed. If the CORS rule allows all cross-origin requests, the server includes the Access-Control-Allow-Origin header and sets its value to an asterisk \(\*\) in the response header.
3.  The browser checks whether the Access-Control-Allow-Origin header is included in the response to determine whether the CORS request is allowed. If this header is not returned, the browser rejects the request.

For more information about CORS configurations, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).

## FAQ

-   What do I do if common errors of CORS configuration items occur?

    For more information about common errors of CORS configuration items, see the "Troubleshooting" section in [OSS CORS]().

-   What do I do if CORS errors occur when I use an accelerated domain name to access OSS?

    If a CORS error occurs when you use an accelerated domain name to access OSS, you must configure CORS rules in the CDN console. For more information, see [Configure CORS for Alibaba Cloud CDN](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm).

-   What do I do if the `Response to preflight request doesn't pass access control check: The value of the 'Access-Control-Allow-Origin' header in the response must not be the wildcard '*' when the request's credentials mode is 'include'.`error occurs when I send a cross-origin request?

    We recommend that you set `xhr.withCredentials` to false to resolve this problem.


