# Bind custom domain names

After you upload objects to a bucket, OSS automatically generates URLs for the uploaded objects. You can use these URLs to access the objects. If you want to access the objects by using custom domain names, you must bind the custom domain names to the bucket in which the objects are stored and add CNAME records.

## Limits

-   In accordance with the administrative regulations of the People's Republic of China, you must file the custom domain names that you want to bind to OSS buckets in mainland China regions at the Ministry of Industry and Information Technology \(MIIT\) in advance.

    For more information about how to file a domain name, see [Alibaba Cloud ICP Filing system](https://beian.aliyun.com/order/selfBaIndex.htm).

-   Up to 100 domain names can be bound to a bucket. One domain name can be bound to only one bucket. OSS does not impose limits on the number of domain names that an Alibaba Cloud account can bind to buckets.
-   When you bind a custom domain name to a bucket in the OSS console, the domain name cannot contain wildcards. When you bind an accelerated domain name to a bucket, the domain name can contain wildcards but is not displayed in the OSS console.

## Scenarios

-   When you use a browser to access image objects in buckets created in mainland China regions after September 23, 2019, make sure that the browser accesses the objects by previewing but not downloading.
-   When you access a bucket for which static website hosting is configured, make sure that you do not access the bucket by downloading.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md)|A user-friendly and intuitive web application|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Buckets/Bind a custom domain name.md)|SDK demos for various programming languages|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Buckets/CNAME.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/CNAME.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Buckets/CNAME.md)|

## Usage notes

For example, a public read object named exampleobject.jpg is stored in the root folder of the bucket named examplebucket in the China \(Hangzhou\) region. A custom domain name `www.example.com` is bound to the bucket. You can access the object by using different URLs before and after the custom domain name is bound to the bucket.

-   Before the custom domain name is bound to the bucket

    You can use the following URL that contains the default domain name of the bucket to access the object exampleobject.jpg: `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/exampleobject.jpg`.

-   After the custom domain name is bound to the bucket

    You can use the following URL that contains the custom domain name bound to the bucket to access the object exampleobject.jpg: `https://www.example.com/exampleobject.jpg`.


## References

-   For more information about how to bind an accelerate endpoint, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).
-   For more information about how to access OSS resources as a static website, see [Configure static website hosting](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure static website hosting.md) and [Tutorial: Configure static website hosting by using a custom domain name](/intl.en-US/Developer Guide/Static website hosting/Tutorial: Configure static website hosting by using a custom domain name.md).
-   For more information about how to access OSS by using HTTPS, see [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md).

