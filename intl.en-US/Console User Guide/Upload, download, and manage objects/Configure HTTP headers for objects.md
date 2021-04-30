# Configure HTTP headers for objects

You can configure HTTP headers to customize HTTP request policies, such as the cache policy and forced object download policy.

You can configure HTTP headers for up to 100 objects at a time in the OSS console. To configure HTTP headers for more than 100 objects at a time, use ossutil. For more information, see [ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that contains the object for which you want to configure HTTP headers.

3.  In the left-side navigation pane, click **Files**.

4.  Use one of the following methods to open the Set HTTP Header panel:

    -   Configure HTTP headers for one or more objects

        Select one or more objects. Choose **Batch Operation** \> **Set HTTP Header**.

    -   Configure HTTP headers for a single object

        Click **View Details** in the Actions column corresponding to the object. In the View Details panel, click **Configure** in the header section.

5.  Set the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Content-Type**|The type of the object. The browser determines the default opening method of an object based on the object type. For example, the Content-Type value of a GIF image is **image/gif**.For more information about how to set Content-Type for different types of objects, see [How do I configure the Content-Type of objects?](/intl.en-US/Developer Guide/Objects/FAQ/How do I configure the Content-Type of objects?.md) |
    |**Content-Encoding**|The encoding method of the object. You must set this parameter based on the encoding type of the object. Otherwise, the client \(browser\) may fail to parse the encoding type of the object, or the object may fail to be downloaded. If the object is not encoded, leave this parameter empty. Default value: identity. Valid values:    -   gzip: OSS uses the Lempel-Ziv coding \(LZ77\) compression algorithm and 32-bit CRC to encode the object.
    -   compress: OSS uses the Lempel-Ziv-Welch \(LZW\) compression algorithm to encode the object.
    -   deflate: OSS uses the zlib library and the deflate algorithm to encode the object.
    -   identity: OSS does not encode the object.
    -   br: OSS uses the Brotli algorithm to encode the object.
For more information about **Content-Encoding**, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt).

**Note:** If you want the static web page objects, such as HTML, JavaScript, XML, and JSON objects to be compressed into GZIP objects when you access them, you must leave this parameter empty and add the `Accept-Encoding: gzip` header to your request. For more information, see [How do I compress objects downloaded from OSS into GZIP files?](/intl.en-US/Developer Guide/Objects/FAQ/How to use OSS gzip?.md) |
    |**Content-Language**|The language of the object content. For example, if the content of an object is written in simplified Chinese, you can set this parameter to **zh-CN**.|
    |**Content-Disposition**|The method used to access the object. Valid values:    -   inline: The object is opened in the browser.

To ensure that an image object or a web page object is previewed but not downloaded when the object is accessed, you must set **Content-Disposition** to **inline** and use the custom domain name bound to the bucket to access the object. For more information about how to bind custom domain names, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

    -   attachment: The object is downloaded to a local file.

You can use the additional `filename` parameter to specify the name of the local file to which the object is downloaded. Example: `attachment; filename="example.jpg"`.

For more information about the valid values of **Content-Disposition**, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). |
    |**Cache-Control**|The cache configurations for the object. Valid values:    -   no-cache: The object can be cached on the client or on the browser of the proxy server. However, each time you access the object, the cache must be verified by OSS before it can be used. If the cache is available, you can directly access the cache. If the cache is unavailable, the access request is sent to OSS.
    -   no-store: The object cannot be cached.
    -   public: The object can be cached on each node in the route in which the response is returned, such as the proxy server and the client.
    -   private: The object can be cached only on the browser of the client.
For more information about the valid values of **Cache-Control**, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). |
    |**Expires**|The time before which the cache is valid. The value of this parameter is a GMT timestamp. Example: **Fri, 22 Jan 2021 07:28:00 GMT**.|
    |**User Metadata**|The user-defined metadata of the object. You can add multiple user metadata headers for an object. However, the total size of user metadata cannot exceed 8 KB. To add a user metadata header, you must prefix the header name with `x-oss-meta-` and specify a value for the header. Example: **x-oss-meta-location:hangzhou**.|

6.  Click **OK**.


