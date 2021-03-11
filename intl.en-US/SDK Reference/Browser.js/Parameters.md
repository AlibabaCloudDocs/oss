# Parameters

This topic describes the parameters that you can configure when you use OSS SDK for Browser.js.

OSS configuration item descriptions

-   \[accessKeyId\] \{String\}: the AccessKey ID that you created in the Alibaba Cloud Management Console.
-   \[accessKeySecret\] \{String\}: the AccessKey secret that you created in the Alibaba Cloud Management Console.
-   \[stsToken\] \{String\}: the temporary STS token used to access OSS. For more information, see the "[Use STS to authorize temporary access](/intl.en-US/SDK Reference/Node. js/Authorized access.md)" section of the Authorized access topic.
-   \[bucket\] \{String\}: the bucket that you create in the console.
-   \[endpoint\] \{String\}: the endpoint used to access your OSS bucket.
-   \[region\] \{String\}: the region where your bucket is located. Default value: oss-cn-hangzhou.
-   \[internal\] \{Boolean\}: indicates whether to use the Alibaba Cloud internal network to access OSS. Default value: false. Example: Set this parameter to true if you use an ECS instance to access OSS. The ECS instance accesses OSS through the Alibaba Cloud internal network, which saves costs.
-   \[cname\] \{Boolean\}: indicates whether CNAME can be used to access OSS. Default value: false. If you set this parameter to true, you must bind a CNAME to your bucket before you use the CNAME to access the bucket.
-   \[isRequestPay\] \{Boolean\}: indicates whether the pay-by-requester mode is enabled for your bucket. Default value: false.
-   \[secure\] \{Boolean\}: indicates whether HTTPS is used to access OSS. The value of true indicates that HTTPS is used to access OSS. The value of false indicates that HTTP is used to access OSS. For more information, see [FAQ](/intl.en-US/SDK Reference/Node. js/FAQ.md).
-   \[timeout\] \{String\|Number\}: the timeout period in seconds. Default value: 60.

Examples:

```
// Assume that the OSS object is imported to the browser environment. The introduction method can be 'script' or 'npm'.
let store = new OSS({
  accessKeyId: 'your access key',
  accessKeySecret: 'your access secret',
  bucket: 'your bucket name',
  region: 'oss-cn-hangzhou'
});

```

