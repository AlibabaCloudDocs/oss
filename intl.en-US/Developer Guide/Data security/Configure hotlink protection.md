# Configure hotlink protection

You can configure a Referer whitelist for a bucket to prevent your resources in the bucket from unauthorized access.

## Background information

The hotlink protection function allows you to configure a Referer whitelist for a bucket and select whether to allow empty Referer field. This way, only requests from the domain names that are included in the Referer whitelist can access the data in the bucket. OSS allows you to configure Referer whitelists based on the Referer header field in HTTP or HTTPS requests.

The following scenarios describe whether to use hotlink protection to verify access to OSS:

-   Only access to objects by using signed URLs and anonymous requests are verified.
-   Requests that contain the `Authorization` header field are not verified.

OSS determines the source from which a request is sent based on the Referer header field in the request. When a browser sends a request to the web server, the Referer field is contained in the request to indicate the source from which the request is sent. OSS determines whether to allow or deny a request based on the Referer field contained in the request and the Referer whitelist configured for the specified bucket. If the Referer field in the request matches the Referer whitelist, the request is allowed. Otherwise, the request is denied. Example: The Referer whitelist configured for a bucket includes only https://10.10.10.10.com.

-   User A adds an image object named test.jpg to website https://10.10.10.10.com. When a user accesses the image on the website, the browser sends a request in which the value of the Referer field is https://10.10.10.10.com. OSS allows the request because the Referer field in the request is included in the Referer whitelist.
-   User B adds the URL of the image object to the website https://127.0.0.1.com without authorization. When a user accesses the image on the website, the browser sends a request in which the value of Referer field is https://127.0.0.1.com. OSS denies the request because the Referer field in the request is excluded in the Referer whitelist.

For more information about the PutBucketReferer operation, see [PutBucketReferer](/intl.en-US/API Reference/Bucket operations/Hotlink protection/PutBucketReferer.md).

To grant specified users permissions to access a part or all of the resources in your bucket, we recommend that you configure bucket policies. For example, you can configure a policy for your bucket to allow only users with specified IP addresses to access the bucket. For more information about bucket policies, see [Configure bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure bucket policies to authorize other users to access OSS resources.md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure hotlink protection.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/referer.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Buckets/Hotlink protection.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Buckets/Hotlink protection.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Hotlink protection.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Buckets/Hotlink protection.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Buckets/Hotlink protection.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Buckets/Hotlink protection.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/Hotlink protection.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/Hotlink protection.md)|

## Referer

This section describes how to configure Referers for a bucket, how to use wildcards in Referers, and the effects of different Referer configurations.

-   Configure Referers
    -   You can configure multiple Referers for a bucket. Use line breaks to separate Referers when you configure Referers in the console. Use commas to separate Referers when you configure Referers by calling API operations.
    -   You can use asterisks \(\*\) and question marks \(?\) as wildcards in Referers.

        -   An asterisk \(\*\) can be used as a wildcard to indicate zero or more characters. For example, if you add \*.aliyun.com to the Referer whitelist and do not allow empty Referer fields, only HTTP or HTTPS requests in which the Referer field contains aliyun.com such as help.aliyun.com and www.aliyun.com, are allowed to access your resources. if you add \*.aliyun.com to the Referer whitelist and allow empty Referer fields, requests in which the Referer field is empty are also allowed to access your resources.
        -   A question mark \(?\) can be used as a wildcard to indicate a character. For example, if you add aliyun?.com to the Referer whitelist and do not allow empty Referer fields, only HTTP or HTTPS requests in which the Referer field is in the aliyun?.com format, such as aliyuna.com and aliyunb.com, are allowed to access your resources. if you add aliyun?.com to the Referer whitelist and allow empty Referer fields, requests in which the Referer field is empty are also allowed to access your resources.
        For more examples of Referer configurations, see [Configure hotlink protection](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure hotlink protection.md).

-   Effects of Referer configurations
    -   If the Referer whitelist is empty, all requests are allowed.
    -   If the Referer whitelist is not empty and empty Referer fields are not allowed, only requests that contain the Referers specified in the whitelist are allowed.
    -   If the Referer whitelist is not empty and empty Referer fields are allowed, only requests in which the Referer field matches the whitelist or is empty are allowed.

## References

For more information about hotlink protection errors, see [Referer]().

