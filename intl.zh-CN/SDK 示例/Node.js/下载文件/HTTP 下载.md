# HTTP 下载

您可以在不使用SDK的情况下，直接使用HTTP下载存放在OSS中的文件。

HTTP下载包括直接使用浏览器下载，或者使用`wget`、`curl`等命令行工具下载。文件的URL由SDK生成。

**说明：** 使用`signatureUrl`方法生成可下载的HTTP地址，URL的有效时间默认设置为3600秒，无最大有效时间限制，但出于安全考虑，建议设置的URL有效时间不宜过长。

使用HTTP下载OSS中的文件的示例代码如下：

```
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'，
});

let url = client.signatureUrl('object-name');
console.log(url);

let url = client.signatureUrl('object-name', {expires: 3600});
console.log(url);

// signed URL for PUT
let url = client.signatureUrl('object-name', {method: 'PUT'});
console.log(url);
		
```

