# OSS跨域资源共享（CORS）错误排除

## 问题现象

-   浏览器报403错误，具体如下：

    ```
    OPTIONS http://bucket.oss-cn-beijing.aliyuncs.com/
    XMLHttpRequest cannot load http://bucket.oss-cn-beijing.aliyuncs.com/. Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource. Origin '{yourwebsiet}' is therefore not allowed access. The response had HTTP status code 403.
    ```

-   OSS报CORS请求不允许的错误，具体如下：

    ```
    <Code>AccessForbidden</Code>
    <Message>CORSResponse: This CORS request is not allowed. This is usually because the evaluation of Origin, request method / Access-Control-Request-Method or Access-Control-Requet-Headers are not whitelisted by the resource's CORS spec.</Message>
    ```


## 可能原因

跨域规则配置错误。

**说明：**

-   CORS报错一般是站点类应用导致，浏览器中可以查看请求详情。例如Chrome，按**F12**打开**开发者工具** ，在**Network**页签中查看相应元素。
-   OSS返回的错误可以通过抓包获取。如果使用Wireshark，筛选器可以指定为**host bucket-name.oss-cn-beijing.aliyuncs.com**。

## 解决方案

按照以下步骤正确配置跨域规则。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)，选择目标Bucket，然后选择**权限管理** \> **跨域设置**。

2.  在跨域规则配置页面，设置以下参数：

    -   **来源**（AllowedOrigin）：设置为**\***。
    -   **允许Methods**：选中全部选项（**GET**、**PUT**、**DELETE**、**POST**、**HEAD**）。
    -   **允许Headers**：设置为**\***。
    -   **暴露Headers**：设置为指定值或者不填。

