# 设置文件HTTP头

您可以通过设置文件HTTP头来自定义HTTP请求的策略，例如文件（Object）缓存策略、强制下载策略等。

使用OSS管理控制台每次可批量设置100个Object的HTTP头。如需同时为更多Object设置HTTP头，请使用[ossutil](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md)。

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。

2.  单击左侧导航栏的**Bucket列表**，然后单击目标Bucket。

3.  单击左侧导航栏的**文件管理**。

4.  通过以下任意方式进入设置HTTP头面板：

    -   设置批量Object的HTTP头

        选中一个或多个Object，然后选择**批量操作** \> **设置HTTP头**。

    -   设置单个Object的HTTP头

        单击目标Object或其右侧的**详情**，然后在详情面板单击**设置HTTP头**。

5.  根据需求设置以下参数。

    |参数|说明|
    |--|--|
    |**Content-Type**|声明Object的文件类型，浏览器根据文件类型决定Object的默认打开方式。例如GIF类型的图片设置为**image/gif**。不同文件类型对应的Content-Type设置，请参见[t5041.dita\#concept\_5041](/cn.zh-CN/开发指南/对象/文件（Object）/常见问题/如何设置Content-Type（MIME）？.md)。 |
    |**Content-Encoding**|声明Object的编码方式。您需要按照Object 的实际编码类型填写，否则可能造成客户端（浏览器）解析编码失败或Object下载失败。若Object未编码，请置空此项。可选值：    -   gzip：表示Object采用Lempel-Ziv（LZ77）压缩算法以及32位CRC校验的编码方式。
    -   compress：表示Object采用Lempel-Ziv-Welch（LZW）压缩算法的编码方式。
    -   deflate：表示Object采用zlib结构和deflate压缩算法的编码方式。
    -   identity（默认值）：表示Object未经过压缩或编码。
    -   br：表示Object采用Brotli算法的编码方式。
关于**Content-Encoding**的更多介绍，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。

**说明：** 若您希望访问OSS内常见网页静态文件（HTML、Javascript、XML、json）时进行Gzip压缩，您需要置空此项，并在请求中增加`Accept-Encoding: gzip`。具体操作，请参见[OSS的Gzip压缩如何使用？](/cn.zh-CN/开发指南/对象/文件（Object）/常见问题/OSS的gzip压缩如何使用？.md)。 |
    |**Content-Language**|声明Object内容使用的语言。例如某个Object使用简体中文编写，此项可设置为**zh-CN**。|
    |**Content-Disposition**|指定Object的访问形式。常见取值如下：    -   inline：直接在浏览器中打开Object。

如需确保通过浏览器访问图片或网页文件时是预览行为，除设置**Content-Disposition**为**inline**外，您还必须使用Bucket绑定的自定义域名进行访问。有关绑定自定义域名操作，请参见[绑定自定义域名](/cn.zh-CN/控制台用户指南/存储空间管理/传输管理/绑定自定义域名.md)。

    -   attachment：将Object下载到本地。

增加`filename`参数可预设Object保存在本地的文件名。例如`attachment; filename="example.jpg"`。

关于**Content-Disposition**的更多取值说明，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 |
    |**Cache-Control**|指定Object的缓存配置。常见取值如下：    -   no-cache：Object允许被缓存在客户端或代理服务器的浏览器中，但每次访问时需要向OSS验证缓存是否可用。缓存可用时直接访问缓存，缓存不可用时重新向OSS请求。
    -   no-store：不允许缓存Object。
    -   public：允许在响应经过的任何位置缓存Object，例如代理服务器、客户端等。
    -   private：只允许在客户端浏览器中缓存Object。
关于**Cache-Control**的更多取值说明，请参见[RFC2616](https://www.ietf.org/rfc/rfc2616.txt)。 |
    |**Expires**|指定Object缓存时间，取值为GMT时间。例如**Fri, 22 Jan 2021 07:28:00 GMT**。|
    |**用户自定义元数据**|用于为Object添加描述信息。您可以添加多条自定义元信息（User Meta），但所有的自定义元信息总大小不能超过8 KB。添加自定义元信息时，要求参数以`x-oss-meta-`为前缀，并为参数赋值，例如**x-oss-meta-location:hangzhou**。|

6.  单击**确定**。


