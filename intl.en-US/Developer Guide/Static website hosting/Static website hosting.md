# Static website hosting

Static websites are websites where all web pages consist only of static content, including scripts such as JavaScript code running on the client. You can use the static website hosting feature to host your static website in an Object Storage Service \(OSS\) bucket and use the endpoint of the bucket to access the website.

**Note:** For more information about the API operation used to configure static website hosting, see [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md).

## Usage notes

When you configure static website hosting, you must specify the default homepage and the default 404 page for the website.

-   The default homepage is displayed when you use a browser to access the static website hosted in OSS. The default homepage functions similar to index.html.

    The object that you specify as the default homepage must be an object within the root directory of the bucket that allows anonymous access. Only objects in the HTML format are supported.

-   The default 404 page is the error page returned by OSS when a 404 error occurs when you access objects in a bucket by using the browser.

    The object that you specify as the default 404 page must be an object within the root directory of the bucket that allows anonymous access. The following object formats are supported: HTML, JPG, PNG, BMP, and WebP.


When you access a website hosted on a bucket by using the default endpoint of the bucket, the static website is downloaded as an object to your local computer. To preview a static website, you must map a custom domain name to the bucket and access the static website by using the custom domain name. For more information about how to map a custom domain name, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

## Examples

The following figure provides an example of the structure of objects in a bucket:

```
Bucket
 ├── index.html
 ├── error.html
 ├── example.txt
 └── subdir/
      └── index.html
```

The custom domain name `example.com` is mapped to the bucket, the default homepage of the configured static website is index.html, and the default 404 page is error.html. The following rules apply when you access the static website by using the custom domain name:

-   When you access `https://example.com/` and `https://example.com/subdir/`, OSS returns `https://example.com/index.html`.
-   When you access `https://example.com/example.txt`, the example.txt object can be obtained if the object exists.
-   When you access `https://example.com/example.txt`, OSS returns `https://example.com/error.html` if the example.txt object does not exist.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure static website hosting.md)|A user-friendly and intuitive web application|
|[OSS SDK for Java](/intl.en-US/SDK Reference/Java/Buckets/Static website hosting.md)|SDK demos for various programming languages|
|[OSS SDK for Python](/intl.en-US/SDK Reference/Python/Buckets/Static website hosting.md)|
|[OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Buckets/Static website hosting.md)|
|[OSS SDK for Go](/intl.en-US/SDK Reference/Go/Buckets/Static website hosting.md)|
|[OSS SDK for C](/intl.en-US/SDK Reference/C/Buckets/Static website hosting.md)|
|[OSS SDKfor .NET](/intl.en-US/SDK Reference/. NET/Buckets/Static website hosting.md)|
|[OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Buckets/Static website hosting.md)|
|[OSS SDK for Ruby](/intl.en-US/SDK Reference/Ruby/Buckets/Static website hosting.md)|

## References

You can host a static website to an OSS bucket and allow users to access the website by using the custom domain name that is mapped to the bucket, such as example.com. For more information about how to attach a policy to a RAM role, see [Tutorial: Configure static website hosting by using a custom domain name](/intl.en-US/Developer Guide/Static website hosting/Tutorial: Configure static website hosting by using a custom domain name.md).

