# IMG implementation modes

You can use object URLs, API operations, or SDKs to process images in OSS. This topic describes how to use these three methods to process images.

## Add parameters to the URL of an image object

You can add Image Processing \(IMG\) parameters or image styles to the end of the URL of an image object.

**Note:** By default, when you use a URL to access an image object in a bucket, the image is downloaded. To ensure that an image object is previewed when you access the image object, you must map a custom domain name to your bucket and add a CNAME record. For more information, see [Map custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Map custom domain names.md).

-   Add IMG parameters

    Format: `https://bucketname.endpoint/objectname?x-oss-process=image/action,parame_value`

    -   `https://bucketname.endpoint/objectname`: the URL of the image object. For more information about how to obtain the URL of an object, see [How do I obtain the URL of an uploaded object?](/intl.en-US/Developer Guide/Objects/FAQ/How do I obtain the URL of an uploaded object?.md)
    -   `x-oss-process=image/`: the fixed parameter, which indicates that the image object is processed by using IMG parameters.
    -   `action, param_value`: the action, parameter, and value of an IMG operation. These parameters determine the IMG operation that is used to process the image object. Separate multiple operations with forward slashes \(/\). OSS processes images in the order of IMG parameters. For example, `image/resize,w_200/rotate,90` indicates that OSS resizes the image to a width of 200 pixels, and rotates the image 90°. For more information about the supported IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).
    Example:[https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w\_300/quality,q\_90](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_300,h_300/quality,q_90/watermark,image_cGFuZGEucG5n,t_90,g_se,x_10,y_10)

-   Add style parameters

    Format: `https://bucketname.endpoint/objectname?x-oss-process=style/stylename`

    -   https://bucketname.endpoint/objectname: the URL of the image object.
    -   x-oss-process=style/: the fixed parameter, which indicates that the image object is processed by using style parameters.
    -   stylename: the name of the style that you set in the OSS console. For more information, see [Create a style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).
    Example: [https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda\_style](https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda_style)

    If you specify a custom delimiter, you can use the delimiter to replace`?x-oss-process=style/` to simplify the IMG URL. For example, if you set the delimiter to an exclamation point \(!\), the URL of the processed image object is`<https://bucketname.endpoint/objectname!stylename`. For more information about how to configure custom delimiters, see the "Configure source image protection" section in [Source image protection](/intl.en-US/Console User Guide/Manage buckets/Data Processing/Image processing/Configure source image protection.md).


The preceding description applies to only objects that can be anonymously accessed. If your object does not allow anonymous access, you must add the IMG operation to the signature. For more information, see the following topics:

-   OSS SDK for Java: [IMG](/intl.en-US/SDK Reference/Java/IMG.md)
-   OSS SDK for Python: [IMG](/intl.en-US/SDK Reference/Python/IMG.md)
-   OSS SDK for PHP: [IMG](/intl.en-US/SDK Reference/PHP/IMG.md)
-   OSS SDK for Go: [IMG](/intl.en-US/SDK Reference/Go/IMG.md)
-   OSS SDK for C: [IMG](/intl.en-US/SDK Reference/C/IMG.md)
-   OSS SDK for C++: [IMG](/intl.en-US/SDK Reference/C++/IMG.md)
-   OSS SDK for .NET: [IMG](/intl.en-US/SDK Reference/. NET/IMG.md)
-   OSS SDK for Android: [IMG](/intl.en-US/SDK Reference/Android/IMG.md)
-   OSS SDK for iOS: [IMG](/intl.en-US/SDK Reference/iOS/IMG.md)
-   OSS SDK for Node.js: [IMG](/intl.en-US/SDK Reference/Node. js/IMG.md)
-   OSS SDK for Browser.js: [IMG](/intl.en-US/SDK Reference/Browser.js/IMG.md)

## Use SDKs to process images

You can configure IMG or style parameters in SDKs to process image objects. The following code provides an example on how to use OSS SDK for Java to process images:

-   Configure IMG parameters

    When you use IMG parameters to process images, separate multiple IMG operations with forward slashes \(/\).

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint. 
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    // Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    // Resize the image to a height and width of 100 pixels, and then add text watermarks. The content of the watermark is: Hello image service. 
    String style = "image/resize,m_fixed,w_100,h_100/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    ossClient.getObject(request, new File("<example.jpg>"));
    // Shut down the OSSClient instance. 
    ossClient.shutdown();
                            
    ```

-   Configure style parameters

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint. 
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Customize the image style. 
    String style = "style/<yourCustomStyleName>";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    
    ossClient.getObject(request, new File("<example.jpg>"));
    
    // Shut down the OSSClient instance. 
    ossClient.shutdown();
    ```


For more information about SDK demos for other programming languages, see the following topics:

-   [Java](/intl.en-US/SDK Reference/Java/IMG.md)
-   [Python](/intl.en-US/SDK Reference/Python/IMG.md)
-   [PHP](/intl.en-US/SDK Reference/PHP/IMG.md)
-   [Go](/intl.en-US/SDK Reference/Go/IMG.md)
-   [C](/intl.en-US/SDK Reference/C/IMG.md)
-   [C++](/intl.en-US/SDK Reference/C++/IMG.md)
-   [.NET](/intl.en-US/SDK Reference/. NET/IMG.md)
-   [Android](/intl.en-US/SDK Reference/Android/IMG.md)
-   [iOS](/intl.en-US/SDK Reference/iOS/IMG.md)
-   [Node.js](/intl.en-US/SDK Reference/Node. js/IMG.md)
-   [Browser.js](/intl.en-US/SDK Reference/Browser.js/IMG.md)

## Call API operations to process images

You can add IMG or style parameters to GetObject requests to process image objects.

-   Add IMG parameters

    Sample requests

    ```
    GET /oss.jpg?x-oss-process=image/resize,w_100 HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 06:38:30 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:UNQDb7GapEgJkcde6OhZ9J*****
    ```

-   Add style parameters

    Sample requests

    ```
    GET /oss.jpg?x-oss-process=style/styleexample HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 06:40:10 GMT
    Authorization: OSS qn6qrrawuk53oqxo2otfjbyc:UNapEgQDb7GJkcde6OhZ9J*****
    ```


## Save processed images

By default, IMG does not save processed images. However, OSS allows you to add the saveas parameter in an IMG request to save the processed image as an object in a specified bucket. For more information, see [Save processed images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Save processed images.md).

