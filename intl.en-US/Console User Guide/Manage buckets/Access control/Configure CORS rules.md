# Configure CORS rules

This topic describes how to configure cross-origin resource sharing \(CORS\) rules in the OSS console.

OSS provides CORS over HTML5 to enable cross-origin access. When OSS receives a cross-origin request \(or an OPTIONS request\) sent to a bucket, OSS reads the CORS rules of the bucket and checks the relevant permissions. OSS matches the request with the rules one by one. When OSS finds the first match, OSS returns a corresponding header in the response. If no match is found, OSS does not include any CORS header in the response.

-   You can configure up to 10 CORS rules for a bucket.
-   To implement cross-origin access \(CORS\) when Alibaba Cloud CDN is activated, you must configure CORS rules in the CDN console. For more information, see [Configure CORS for Alibaba Cloud CDN](https://www.alibabacloud.com/help/zh/faq-detail/40183.htm).

For more information, see [Configure CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md).

## Video tutorial

You can view the following video to know how to configure cross-origin access. 

## Procedure

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Access Control** \> **Cross-Origin Resource Sharing \(CORS\)**. In the **Cross-Origin Resource Sharing \(CORS\)** section, click **Configure**.

4.  Click **Create Rule**. In the Create Rule dialog box that appears, configure the parameters described in the following table.

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |**Sources**|Yes|Specifies the sources from which you want to allow cross-origin requests. Note the following rules when you configure the sources:    -   You can configure multiple rules for sources. Separate multiple rules with new lines.
    -   The domain names must include the protocol name, such as HTTP or HTTPS.
    -   Asterisks \(\*\) are supported as wildcards. Each rule can contain up to one asterisk \(\*\).
    -   A domain name must contain the port number if the domain name does not use the default port. Example: https://www.example.com:8080.
The following examples show how to configure domain names:

    -   Enter the full domain name to match a specified domain name. Example: https://www.example.com.
    -   Use an asterisk \(\*\) as a wildcard in the domain name to match second-level domain names. Example: https://\*.example.com.
    -   Enter only an asterisk \(\*\) as the wildcard to match all domain names. |
    |**Allowed Methods**|Yes|Specifies the cross-origin request methods that are allowed.|
    |**Allowed Headers**|No|Specifies the response headers for the allowed cross-origin requests. Note the following rules when you configure the allowed headers:     -   This parameter is in the key:value format and case-insensitive. Example: content-type:text/plain.
    -   You can configure multiple rules for allowed headers. Separate multiple rules with new lines.
    -   Each rule can contain up to one asterisk \(\*\) as the wildcard. Set this parameter to an asterisk \(\*\) if you do not have special requirements. |
    |**Exposed Headers**|No|Specifies the response headers for allowed access requests from applications, such as an XMLHttpRequest object in JavaScript. Exposed headers cannot contain asterisks \(\*\).|
    |**Cache Timeout \(Seconds\)**|No|Specifies the time the browser caches the response for a prefetch \(OPTIONS\) request for specific resources.|
    |**Vary: Origin**|No|Selects whether to return the Vary: Origin header. If both CORS and non-CORS requests are sent to OSS, or if the Origin header has multiple possible values, we recommend that you configure the **Vary: Origin** header to avoid errors in the local cache.

**Note:** If **Vary: Origin** is selected, access through the browser or the CDN back-to-origin requests may increase. |

5.  Click **OK**.


