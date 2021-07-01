# 获取Request ID

对象存储OSS为接收到的每个请求分配唯一的服务器请求ID，作为关联各类日志信息的标识符。当您在使用OSS过程中遇到错误且希望阿里云技术支持提供协助时，需要提交失败请求的Request ID，以便技术支持快速定位并解决问题。本文介绍获取Request ID的多种方式。

## 通过实时日志获取Request ID

您可以通过OSS控制台实时查询Bucket、Object级别的各类请求日志，请求日志中包含Request ID的信息。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击左侧导航栏的**Bucket列表**，然后单击目标Bucket名称。

3.  选择**日志管理** \> **实时查询**。

4.  按`Ctrl+F`键，搜索request\_id。

    ![log](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3797193261/p283829.jpg)


## 通过开发者工具获取Request ID

以下以上传文件为例，说明如何通过开发者工具获取对应操作的Request ID。

1.  打开开发者工具。

    1.  以Windows系统为例，在浏览器页面按`F12`键，打开开发者工具页面。

    2.  在开发者工具页面，单击**Network**。

2.  上传文件。

    1.  在OSS管理控制台，单击左侧导航栏的**Bucket列表**，然后单击目标Bucket名称。

    2.  单击左侧导航栏的**文件管理**。

    3.  上传文件。具体操作，请参见[上传文件](/cn.zh-CN/控制台用户指南/文件管理/上传文件.md)。

3.  通过开发者工具获取Request ID。

    1.  在开发者工具页面，单击Name页签，选中目标文件。

    2.  单击**Headers**页签，在**Response Headers**区域，获取对应操作的Request ID。

        ![log](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3661383261/p283939.jpg)


## 通过SDK获取Request ID

以下以Python SDK上传、下载文件为例，说明如何获取对应操作的Request ID。

-   获取上传文件操作的Request ID

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # 阿里云账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM用户进行API访问或日常运维，请登录RAM控制台创建RAM用户。
    auth = oss2.Auth('[$AccessKey_ID]', '[$AccessKey_Secret]')
    # Endpoint填写Bucket所在地域对应的Endpoint。以华东1（杭州）为例，Endpoint填写为https://oss-cn-hangzhou.aliyuncs.com。
    # Bucket_Name填写Bucket名称。
    bucket = oss2.Bucket(auth, '[$Endpoint]', '[$Bucket_Name]')
    # Object_Name填写Object名称。
    key = '[$Object_Name]'
    # Object_Content填写待上传的字符串内容。
    requestid= bucket.put_object(key, '[$Object_Content]').request_id
    # 打印Request ID。
    print(requestid)
    ```

-   获取下载文件操作的Request ID

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # 阿里云账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM用户进行API访问或日常运维，请登录RAM控制台创建RAM用户。
    auth = oss2.Auth('[$AccessKey_ID]', '[$AccessKey_Secret]')
    # Endpoint填写Bucket所在地域对应的Endpoint。以华东1（杭州）为例，Endpoint填写为https://oss-cn-hangzhou.aliyuncs.com。
    # Bucket_Name填写Bucket名称。
    bucket = oss2.Bucket(auth, '[$Endpoint]', '[$Bucket_Name]')
    # Object_Name填写不包含Bucket名称在内的Object的完整路径。例如，您在名为examplebucket的Bucket下有名为destdir的目录，目录下包含名为example.txt的Object，则填写为destdir/example.txt。
    # Local_File填写Object下载到本地后的存放路径。例如，Object下载后需要存放到/tmp中，并将文件重命名为local.txt，则填写为/tmp/local.txt。
    requestid = bucket.get_object_to_file('[$Object_Name]', '[$Local_File]').request_id
    # 打印Request ID。
    print(requestid)
    ```


