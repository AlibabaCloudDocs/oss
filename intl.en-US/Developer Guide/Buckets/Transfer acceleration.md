# Transfer acceleration

OSS uses data centers distributed around the globe to implement transfer acceleration. When a request is sent to your bucket, it is resolved and routed to the data center where the bucket resides over the most optimal network path and protocol. The transfer acceleration feature provides an optimized end-to-end acceleration solution to access OSS over the Internet.

## Scenarios

OSS transfer acceleration can be used in the following scenarios to accelerate access to OSS and improve user experience:

-   Accelerate remote data transfer

    For example, forums and online collaboration tools that provide services to users all across the globe can store data in OSS. However, upload and download speeds vary from region to region, delivering inconsistent user experience. OSS transfer acceleration can be used to allow users from different regions to access data in OSS from their regions over the most optimal network path. This accelerates data transfer and improves access experience for users across different regions.

-   Accelerate the uploads and downloads of gigabyte- and terabyte-grade objects

    When large objects are uploaded or downloaded over long geographical distances, transmission failures may occur due to high network latency. Transfer acceleration uses optimal route selection, protocol stack tuning, and transmission algorithms to reduce timeouts during remote transfer of large objects over the Internet. You can combine transfer acceleration with [Multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) and [Resumable download](/intl.en-US/Developer Guide/Objects/Download files/Resumable download.md) to develop a solution to upload and download large objects over long distances.

-   Accelerate non-static and cold data download

    For example, user experience is a driving factor for product competitiveness and customer retention in applications that require high data download speeds, such as photo management applications, games, e-commerce applications, enterprise portal websites, and financial applications. Obtaining comments on social networking applications also requires high download speeds. OSS transfer acceleration is a service designed to accelerate OSS uploads and downloads. You can enable transfer acceleration to maximize bandwidth utilization and accelerate data transfer.


## Accelerate endpoint types

After you enable transfer acceleration for a bucket, you can access the bucket by using the following two endpoints in addition to the default endpoint.

-   Global accelerate endpoint: `oss-accelerate.aliyuncs.com`. Transfer acceleration access points are distributed across the world. You can use this endpoint to accelerate data transfer for buckets in all regions.
-   Accelerate endpoint of regions outside mainland China: `oss-accelerate-overseas.aliyuncs.com`. Transfer acceleration access points are distributed across regions outside mainland China. This endpoint can be used to accelerate data transfer across buckets in the China \(Hong Kong\) region and regions outside mainland China.

In this example, ossutil is used to test the actual acceleration effect. The following figure shows the test results.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2295688951/p57059.png)

