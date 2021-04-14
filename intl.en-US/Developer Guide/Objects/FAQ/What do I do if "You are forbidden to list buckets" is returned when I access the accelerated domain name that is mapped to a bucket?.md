# What do I do if "You are forbidden to list buckets" is returned when I access the accelerated domain name that is mapped to a bucket?

This topic describes the cause of the "You are forbidden to list buckets" error returned when you access the accelerated domain name that is mapped to a bucket and the solution to the error.

Problem: When you access the accelerated domain name that is mapped to a private bucket for which CDN back-to-origin is enabled, the `You are forbidden to list buckets` error is returned.

![CDN](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9320838161/p245112.png)

Cause: AGetBucket \(ListObejcts\) request is sent to CDN. By default, this request is denied by CDN.

Solution: Rewrite the URL of the accelerated domain name in the CDN console to redirect the URL to a file in the path specified by the URL. For example, if the URL of the accelerated domain name that is mapped to your bucket is `www.cdndomain.com`, rewrite the URL to `www.cdndomain.com/index.html`. For more information about how to rewrite URLs, see [Create a URI rewrite rule](/intl.en-US/Domain Management/Cache settings/Create a URI rewrite rule.md).

