# Image style

You can include multiple IMG parameters in an image style to perform complex operations on image objects. This topic describes how to create and use image styles.

## Create a style

You can create up to 50 styles for a bucket. Styles that are created for a bucket can be used only to process image objects in the bucket. To create more styles for a bucket,contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Data Processing** \> **Image Processing \(IMG\)**. Click **Create Rule**.

4.  In the Create Rule panel, configure the style.

    You can use **Basic Edit** or **Advanced Edit** to create a style:

    -   **Basic Edit**: You can specify the provided IMG parameters to process images. For example, resize an image, add a watermark, and modify the image format.
    -   **Advanced Edit**: You can enter the API code to process images. The code is in the following format: `image/action1,parame_value1/action2,parame_value2/...`.

        Example: `image/resize,p_63/quality,q_90` indicates that the image is scaled down to 63% of the source image, and then the relative quality of the image is set to 90%.

        **Note:** If you want to add image and text watermarks to images at the same time by using a style, use **Advanced Edit** to create the style.

    For more information about IMG parameters, see the corresponding topics for IMG parameters. For example, to know more information about the IMG parameters used to resize an image, see [Resize images](/intl.en-US/Developer Guide/Data Processing/Image Processing/Parameters/Resize images.md)

5.  Click **OK**.


## Usage notes

After you configure an image style, you can use the style instead of IMG parameters in the following methods to process images.

**Note:** If you use a style to process dynamic images such as GIF images, you must add the /format,gif parameter to convert the image format. Otherwise, the GIF image may become a static image after being processed.

-   Use a style in the image URL

    You can add a style to the URL of an image in the following format:`http(s)//:BucketName.Endpoint/ObjcetName?x-oss-process=style/<StyleName>`.

    Example: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=style/small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=style/small)

    If you specify a custom delimiter, you can use the delimiter to replace`?x-oss-process=style/` to simplify the IMG URL.

    For example, if you set the delimiter to an exclamation point \(!\), the IMG URL can be simplified in the following format: `http(s)//:BucketName.Endpoint/ObjcetName! StyleName`

    Example: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg! small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg!small)

    For more information about how to configure a custom delimiter, see [Source image protection]().

-   SDK

    The following code provides an example on how to use styles to process images in OSS SDK for Java:

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
    
    // Use the custom style to process an image.
    String style = "style/<yourCustomStyleName>";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Save the image object named example-new.jpg to your local computer.
    ossClient.getObject(request, new File("example-new.jpg"));
    
    // Shut down the OSSClient instance.
    ossClient.shutdown();
    ```

    For more information about SDK demos for other programming languages, see the following topics:

    -   [Java](/intl.en-US/SDK Reference/Java/IMG.md)
    -   [Python](/intl.en-US/SDK Reference/Python/IMG.md)
    -   [PHP](/intl.en-US/SDK Reference/PHP/IMG.md)
    -   [Go](/intl.en-US/SDK Reference/Go/IMG.md)
    -   [C](/intl.en-US/SDK Reference/C/IMG.md)
    -   [.NET](/intl.en-US/SDK Reference/. NET/IMG.md)
    -   [Node.js](/intl.en-US/SDK Reference/Node. js/IMG.md)
    -   [Browser.js](/intl.en-US/SDK Reference/Browser.js/Image processing.md)

## Apply styles to other buckets

You can apply a style that is configured for a bucket to another bucket by using the style export and import features.

1.  On the Overview page of the bucket, choose **Data Processing** \> **Image Processing \(IMG\)** in the left-side navigation pane.

2.  Click **Export**. In the Save As dialog box, select the path to which you want to save the style, and then click **Save**.

3.  On the Image Processing \(IMG\) tab of the bucket to which you want to apply the style, click **Import**.

4.  In the Open dialog box, select the exported style file, and then click **Open**.

    After the style is imported to the new bucket, you can use the style to process images stored in the bucket.


