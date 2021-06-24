# Configure object metadata

Object metadata describes the attributes of objects uploaded to Object Storage Service \(OSS\). These attributes include standard HTTP attributes \(HTTP headers\) and user metadata.

You can configure object metadata for up to 100 objects at a time in the OSS console. To configure object metadata for more than 100 objects at a time, use [ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that contains the object for which you want to configure object metadata.

3.  In the left-side navigation pane, click **Files**.

4.  Use one of the following methods to open the Set HTTP Header panel:

    -   Configure HTTP headers for one or more objects

        Select one or more objects. Choose **Batch Operation** \> **Set HTTP Header**.

    -   Configure HTTP headers for a single object

        Find the object for which you want to configure HTTP headers and choose **More** \> **Set HTTP Header** in the Actions column.

5.  In the Set HTTP Header panel, configure the parameters. The following table describes the parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Content-Type**|The type of the object. The browser determines the default method used to open an object based on the object type. For example, the Content-Type value of a GIF image is image/gif. For more information about the Content-Type configurations for different object types, see [t5041.dita\#concept\_5041](/intl.en-US/Developer Guide/Objects/FAQ/How do I configure the Content-Type of objects?.md). |
    |**Content-Encoding**|The encoding method of the object. You must set this parameter based on the encoding type of the object. Otherwise, the browser that serves as the client may fail to parse the encoding type of the object, or the object may fail to be downloaded. If the object is not encoded, leave this parameter empty. Default value: identity. Valid values:    -   identity: OSS does not compress or encode the object.
    -   gzip: OSS uses the LZ77 compression algorithm created by Lempel and Ziv in 1977 and 32-bit cyclic redundancy check \(CRC\) to encode the object.
    -   compress: OSS uses the Lempel–Ziv–Welch \(LZW\) compression algorithm to encode the object.
    -   deflate: OSS uses the zlib library and the deflate algorithm to encode the object.
    -   br: OSS uses the Brotli algorithm to encode the object.
For more information about Content-Encoding, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt).

**Note:** If you want the static web page objects, such as HTML, JavaScript, XML, and JSON objects to be compressed into GZIP objects when you access these objects, you must leave this parameter empty and add the `Accept-Encoding: gzip` header to your request. For more information, see [How do I use GZIP for compression in OSS?](/intl.en-US/Developer Guide/Objects/FAQ/How to use OSS gzip?.md) |
    |**Content-Language**|The language of the object content. For example, if the content of an object is written in simplified Chinese, you can set this parameter to zh-CN.|
    |**Content-Disposition**|The method used to access the object. Valid values:    -   inline: The object is directly opened in the browser.

To ensure that an image object or a web page object is previewed but not downloaded when the object is accessed, you must set Content-Disposition to inline and use the custom domain name mapped to the bucket to access the object. For more information about how to map custom domain names, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

    -   attachment: The object is downloaded to the local computer. For example, if this header is set to `attachment; filename="example.jpg"`, the object is downloaded to the local computer. After the object is downloaded, the local file is named `example.jpg`.
For more information about Content-Disposition, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). |
    |**Cache-Control**|The cache configurations for the object. Valid values:    -   no-cache: The object can be cached on the client or on the browser of the proxy server. However, each time you access the object, OSS checks whether the cached object is available. If the cache is available, you can directly access the cache. Otherwise, the access request is sent to OSS.
    -   no-store: All content of the object is not cached.
    -   public: All content of the object is cached.
    -   private: All content of the object is cached only on the client.
For more information about Cache-Control, see [RFC 2616](https://www.ietf.org/rfc/rfc2616.txt). |
    |**Expires**|The expiration time of the cache in Greenwich Mean Time \(GMT\). Example: `2022-10-12T00:00:00.000Z`. If `max-age=<seconds>` is set for Cache-Control, `max-age=<seconds>` takes precedence over Expires.|
    |**User Metadata**|The user-defined metadata of the object. You can add multiple user metadata headers for an object. However, the total size of user metadata cannot exceed 8 KB. When you add user metadata, user metadata headers must be prefixed with `x-oss-meta-` and assigned values. Example: x-oss-meta-last-modified:20200909u.|

6.  Click **OK**.


