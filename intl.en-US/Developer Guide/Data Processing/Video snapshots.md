# Video snapshots

This topic describes the parameters that you can configure to capture video snapshots and provides examples.

## Usage notes

-   When you capture video snapshots, you are charged for the number of captured images. For more information about billing, see [Data processing fees](/intl.en-US/Pricing/Billing items and methods/Data processing fees.md).
-   OSS can capture images from video objects only in the H.264 and H.265 formats.
-   By default, OSS does not automatically store captured images. You must manually download the captured images to your local storage devices.

## Parameters

Operation type: `video`

Operation name: snapshot

|Parameter|Description|Valid value|
|---------|-----------|-----------|
|t|The time when the image is to be captured.|\[0, video duration\] Unit: ms |
|w|The width based on which to capture the image. If this parameter is set to 0, the width based on which to capture the image is automatically calculated.|\[0, video width\] Unit: px |
|h|The height based on which to capture the image. If this parameter is set to 0, the height based on which to capture the image is automatically calculated. If w and h are set to 0, the width and height of the source image are used.|\[0, video height\] Unit: px |
|m|The mode used to capture the image. If this parameter is not specified, the image is captured in the default mode. In other words, the image at the specified point in time in the video is captured. If this parameter is set to fast, the most recent key image before the specified time is captured.|fast|
|f|The format of the captured image.|jpg and png|
|ar|Specifies whether to automatically rotate the image based on the video information. If this parameter is set to auto, the system automatically rotates the image based on the video information.|auto|

## Examples

-   Use the fast mode to capture the image at the seventh second of the video. Export the captured image as a JPG image whose width is 800 pixels and height is 600 pixels.

    The URL of the processed image is in the following format: `<Video URL>?x-oss-process=video/snapshot,t_7000,f_jpg,w_800,h_600,m_fast`

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8956348951/p2927.jpg)

-   Capture the image at the fiftieth second of the video accurately. Export the captured image as a JPG image whose width is 800 pixels and height is 600 pixels.

    The URL of the processed the image is in the following format: `<Video URL>? x-oss-process=video/snapshot,t_50000,f_jpg,w_800,h_600`

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8956348951/p2928.jpg)


## Generate a signed URL for a captured snapshot

You can use OSS SDKs to generate a signed URL to capture a snapshot. The following code provides an example on how to use OSS SDK for Java to generate a signed URL to capture a snapshot:

```
// The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS, because the account has permissions on all API operations.  We recommend that you use your RAM user's credential to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
// Specify the bucket in which the video object is stored.
String bucketName = "<yourBucketName>";
// Specify the full path of the video object. If the video object is not stored in the root directory of the bucket, the path must include the directory in which the object is stored. Example: examplefolder/videotest.mp4.
String objectName = "<yourObjectName>";
// Create an OSSClient
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
// Capture the image at the fiftieth second of the video accurately. Export the captured image as a JPG image whose width is 800 pixels and height is 600 pixels.
String style = "video/snapshot,t_50000,f_jpg,w_800,h_600";
// Set the validity period of the URL to 10 minutes.
Date expiration = new Date(new Date().getTime() + 1000 * 60 * 10 );
GeneratePresignedUrlRequest req = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.GET);
req.setExpiration(expiration);
req.setProcess(style);
URL signedUrl = ossClient.generatePresignedUrl(req);
System.out.println(signedUrl);
// Shut down the OSSClient.
ossClient.shutdown();
```

The method used to generate signed URLs to capture snapshots is similar to that used to generated signed URLs to process images by using Image Processing \(IMG\). To use the following OSS SDKs to generate a signed URL to capture a snapshot, replace the IMG operations in the code with the snapshot operation.

-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/IMG.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/IMG.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/IMG.md)
-   [OSS SDK for C](/intl.en-US/SDK Reference/C/IMG.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/IMG.md)
-   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/IMG.md)
-   [OSS SDK for Android](/intl.en-US/SDK Reference/Android/IMG.md)
-   [OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/IMG.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/IMG.md)
-   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Image processing.md)

