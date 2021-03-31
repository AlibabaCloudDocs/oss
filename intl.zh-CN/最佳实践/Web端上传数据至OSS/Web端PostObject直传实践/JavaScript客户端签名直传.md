# JavaScript客户端签名直传

本文主要介绍如何基于POST Policy的使用规则在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。

## 注意事项

-   在客户端通过JavaScript代码完成签名，无需过多配置，即可实现直传，非常方便。但是客户端通过JavaScript把AccesssKeyID和AccessKeySecret写在代码里面有泄露的风险，建议采用[服务端签名后直传](/intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/服务端签名后直传.md)。
-   本文档提供的应用服务器代码支持html5、flash、silverlight、html4等协议，请保证您的浏览器支持以上协议。若提示“你的浏览器不支持flash,Silverlight或者HTML5！”，请升级您的浏览器版本。

## 步骤1：下载浏览器客户端代码

本示例采用Plupload直接提交表单数据（即[PostObject](/intl.zh-CN/API 参考/关于Object操作/基础操作/PostObject.md)）到OSS，可以运行于PC浏览器、手机浏览器、微信等。您可以同时选择多个文件上传，并设置上传到指定目录和设置上传文件名称是随机文件名还是本地文件名。您还可以通过进度条查看上传进度。

1.  下载[浏览器客户端代码](https://gosspublic.alicdn.com/doc/oss-h5-upload-js-direct.zip)。

2.  将下载的文件解压。


**说明：** 示例中使用的前端插件是Plupload，您可以根据实际需求更换插件。

## 步骤2：修改配置文件

打开upload.js文件，修改访问配置。

```
accessid= '<yourAccessKeyId>';
accesskey= <yourAccessKeySecret>';
host= <yourHost>';
// 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 [https://ram.console.aliyun.com](https://ram.console.aliyun.com/)
.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        ....
    };

//如果是STS方式临时授权访问OSS，请参见如下代码。
accessid= 'accessid';
accesskey= 'accesskey';
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
STS_ROLE = ''; // eg: acs:ram::1069test7698:role/all

.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': STS.accessID, 
        'x-oss-security-token':STSToken,
        ....
    };
//===========
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
```

-   accessid：您的AccessKeyId。
-   accesskey：您的AcessKeySecret。
-   STS\_ROLE：表示指定角色的ARN，详情请参见[AssumeRole](/intl.zh-CN/API参考/API 参考（STS）/操作接口/AssumeRole.md)中请求参数RoleArn的说明。
-   STSToken：您的STS token。使用STS方式验证时，您需要通过STS API获取STS AccessKeyId、STS AcessKeySecret、SecurityToken，详情请参见[STS临时授权访问OSS](/intl.zh-CN/开发指南/数据安全/访问控制/STS临时授权访问OSS.md)。若您的AccessKeyId和AcessKeySecret为主账号或永久子账号AK，此项可不填。
-   host：您的OSS访问域名，格式为`BucketName.Endpoint`，例如`post-test.oss-cn-hangzhou.aliyuncs.com`。 关于OSS访问域名的介绍请参见[OSS访问域名使用规则](/intl.zh-CN/开发指南/访问域名（Endpoint）/OSS访问域名使用规则.md)。

## 步骤3：设置CORS

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证，因此需要为Bucket设置跨域规则以支持Post方法。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击**Bucket列表**，之后单击目标Bucket名称。

3.  单击**权限管理** \> **跨域设置**，在**跨域设置**区域单击**设置**。

4.  单击**创建规则**，配置如下图所示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p12308.png)

    **说明：** 为了您的数据安全，实际使用时，**来源**栏建议您填写自己需要的域名。更多配置信息请参见[设置跨域访问](/intl.zh-CN/控制台用户指南/存储空间管理/权限管理/设置跨域访问.md)。


## 步骤4：体验JavaScript客户端签名直传

1.  在解压的客户端代码文件夹中打开index.html文件。

2.  单击**选择文件**，之后选择一个或多个文件，并选择上传后的文件名命名规则及上传后文件所在目录。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p62322.png)

3.  单击**开始上传**，并等待上传完成。

4.  上传成功后，您可以登录OSS控制台查看上传结果。


## 核心代码解析

因为OSS支持POST协议，所以只要在Plupload发送POST请求时带上OSS签名即可。核心代码如下：

```
function set_upload_param(up, filename, ret)
{
  g_object_name = g_dirname;
  if (filename != '') {
      suffix = get_suffix(filename)
      calculate_object_name(filename)
  }
  new_multipart_params = {
      'key' : g_object_name,
      'policy': policyBase64,
      'OSSAccessKeyId': accessid,
      'success_action_status' : '200', //如果不设置success_action_status为200，文件上传成功后则返回204状态码。
      'signature': signature,
  };

  up.setOption({
      'url': host,
      'multipart_params': new_multipart_params
  });

  up.start();
}
　....
```

上述代码中，`’key’: g_object_name`表示上传后的文件路径。若您希望上传后保持原来的文件名，请将该字段改为`’key’: '${filename}'`。

如果您希望上传到特定目录，例如abc下，且文件名不变，请修改代码如下：

```
new_multipart_params = {
  'key' : 'abc/' + '${filename}',
  'policy': policyBase64,
  'OSSAccessKeyId': accessid,
  'success_action_status' : '200', //让服务端返回200,不然，默认会返回204
  'signature': signature,
};
```

-   设置成随机文件名

    如果想在上传时固定设置成随机文件名，后缀保持跟客户端文件一致，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   设置成用户的文件名

    如果想在上传时固定设置成用户的文件名，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   设置上传目录

    您可以将文件上传到指定目录下。下面的代码是将上传目录改成abc/，注意目录必须以正斜线（/）结尾。

    ```
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   上传签名

    上传签名主要是对policyText进行签名，最简单的例子如下：

    ```
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", // 设置Policy的失效时间，如果超过失效时间，就无法通过此Policy上传文件
        "conditions": [
        ["content-length-range", 0, 1048576000] // 设置上传文件的大小限制，如果超过限制，文件上传到OSS会报错
        ]
    }
    ```


## 常见问题

-   如何限制上传文件的格式？

    您可以利用Plupload的属性filters设置上传的过滤条件，如设置只能上传图片类型的文件、上传文件的大小、不能有重复上传等。详细代码请参见[服务端签名直传并设置上传回调—客户端源码解析](/intl.zh-CN/最佳实践/Web端上传数据至OSS/Web端PostObject直传实践/服务端签名直传并设置上传回调.md)中的“设置上传过滤条件”。

-   上传后如何获取文件URL？

    您可以根据Bucket访问域名及文件访问路径获取文件的URL，详情请参见[上传Object后如何获取访问URL？](/intl.zh-CN/开发指南/对象/文件（Object）/常见问题/上传Object后如何获取访问URL？.md)。

-   如何获取已上传文件的MD5值？

    您需要按F12打开开发者模式，之后开始上传文件。文件上传成功后在响应头中可以查看文件的MD5，如下图所示。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9354449951/p64542.png)


如果您还有更多问题，请[提交工单](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.ditem-sub.433c12d26HHFbc#/ticket/createIndex)寻求解答。

