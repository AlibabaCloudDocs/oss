# Initialization

This topic describes how to initialize OSS SDK for Node.js.

Create an object named `app.js` and write the following content to the object:

```
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>'
});
        
```

The `region` parameter indicates the region from which OSS services are provided. Example: `oss-cn-hangzhou`. For the complete list of regions, see [Endpoints and regions](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

**Note:** If the endpoint that you use is not included in the list, you can configure the following parameters to specify the endpoint.

-   internal: This parameter must be set together with `region`. If you set `internal` to `true`, you can access OSS over the internal network.
-   secure: This parameter must be set together with the `region` parameter. If you set `secure` to `true`, you can access OSS by using HTTPS.
-   endpoint: If you set the `endpoint` parameter to a value such as `http://oss-cn-hangzhou.aliyuncs.com`, you do not need to set the `region` parameter. The value of the `endpoint` parameter can be an HTTPS domain name or an IP address.
-   cname: This parameter must be set together with the `endpoint` parameter. If you set `cname` to `true`, the value of `endpoint` is used as the custom domain name that is bound to a bucket.
-   bucket: If the `bucket` parameter is not configured, you must first call `useBucket` when you perform operations on objects stored in the bucket. You need to call this operation only once.
-   timeout: This parameter specifies the timeout period for API operations performed to access OSS. Default value: 60000. Unit: millisecond.

