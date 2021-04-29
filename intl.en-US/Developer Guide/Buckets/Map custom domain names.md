# Map custom domain names

After you upload objects to a bucket, OSS automatically generates URLs for the uploaded objects. You can use these URLs to access the objects. If you want to access the objects by using custom domain names, you must map the custom domain names to the bucket in which the objects are stored and add CNAME records for the custom domain names.

## Usage notes

-   In accordance with the administrative regulations of the People's Republic of China, the custom domain names that you want to map to OSS buckets in mainland China regions must be filed with the Ministry of Industry and Information Technology \(MIIT\) in advance.

    For more information about how to file a domain name, see [Alibaba Cloud ICP Filing system](https://beian.aliyun.com/order/selfBaIndex.htm).

-   Up to 100 domain names can be mapped to a bucket. One domain name can be mapped to only one bucket. OSS does not impose limits on the number of domain names that an Alibaba Cloud account can map to buckets.
-   When you map a custom domain name to a bucket in the OSS console, the domain name cannot contain wildcards. When you map an accelerated domain name to a bucket, the domain name can contain wildcards but is not displayed in the OSS console.

## Scenarios

-   When you use a browser to access image objects in buckets created in mainland China regions after September 23, 2019, make sure that the browser accesses the objects by previewing but not downloading.
-   When you access a bucket for which static website hosting is configured, make sure that you do not access the bucket by downloading.

## Implementation methods

You can map custom domain names to a bucket in the OSS console. For more information, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

## Usage notes

For example, a public read object named exampleobject.jpg is stored in the root folder of the bucket named examplebucket in the China \(Hangzhou\) region. A custom domain name `www.example.com` is mapped to the bucket. You can access the object by using different URLs before and after the custom domain name is mapped to the bucket.

-   Before the custom domain name is mapped to the bucket

    You can use the following URL that contains the default domain name of the bucket to access the object exampleobject.jpg: `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/exampleobject.jpg`.

-   After the custom domain name is mapped to the bucket

    You can use the following URL that contains the custom domain name mapped to the bucket to access the object exampleobject.jpg: `https://www.example.com/exampleobject.jpg`.


## References

-   For more information about how to map an accelerate endpoint, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).
-   For more information about how to access OSS resources as a static website, see [Configure static website hosting](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure static website hosting.md) and [Tutorial: Configure static website hosting by using a custom domain name](/intl.en-US/Developer Guide/Static website hosting/Tutorial: Configure static website hosting by using a custom domain name.md).
-   For more information about how to access OSS by using HTTPS, see [Host SSL certificates](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Host SSL certificates.md).

