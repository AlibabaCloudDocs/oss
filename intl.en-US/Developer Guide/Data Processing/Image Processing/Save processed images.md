# Save processed images

By default, Image Processing \(IMG\) does not save processed images. You must add the saveas parameter to an IMG request to save a processed image as an object to a specified bucket.

## Usage notes

-   Permission

    To save a processed image, you must have the `oss:PostProcessTask` permission on the source bucket in which the source image is stored, the `oss:PutBucket` permission on the destination bucket in which you want to store the processed image, and the `oss:PutObject` permission on the object as which you want to store the processed image.

-   Region

    You can save the processed image to the same bucket where the source image is stored or to a different bucket. However, the source bucket and the destination bucket must belong to the same Alibaba Cloud account and must be in the same region.

-   Storage method

    Images that are processed by using object URLs cannot be directly saved to a specified bucket. You can save the processed images to your local device and then upload them to the specified bucket.

-   ACL

    The access control list \(ACL\) of the processed image is the same as that of the bucket in which the image is saved and cannot be customized.

-   Storage duration

    If you want to store the processed image for the specific duration, configure a lifecycle rule for the image object to specify the time when the object expires. For more information, see [Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md).


## SDK

If you use Object Storage Service \(OSS\) SDKs to process images, you can use the ImgSaveAs operation to save the processed images to a specified bucket. The following code provides an example on how to use OSS SDK for Java to save a processed image to a specified bucket:

```
// Specify the endpoint of the region in which the specified bucket is located. In this example, the endpoint of the China (Hangzhou) region is used. 
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "YourAccessKeyId";
String accessKeySecret = "YourAccessKeySecret";
// Specify the name of the bucket where the source image is stored. 
String bucketName = "SourceBucketName";
// Specify the name of the source image. The source image name must be the full path of the source image that excludes the bucket name. Example: example/image.png. 
String sourceImage = "SourceObjectName";
// Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
try {
    // Resize the image to 100 × 100 pixels and save the processed image to the specified bucket. 
    StringBuilder sbStyle = new StringBuilder();
    Formatter styleFormatter = new Formatter(sbStyle);
    String styleType = "image/resize,m_fixed,w_100,h_100";
    String targetImage = "TargetBucketName";
    styleFormatter.format("%s|sys/saveas,o_%s,b_%s", styleType,
            BinaryUtil.toBase64String(targetImage.getBytes()),
            BinaryUtil.toBase64String(bucketName.getBytes()));
    System.out.println(sbStyle.toString());
    ProcessObjectRequest request = new ProcessObjectRequest(bucketName, sourceImage, sbStyle.toString());
    GenericResult processResult = ossClient.processObject(request);
    String json = IOUtils.readStreamAsString(processResult.getResponse().getContent(), "UTF-8");
    processResult.getResponse().getContent().close();
    System.out.println(json);
} catch (Exception e) {
    e.printStackTrace();
}
// Shut down the OSSClient instance. 
ossClient.shutdown();
```

For more information about the naming conventions for buckets and objects, see [Terms](/intl.en-US/Developer Guide/Terms.md).

For more information about SDK demos for other programming languages, see the following topics:

-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/IMG.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/IMG.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/IMG.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/IMG.md)
-   [OSS SDK for Android](/intl.en-US/SDK Reference/Android/IMG.md)
-   [OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/IMG.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/IMG.md)
-   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/IMG.md)

## API

When you use the PostObject operation to call IMG, x-oss-process is passed by the request body. You can add saveas to the IMG request to save the processed image to a specified bucket.

You must specify the parameters described in the following table when you add saveas to the request.

|Parameter|Description|
|---------|-----------|
|o|The name of the object as which the processed image is stored. This parameter must be URL-safe Base64-encoded. For more information, see [Encode watermarks](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Add watermarks.md).|
|b|The name of the bucket in which the processed image is stored. This parameter must be URL-safe Base64-encoded. By default, the processed image is saved to the current bucket if this parameter is not specified.|

You can use the following two methods to process an image and save the processed image to the specified bucket:

-   The following code provides an example on how to configure IMG parameters to process an image and save the processed image to a specified bucket:

    ```
    POST /ObjectName?x-oss-process HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 247
    Date: Fri, 04 May 2012 03:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT****=
    
    // Proportionally scale up the source image named test.jpg to a width of 100 pixels and save the processed image to a bucket named test. 
    x-oss-process=image/resize,w_100|sys/saveas,o_dGVzdC5qcGc,b_dGVzdA
    ```

-   The following code provides an example on how to use an image style to process an image and save the processed image to a specified bucket:

    ```
    POST /ObjectName?x-oss-process HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 247
    Date: Fri, 04 May 2012 03:22:13 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:KU5h8YMUC78M30dXqf3JxrT****=
    
    // Use an image style named examplestyle to process the source image named test.jpg and save the processed image to a bucket named test. 
    x-oss-process=style/examplestyle|sys/saveas,o_dGVzdC5qcGc,b_dGVzdA
    ```


