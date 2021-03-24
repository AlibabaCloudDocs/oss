# Configure source image protection

OSS provides the source image protection feature to protect your images from being used by unauthorized anonymous requesters. After you enable source image protection for your bucket, anonymous requesters can access images in the bucket only by adding style parameters in the requests or by using signed URLs.

You can use the following methods to access the images:

-   Use the file URL that has the style parameters in the format of `https:// BucketName.Endpoint/ObjectName? x-oss-process=style/StyleName`.
-   Use the file URL that has a signed URL in the format of `https://BucketName.Endpoint/ObjectName?Signature`.

## Procedures

1.  Log on to the [OSS console](https://oss.console.aliyun.com/overview).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the bucket for which you want to configure scheduled backup.

3.  In the left-side navigation pane, choose **Data Processing** \> **Image Processing \(IMG\)**. Then, click **Access Settings**.

4.  In the Access Settings panel, turn on **Protect Source Image File** and configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Source Image Protection Rule**|Configure the prefix and suffix of the image objects that you want to protect. When you configure the prefix and suffix, take note of the following items:    -   You can configure the prefix and suffix separately or at the same time. If you configure both the prefix and suffix, only images whose names contain both the specified prefix and suffix are protected by the rule.
    -   If multiple rules are configured, images whose names match one of the rules are protected. You can configure up to 10 rules.
    -   If you specify both **Source Image Protection Rule** and **Protected File Extensions**, images whose names match one of the specified values are protected.
    -   If you select **Case Insensitive**, the prefix and suffix specified in the rule are case-insensitive.
**Note:** Source image protection is in public preview in the China \(Shanghai\) region.Contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial. |
    |**Protected File Extensions**|Select a file suffix from the **Protected File Extensions** drop-down list. All objects in the bucket that match the specified suffix are protected.|

5.  Click **OK**.


## FAQ

-   Why is HTTP status code 403 returned when I directly access a protected image, whereas HTTP status code 200 is returned when I access the image over CDN?

    One possible cause is that the request is redirected to access a private bucket through CDN. Source image protection applies only to objects that can be accessed by anonymous users.

-   How can my source image be accessed through a signed URL when source image protection is enabled for the image?

    Source image protection applies only to objects that can be accessed by anonymous users. Access by signed URLs is not anonymous. Therefore, the source image can be accessed by using a signed URL even if you enable source image protection.


