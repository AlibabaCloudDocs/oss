# Configure image styles

You can encapsulate multiple image processing \(IMG\) parameters in a style and perform complex IMG operations by using the style.

Up to 50 styles can be created for a bucket. These styles can be used only for image objects in the bucket. If you want to create more than 50 styles, contact[technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).

## Create a style

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the bucket that you want to configure.

3.  In the left-side navigation pane, choose **Data Processing** \> **Image Processing \(IMG\)**. Then, click **Create Rule**.

4.  In the Create Rule pane, configure the style.

    You can use **Basic Edit** or **Advanced Edit** to create a style:

    -   **Basic Edit**: You can use the IMG parameters listed by using the graphical user interface \(GUI\) to choose the IMG methods. For example, resize an image, add a watermark, and modify the image format.
    -   **Advanced Edit**: You can use the API code to edit the IMG methods. The format is: `image/action1,parame_value1/action2,parame_value2/...`. For more information about supported IMG parameters, see the "[Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md)" section of the Overview topic.

        Example: `image/resize,p_63/quality,q_90` indicates that the image is scaled down to 63% of the source image, and then the relative quality of the image is set to 90%.

        **Note:** If you want to add image and text watermarks to images at the same time by using a style, use **Advanced Edit** to create the style.

5.  Click **OK**.


## Apply styles

After the style is created, you can use the style in the bucket to process your image objects.

1.  On the buckets page, click **Files**.

2.  Click the name of the image.

3.  In the **View Details** panel, select an image style from the **Image Style** drop-down list.

    You can view the processed image in the **View Details** panel. Right-click the image and click **Save As** to save the image to your local computer.

    You can also add styles to the IMG URLs and SDKs. For more information, see [Usage notes](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).


## Import the styles of the source bucket to the destination bucket

You can export styles that are created in the source bucket and import the styles to the destination bucket. This way, you can apply styles in the destination bucket to process image objects.

1.  Export styles in the source bucket.

    1.  On the Overview page of the source bucket, choose **Data Processing** \> **Image Processing \(IMG\)** in the left-side navigation pane.

    2.  Click **Export**.

    3.  In the Save As dialog box, select the path to which you want to save the style, and click **Save**.

2.  Import styles to the destination bucket.

    1.  On the Overview page of the destination bucket, choose **Data Processing** \> **Image Processing \(IMG\)** in the left-side navigation pane.

    2.  Click **Import**.

    3.  In the Open dialog box, select the exported style file and click **Open**.

        After the style is imported to the destination bucket, you can use the style to process images stored in the bucket.


## Simplify IMG URLs that have style parameters

IMG URLs that have styles contain file access URLs, style parameters, and style names. Example: [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg? x-oss-process=style/small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg?x-oss-process=style/small). You can replace the `?x-oss-process=style/` field by using custom delimiters to simplify the IMG URL. For example, set the custom delimiter to an exclamation point \(!\). IMG URLs can be replaced by [https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg! small](https://image-demo-oss-zhangjiakou.oss-cn-zhangjiakou.aliyuncs.com/example.jpg!small).

1.  On the Overview page of the bucket, choose **Data Processing** \> **Image Processing \(IMG\)** in the left-side navigation pane.

2.  Click **Access Settings**.

3.  On the Access Settings pane, select **Delimiters**.

    Only hyphens \(-\), underscores \(\_\), forward slashes \(/\), and exclamation points \(?\) can be used as delimiters.

4.  Click **OK**.

    You can also bind a custom domain name to the bucket to further simplify the IMG URL. For example, if the sample bucket is bound to the `example.com` custom domain name , the sample URL can be replaced with `https://example.com/example.jpg! small`. After you bind a custom domain name, you can also preview the effect of IMG online. For more information, see [Bind custom domain names](/intl.en-US/Console User Guide/Manage buckets/Manage a domain/Bind custom domain names.md).


## References

-   For more information about how to use IMG parameters to process images, see [IMG implementation modes](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).
-   For more information about how to save processed images in OSS, see [IMG persistence](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG persistence.md).

