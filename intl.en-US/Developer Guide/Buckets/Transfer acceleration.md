# Transfer acceleration

Alibaba Cloud OSS provides transfer acceleration to improve data transfer speeds. Transfer acceleration enables enterprises to deploy their business in more Alibaba Cloud regions and improve user experience. OSS provides transfer acceleration to accelerate the upload and download of OSS objects over the Internet. Transfer acceleration provides an end-to-end acceleration solution by combining smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations.

OSS uses data centers distributed around the globe to implement transfer acceleration. When a data transfer request is sent, it is resolved and routed over an optimal network path and protocol to the data center where your bucket resides.

## Scenarios

OSS transfer acceleration can be used in the following scenarios to accelerate access and improve user experience:

-   Accelerates remote data transfer

    For example, forums and online collaboration tools that have users all across the globe can store data in OSS. However, upload and download speeds vary from region to region, which results in inconsistent access experiences. In this case, OSS transfer acceleration can be used to allow users from different regions to transfer data over the optimal network based on their regions. This feature accelerates data transfer and improves access experience for users across different regions.

-   Accelerates uploads and downloads of objects of gigabytes or terabytes in size

    When large objects are uploaded or downloaded over the Internet over a long distance, transmission failures may occur due to high network latency. Transfer acceleration uses optimal route selection, protocol stack tuning, and transmission algorithms to reduce timeouts during remote transfer of large objects over the Internet. You can combine transfer acceleration with [Multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) and [Resumable download](/intl.en-US/Developer Guide/Objects/Download files/Resumable download.md) to develop a solution to upload and download large objects over long distances.

-   Accelerates non-static and non-hotspot data download

    For example, user experience is a driving factor for product competitiveness and customer retention in applications that require high data download speeds, such as photo management applications, games, e-commerce applications, enterprise portal websites, and financial applications. Obtaining comments on social networking applications also requires high download speeds. OSS transfer acceleration is a service designed to accelerate OSS uploads and downloads. You can enable transfer acceleration to maximize bandwidth utilization and accelerates data transfer.


## Usage notes

When transfer acceleration is enabled for a bucket, two public endpoints become available for the bucket.

-   The following accelerate endpoints are available:

    -   Accelerate endpoint: `oss-accelerate.aliyuncs.com`. Transfer acceleration access points are distributed all over the world. You can use this endpoint to accelerate data transfer for buckets in all regions.
    -   Accelerate endpoint of regions outside mainland China: `oss-accelerate-overseas.aliyuncs.com`. Transfer acceleration access points are distributed across regions outside mainland China. This endpoint can be used to accelerate transfers across buckets in the China \(Hong Kong\) region and regions outside mainland China.
    We recommend that you use accelerate endpoints outside mainland China only when you need to bind a custom domain name for which you have not applied for an ICP filing to a bucket in the China \(Hong Kong\) region or regions outside China, and use the global accelerate endpoint in other regions. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

-   Regular endpoint: The format is `oss-regionid.aliyuncs.com`. Use this endpoint if your access requests do not need transfer acceleration. For more information about the default endpoint, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

**Note:** We recommend that you use the OSS internal endpoint to access an OSS bucket from an ECS instance in the same region. For example, a bucket and an ECS instance are located in the China \(Shanghai\) region. We recommend that you use `oss-cn-shanghai-internal.aliyuncs.com` to access OSS from the ECS instance.

In this example, ossutil is used to test the actual acceleration effect. The following figure shows the results.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2295688951/p57059.png)

You can compare the speeds of access to different regions when the accelerate and regular endpoints are used. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).

## Implementation methods

For more information about how to enable OSS transfer acceleration, see [Enable transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Enable transfer acceleration.md). When transfer acceleration is enabled, you can use the global accelerate endpoint to implement transfer acceleration by using one of the following methods:

