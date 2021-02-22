# Configure object HTTP headers

You can configure HTTP headers to customize HTTP request policies such as the cache policy and forced object download policy.

You can configure HTTP headers for up to 100 objects at a time. To configure HTTP headers for multiple objects at a time, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md) and [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage files/Manage object metadata.md).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  Click the **Files** tab.

4.  Use any of the following methods to open the Set HTTP Header dialog box:

    -   Configure HTTP headers for one or more objects: Select one or more objects. Choose **Batch Operation** \> **Set HTTP Header**.
    -   Configure HTTP headers for a single object: Click **View Details** in the Actions column corresponding to the object. In the View Details dialog box that appears, click **Set HTTP Header**.
    -   Configure HTTP headers for a single object: Choose **More** \> **Set HTTP Header** in the Actions column corresponding to the object.
5.  Set related parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Content-Type**|The type and encoding method of the object.|
    |**Content-Encoding**|The compression type of the object. Valid values:    -   gzip: uses the Lempel-Ziv coding \(LZ77\) compression algorithm with 32-bit CRC verification to compress objects.
    -   compress: uses the Lempel-Ziv-Welch \(LZW\) compression algorithm to compress objects.
    -   deflate: uses the zlib library and the deflate algorithm to compress objects.
    -   identity: does not compress objects. This is the default value of Content-Encoding.
    -   br: uses the Brotli algorithm to compress objects. |
    |**Content-Language**|The language of the object content.|
    |**Content-Disposition**|The method in which the object is accessed. Valid values:    -   inline: the object is opened in applications, such as a browser.
    -   attachment: the object is downloaded to a local file. You can set the `filename` parameter to specify the name of the local file to which the object is downloaded. Example: `attachment; filename="example.jpg"`.
**Note:** In the following scenarios, when you access an object through a browser, the object is downloaded to a local file even if Content-Disposition is set to inline:

    -   The object is a web page file and is accessed through a domain name that is not a custom domain name bound to the bucket.
    -   The object is an image that is created after September 23, 2019 and is accessed through a domain name that is not a custom domain name bound to the bucket.
    -   The format of the object is not supported by the browser.
For more information, see [Download objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Download objects.md). |
    |**Cache-Control**|The cache configurations for the object. Valid values:    -   no-cache: the cache cannot be used before it is verified by the server.
    -   no-store: no cache is used.
    -   public: the object can be cached on each node in the route in which the response is returned.
    -   private: the object can be cached only on the browser.
    -   max-age=<seconds\>: the caching time. Unit: second.
Example: `public, max-age=20` indicates that the object is cached for 20 seconds.|
    |**Expires**|The time before which the cache is valid. The value of this parameter is a GMT timestamp. If `max-age=<seconds>` is set for **Cache-Control**, `max-age=<seconds>` takes precedence over Expires.|
    |**User metadata**|User metadata allows you to describe objects in details. In OSS, all parameters prefixed with `x-oss-meta-` are considered as user metadata, such as `x-oss-meta-location`. The total size of all user metadata cannot exceed 8 KB.|

    **Note:** For more information about the parameters, see [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers).

6.  Click **OK**.