You can compare the access speeds of different regions when accelerate and default endpoints are used. For more information, see [The Comparison of OSS Direct Data Transfer and Accelerated data Transfer in Different Regions](https://oss-accelerate-test.oss-accelerate.aliyuncs.com/acc/oss-transfer-acc.html).

## Implementation methods

For more information about how to enable transfer acceleration, see [Enable transfer acceleration](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Enable transfer acceleration.md). When transfer acceleration is enabled, you can use an accelerate endpoint to implement transfer acceleration by using one of the following methods:

-   Browser

    When you use a browser to access data stored in OSS, replace the endpoint in the object URL with an accelerate endpoint. For example, replace `https://test.oss-cn-shenzhen.aliyuncs.com/myphoto.jpg` with `https://test.oss-accelerate.aliyuncs.com/myphoto.jpg`. If the ACL of the object you want to access is private, you sign the access request.

    **Note:** To accelerate access to a bucket with transfer acceleration enabled and a custom domain name bound, configure CNAME to map your custom domain name to an accelerate endpoint. For more information about how to configure CNAME, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

-   ossutil

    When you use ossutil to access data stored in OSS, replace the endpoint specified in the configuration file with an accelerate endpoint. You can also add the `-e oss-accelerate.aliyuncs.com` option to each command to manage OSS. The following figure shows an example on how to specify an accelerate endpoint in a command in ossutil.

    ![ossu](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8532654161/p208891.png)

    For more information about how to configure the endpoint when you use ossutil, see [ossutil](/intl.en-US/Tools/ossutil/Common commands/config.md).

-   ossbrowser

    When you use ossbrowser to access data stored in OSS, set the endpoint to Customize, and specify an accelerate endpoint. For more information about how to configure the endpoint when you use ossbrowser, see [ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md).

-   SDK

    When you use SDKs for different programming languages to access OSS, set the endpoint parameter to an accelerate endpoint. The following code provides examples on how to specify an accelerate endpoint when you use OSS SDK for Java to perform simple upload and download:

    -   Simple upload

        ```
        // Specify an accelerate endpoint. In this example, the global accelerate endpoint is used.
        String endpoint = "https://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "yourAccessKeyId";
        String accessKeySecret = "yourAccessKeySecret";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Upload the object to OSS. yourLocalFile consists of the local file path and the file name. Example: /users/local/myfile.txt.
        ossClient.putObject("yourBucketName", "yourObjectName", new File("yourLocalFile"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    -   Simple download

        ```
        // Specify an accelerate endpoint. In this example, the global accelerate endpoint is used.
        String endpoint = "https://oss-accelerate.aliyuncs.com";
        // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
        String accessKeyId = "yourAccessKeyId";
        String accessKeySecret = "yourAccessKeySecret";
        // Specify the bucket name.
        String bucketName = "yourBucketName";
        // Specify the complete name of the object excluding the bucket name. Example: testfolder/exampleobject.txt.
        String objectName = "yourObjectName";
        
        // Create an OSSClient instance.
        OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        
        // Download the object to a local file in the specified path. If the specified local file exists, it is overwritten by the downloaded object. Otherwise, the local file is created.
        // By default, if yourLocalFile does not include the file path, the downloaded object is stored in the relative path of the project to which the sample program belongs.
        ossClient.getObject(new GetObjectRequest(bucketName, objectName), new File("yourLocalFile"));
        
        // Shut down the OSSClient instance.
        ossClient.shutdown();
        ```

    For more information about SDK examples, see [Introduction](/intl.en-US/SDK Reference/Introduction.md).


## Usage notes

-   You must complete real-name registration on the [Real-name Registration](https://account-intl.console.aliyun.com/#/intlAuth) page before you can enable transfer acceleration for a bucket.
-   Transfer acceleration takes effect within 30 minutes after it is enabled.
-   Transfer acceleration is applied only when you use an accelerate endpoint to access a bucket that has transfer acceleration enabled.
-   When you use an accelerate endpoint, you can manage only buckets that have transfer acceleration enabled.
-   After you enable transfer acceleration for a bucket, other endpoints of the bucket remain available. In scenarios where transfer acceleration is not required, you can use the default endpoint to avoid unnecessary charges.
-   To ensure data security, the protocol used in transmission may be changed from HTTP to HTTPS after the peer end receives the request. Therefore, when the client uses an accelerate endpoint to access OSS over HTTP, the protocol recorded in access logs may be HTTPS.
-   Fees are charged when you use transfer acceleration. For more information, see [Transfer acceleration fees](/intl.en-US/Pricing/Billing items and methods/Transfer acceleration fees.md).

## FAQ

-   What are the differences between transfer acceleration and Alibaba Cloud CDN?

    |Feature|How it works|Scenario|
    |-------|------------|--------|
    |Transfer acceleration|Transfer acceleration provides an end-to-end acceleration solution by combining smart scheduling, protocol stack tuning, optimal route selection, and transfer algorithm optimization with OSS server-side configurations.|    -   Accelerate object uploads.
    -   Accelerate remote object uploads and downloads.
    -   Accelerate the uploads and downloads of large objects.
    -   Accelerates dynamic object updates and non-hotspot object downloads. |
    |Alibaba Cloud CDN|Alibaba Cloud CDN uses OSS buckets as origins and distributes the content from the origins to edge nodes. When you read data, the cached object is obtained from the closest node to accelerate downloads.|Accelerates downloads when a large number of users in the same region concurrently download the same static file.|

-   How do I implement transfer acceleration based on a custom domain name?

    After you bind a custom domain name to a bucket, you can map the CNAME to an accelerate endpoint. For more information, see [Bind accelerate endpoints](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind accelerate endpoints.md).

-   Why am I unable to list buckets by using an accelerate endpoint?

    Transfer acceleration is applicable only to third-level domain names that contain a specific bucket name, such as `https://BucketName.oss-accelerate.aliyuncs.com`. The domain name in a ListBuckets request does not contain a bucket name. Therefore, accelerate endpoints cannot be used to list buckets. To list buckets in a specific region, we recommend that you use the default endpoint of the region, such as `https://oss-cn-hangzhou.aliyuncs.com`.


