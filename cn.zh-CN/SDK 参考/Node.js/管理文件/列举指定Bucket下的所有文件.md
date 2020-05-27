# 列举指定Bucket下的所有文件 {#concept_jjz_wxh_dhb .concept}

本文介绍如何列举指定 Bucket 下的所有文件。

通过`list`接口列举当前 Bucket 下的所有文件。主要参数说明如下：

-   prefix：只列出符合特定前缀的文件。
-   marker：只列出文件名大于 marker 之后的文件。
-   delimiter：用于获取文件的公共前缀。
-   max-keys：用于指定最多返回的文件个数。

以下代码用于列举指定 Bucket下的所有文件：

```
let OSS = require('ali-oss');
let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: '<Your bucket name>'，
});
async function list () {
  {
    // 不带任何参数，默认最多返回100个文件。
    let result = await client.list();
    console.log(result);
    // 根据nextMarker继续列出文件。
    if (result.isTruncated) {
      let result = await client.list({
        marker: result.nextMarker
      });
    }
  // 列举前缀为'my-'的文件。
  let result = await client.list({
     prefix: 'my-'
  });
  console.log(result);
  // 列举前缀为'my-'且在'my-object'之后的文件。
  let result = await client.list({
     prefix: 'my-',
     marker: 'my-object'
  });
  console.log(result);
  } catch (e) {
    console.log(e);
  }
}
list();
```

