# Manage object metadata

Object metadata describes the attributes of objects uploaded to OSS. These attributes include standard HTTP attributes \(HTTP headers\) and user metadata.

## HTTP headers

|Header|Description|
|------|-----------|
|Content-Type|The content type of the object. The browser determines the format and encoding type used to read the object based on the content type of the object. If this attribute is not specified, the value is generated based on the extension of the object. If the object does not have an extension, the default value `application/octet-stream` is used as the content type of the object.|
|Content-Encoding|The encoding method of the object. Default value: identity.-   identity: The object is not compressed.
-   gzip: The Lempel-Ziv coding \(LZ77\) compression algorithm and 32-bit CRC is used to encode the object. |
|Content-Language|The language of the object.|
|Content-Disposition|The method in which the object is accessed.Valid values:

-   inline: The object is previewed when you access the object.
-   attachment: The object is downloaded to a local file when you access the object. For example, if this header is set to `attachment; filename="example.jpg"`, the object is downloaded to a local file named `example.jpg` when you access the object.

**Note:** When you access an object in OSS by using a browser, the object is downloaded even if the Content-Disposition header is set to inline in the following scenarios:

-   The object is a web page object and is not accessed by using the custom domain name bound to the bucket.
-   The object is an image and the bucket in which the object is stored is created after September 23, 2019. In addition, the object is not accessed by using the custom domain name bound to the bucket.
-   The browser does not support previewing the object. |
|Cache-Control|The caching behavior for the object.-   no-cache: The cache cannot be used before it is verified by the server. If the object is updated, the cache expires. The object must be downloaded again from the server. If the object is not updated, the cache does not expire, and the local cache is used.
-   no-store: All content of the object is not cached.
-   public: All content of the object is cached.
-   private: All content of the object is cached only in the client.
-   max-age=<seconds\>: The validity period of the cache. Unit: second. This option is available only in HTTP 1.1. |
|Expires|The expiration time of the cache in GMT. Example: `2022-10-12T00:00:00.000Z`. If `max-age=<seconds>` is set for Cache-Control, `max-age=<seconds>` takes precedence over Expires.|
|Last-Modified|The time when the object is last modified.|
|Content-Length|The size of the object.|

## User metadata

In OSS, parameters whose names are prefixed with `x-oss-meta-` are user metadata. Example: `x-oss-meta-location`.

-   An object may have multiple similar parameters. However, the total size of the user metadata of an object cannot exceed 8 KB.
-   When you call the GetObject or HeadObject operation, the user metadata of the object is returned as response headers.

## Implementation methods

The following table describes the methods that you can use to configure, query, and modify the metadata of objects.

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object HTTP headers.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md)|A high-performance command-line tool|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)|SDK demos for various programming languages|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Manage objects/Manage object metadata.md)|
|[OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Manage objects/Manage object metadata.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Manage objects/Manage object metadata.md)|
|[OSS SDK for C++](/intl.en-US/SDK Reference/C++/Manage objects/Manage Object Meta.md)|
|[OSS SDK for C](/intl.en-US/SDK Reference/C/Manage objects/Manage Object Meta.md)|
|[OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Manage objects/Manage Object Meta.md)|
|[OSS SDK for Android](/intl.en-US/SDK Reference/Android/Manage objects/Obtain the metadata of an object.md)|

