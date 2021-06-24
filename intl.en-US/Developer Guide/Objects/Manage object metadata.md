# Manage object metadata

Object Storage Service \(OSS\) uses object metadata to describe object attributes. Object metadata includes standard HTTP headers and user metadata. You can configure HTTP headers to customize HTTP request policies, such as cache policies and policies for forced object download. You can also configure user metadata to identify the purposes or attributes of objects.

## Standard HTTP headers

OSS retains standard HTTP headers for each object that is uploaded to a bucket. The following table describes the standard HTTP headers.

|Header|Description|
|------|-----------|
|Content-Type|The content type of the object. The browser determines the format and encoding type that are used to read the object based on the content type of the object. If this attribute is not specified, the value is generated based on the extension of the object name. If the object name does not have an extension, the default value `application/octet-stream` is used as the content type of the object.|
|Content-Encoding|The encoding method of the object. You must set this parameter based on the encoding type of the object. Otherwise, the browser that serves as the client may fail to parse the encoding type of the object, or the object may fail to be downloaded. If the object is not encoded, leave this parameter empty. Default value: identity. Valid values:-   identity: OSS does not compress or encode the object.
-   gzip: OSS uses the LZ77 compression algorithm created by Lempel and Ziv in 1977 and 32-bit cyclic redundancy check \(CRC\) to encode the object.
-   compress: OSS uses the Lempel–Ziv–Welch \(LZW\) compression algorithm to encode the object.
-   deflate: OSS uses the zlib library and the deflate algorithm to encode the object.
-   br: OSS uses the Brotli algorithm to encode the object. |
|Content-Language|The language of the object content.|
|Content-Disposition|The method used to access the object. Valid values:-   inline: The object is previewed when you access the object.
-   attachment: The object is downloaded to the local computer when you access the object. For example, if this header is set to `attachment; filename="example.jpg"`, the object is downloaded to the local computer. After the object is downloaded, the local file is named `example.jpg`.

**Note:** When you use a browser to access an object in OSS, the object is downloaded even if the Content-Disposition header is set to inline in the following scenarios:

-   The object is a web page object and is not accessed by using the custom domain name that is mapped to the bucket.
-   The object is an image, and the bucket in which the object is stored is created after September 23, 2019. In addition, the object is not accessed by using the custom domain name that is mapped to the bucket.
-   The object cannot be previewed by using the browser. |
|Cache-Control|The caching behavior for the object. Valid values:-   no-cache: Each time you access a cached object, the server checks whether the object is updated. If the object is updated, the cache expires. The object must be downloaded from the server again. If the object is not updated, the cache does not expire, and you can directly use the cached object.
-   no-store: All content of the object is not cached.
-   public: All content of the object is cached.
-   private: All content of the object is cached only on the client.
-   max-age=<seconds\>: the validity period of the cached content. Unit: seconds. This option is available only in HTTP/1.1. |
|Expires|The absolute expiration time of the cache in Greenwich Mean Time \(GMT\). Example: `2022-10-12T00:00:00.000Z`. If `max-age=<seconds>` is set for Cache-Control, `max-age=<seconds>` takes precedence over Expires.|
|Last-Modified|The time when the object is last modified.|
|Content-Length|The size of the object. Unit: byte.|

## User metadata

When you upload an object, you can add user metadata to identify the purposes or attributes of the object.

-   You can configure multiple user metadata parameters for an object. However, the total size of the user metadata of an object cannot exceed 8 KB.
-   User metadata is a set of key-value pairs. The name of a user metadata header must start with `x-oss-meta-`. Example: `x-oss-meta-last-modified:20210506`, which indicates that the local file was last modified on May 6, 2021.
-   When you call the GetObject operation or the HeadObject operation, the user metadata of the object is returned as HTTP headers.

## Implementation methods

The following table describes the methods that you can use to configure, query, and modify the metadata of objects.

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure object metadata.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md)|An easy-to-use graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/set-meta.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Manage objects/Manage Object Meta.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Manage objects/Manage object metadata.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Manage objects/Manage object metadata.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Manage objects/Manage object metadata.md)|
|[C++ SDK](/intl.en-US/SDK Reference/C++/Manage objects/Manage Object Meta.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Manage objects/Manage Object Meta.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Manage objects/Manage Object Meta.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Manage objects/Manage Object Meta.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Manage objects/Obtain the metadata of an object.md)|