-   Browser

    When you use a browser to access data stored in OSS, replace the endpoint in the object URL with an accelerate endpoint. For example, replace `https://test.oss-cn-shenzhen.aliyuncs.com/myphoto.jpg` with `https://test.oss-accelerate.aliyuncs.com/myphoto.jpg`. If the object ACL is private, you must add signature information to the object URL.

    **Note:** To accelerate access to a bucket with transfer acceleration enabled and a custom domain name bound, configure CNAME to map your custom domain name to the accelerate endpoint. For more information about how to configure CNAME, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

-   ossutil

    When you use ossutil to access data stored in OSS, replace the endpoint in the configuration file with an accelerate endpoint. Alternatively, add `-e oss-accelerate.aliyuncs.com` to each command to perform an operation. The following figure shows an example on how to configure the endpoint.

    ![ossu](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8532654161/p208891.png)

    For more information about how to configure the endpoint when you use ossutil, see [ossutil](/intl.en-US/Tools/ossutil/Common commands/config.md).

-   ossbrowser

    When you use ossbrowser to access data stored in OSS, set the endpoint to Customize, and specify an accelerate endpoint. The following figure shows an example on how to configure the endpoint. For more information about how to configure the endpoint when you use ossbrowser, see [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md).

-   SDK

    When you use SDKs for different languages to access OSS, set the endpoint to an accelerate endpoint. The following code provides an example on how to use OSS SDK for Java to perform simple upload and download:

    -   Simple upload

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Upload the object to OSS. <yourLocalFile> consists of the local file path and a file name. Example: /users/local/myfile.txt.
        ossClient.putObject("<yourBucketName>", "<yourObjectName>", new File("<yourLocalFile>"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    -   Simple download

        ```
        // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
        String endpoint = "http://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "<yourAccessKeyId>";
        String accessKeySecret = "<yourAccessKeySecret>";
        String bucketName = "<yourBucketName>";
        String objectName = "<yourObjectName>";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Download the object to a local file.
        ossClient.getObject(new GetObjectRequest(bucketName, objectName), new File("<yourLocalFile>"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    For more information about SDK examples, see [Introduction](/intl.en-US/SDK Reference/Introduction.md).


## Usage notes

-   You must complete real-name registration on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page before you enable transfer acceleration for a bucket.
-   When you enable or disable transfer acceleration, the configurations take effect in 30 minutes.
-   Access is accelerated if you use the accelerate endpoint to access a bucket that has transfer acceleration enabled.
-   If the endpoint of your tool is set to an accelerate endpoint, you can perform operations only on the bucket that has transfer acceleration enabled.
-   After transfer acceleration is enabled, other endpoints provided by OSS remain available. In scenarios where transfer acceleration is not required, you can use the default endpoint to reduce transfer acceleration fees.
-   To ensure data transfer security, HTTP used in the actual transfer endpoint is replaced with HTTPS when the peer end receives the request. Therefore, when the client uses an accelerate endpoint to access OSS over HTTP, the OSS log may record the HTTPS protocol.
-   Fees are charged for the use of transfer acceleration. For more information about usage fees, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

## FAQ

-   What are the differences between transfer acceleration and Alibaba Cloud CDN?

    |Description|How it works|Scenarios|
    |-----------|------------|---------|
    |Transfer acceleration|Transfer acceleration combines smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations to provide an end-to-end acceleration solution.|    -   Accelerates object uploads.
    -   Accelerates remote object uploads and downloads.
    -   Accelerates uploads and downloads of large objects.
    -   Accelerates dynamic object updates and non-hotspot object downloads. |
    |Alibaba Cloud CDN|Alibaba Cloud CDN uses OSS buckets as origins and distributes the content from the origins to edge nodes. When you read data, the cached object is obtained from an optimal node to accelerate downloads.|Accelerates downloads when a large number of users in the same region concurrently download the same static file.|

-   How do I implement transfer acceleration based on a custom domain name?

    After you bind a custom domain name to OSS, you can map the CNAME to the accelerate endpoint. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).


