# Image style

Image Processing \(IMG\) allows you to save IMG operations as a style. The style feature allows you to use a short IMG parameter to perform complex image processing operations.

## Limits

Up to 50 styles can be created for a bucket. These styles can be used only for image objects in the bucket. If you require more than 50 styles, contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

## Create a style

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Data Processing** \> **Image Processing \(IMG\)**. Click **Create Rule**.

4.  In the Create Rule pane, configure the style.

    You can use **Basic Edit** or **Advanced Edit** to create a style:

    -   **Basic Edit**: You can use the IMG parameters listed by the system to choose the IMG methods. For example, resize an image, add a watermark, and modify the image format.
    -   **Advanced Edit**: You can use the API code to edit the IMG methods. The format is: `image/action1,parame_value1/action2,parame_value2 /...`.

        For example: `image/resize,p_63/quality,q_90` indicates that the image is scaled down to 63% of the source image, and set the relative quality of the image to 90%.

    For more information about IMG parameters, see the corresponding topics for IMG parameters.

    **Note:** The name of the image style must be 1 to 64 characters in length, and can contain only digits, letters, underscores \(\_\), hyphens \(-\), and periods \(.\).

5.  Click **OK**.


## Export a style

You can export a style from a bucket, and import the exported style to other buckets.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Data Processing** \> **Image Processing \(IMG\)**.

4.  Click **Export**. In the Save As dialog box, select the address to save the style, and click **Save**.


## Import a style

You can import the style object to a bucket to quickly generate multiple styles at a time.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Data Processing** \> **Image Processing \(IMG\)**.

4.  Click **Import**. In the Open dialog box, select the style object, and click **Open**.


## Usage notes

After you configure an image style, you can use the style to replace the IMG parameters:

-   IMG URL

    You can add an image style to the image URL in the following format: `http(s)//:BucketName.Endpoint/ObjectName? x-oss-process=style/<StyleName>`

    Example: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=style/small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=style/small)

    If you set the custom delimiter, use the delimiter to replace `? x-oss-process=style/` to further simplify the IMG URL.

    For example, if you set the delimiter to an exclamation point \(!\), the IMG URL is: `http(s)//:BucketName.Endpoint/ObjectName! StyleName`

    Example: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg! small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg!small)

    For more information about how to configure a custom delimiter, see [Source image protection]().

-   SDK

    You can replace the IMG parameter code with the IMG style code. The following code provides an example on how to replace the IMG parameter code with the IMG style code by using OSS SDK for Java:

    ```
    // The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    String objectName = "<yourObjectName>";
    
    // Create an OSSClient instance.
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Use a custom style to process an image.
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

**Note:** If you use a custom style to process dynamic images such as GIF images, you must add the /format,gif parameter to convert the format. Otherwise, the GIF image may become a static image after the GIF image is processed.

