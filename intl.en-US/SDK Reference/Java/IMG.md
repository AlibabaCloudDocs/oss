# IMG

Image Processing \(IMG\) provided by Object Storage Service \(OSS\) is a secure, cost-effective, and highly reliable image processing service that can process large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on all Internet devices.

**Note:** For more information about IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md). For the complete IMG code, visit [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/ImageSample.java).

## Use IMG parameters to process images

-   Use a single IMG parameter to process an image and save the image as the local image

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name. 
    String bucketName = "examplebucket";
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    String objectName = "exampleobject.jpg";
    
    // Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Resize the image to the height and width of 100 pixels. 
    String style = "image/resize,m_fixed,w_100,h_100";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-resize.jpg and save the image to your local computer. 
    // Specify the full path of the local file. If the specified local file exists, the processed object replaces the local file. Otherwise, the local file is created. 
    // By default, if the local path is not specified for the processed object, the processed object is saved to the path of the project to which the sample program belongs. 
    ossClient.getObject(request, new File("D:\\localpath\\example-resize.jpg"));
    
    // Shut down the OSSClient instance. 
    ossClient.shutdown();
    ```

-   Use multiple IMG parameters to process an image and save each image as a local image

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name. 
    String bucketName = "examplebucket";
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    String objectName = "exampleobject.jpg";
    
    // Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // Resize the image to the height and width of 100 pixels. 
    String style = "image/resize,m_fixed,w_100,h_100";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-resize.jpg and save the image to your local computer. 
    // Specify the full path of the local file. If the specified local file exists, the processed object replaces the local file. Otherwise, the local file is created. 
    // By default, if the local path is not specified for the processed object, the processed object is saved to the path of the project to which the sample program belongs. 
    ossClient.getObject(request, new File("D:\\localpath\\example-resize.jpg"));
    
    // Crop the image to the height and width of 100 pixels by setting the coordinate pair to (100, 100). 
    style = "image/crop,w_100,h_100,x_100,y_100";
    request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-crop.jpg and save the image to your local computer. 
    ossClient.getObject(request, new File("D:\\localpath\\example-crop.jpg"));
    
    // Rotate the image 90 degrees. 
    style = "image/rotate,90";
    request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-rotate.jpg and save the image to your local computer. 
    ossClient.getObject(request, new File("D:\\localpath\\example-rotate.jpg"));
    
    // Add a text watermark to the image. 
    // After the text watermark content is encoded in Base64, replace plus signs (+) in the encoded result with hyphens (-) and forward slashes (/) with underscores (_). Remove equal signs (=) at the end. Then, you can obtain the watermark string. 
    // Specify that the text watermark content is Hello World. Encode the text content. Then, you can obtain the SGVsbG8gV29ybGQ watermark string. 
    String style = "image/watermark,text_SGVsbG8gV29ybGQ";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-watermarktext.jpg and save the image to your local computer. 
    ossClient.getObject(request, new File("D:\\localpath\\example-watermarktext.jpg"));
    
    // Add an image watermark to the image. Make sure that the watermark image is stored in the bucket that contains the source image. 
    // After the full path of the image watermark is encoded in Base64, replace plus signs (+) in the encoded result with hyphens (-) and forward slashes (/) with underscores (_). Remove equal signs (=) at the end. Then, you can obtain the watermark string. 
    // Specify that the full path of the watermark image is panda.jpg. Encode the full path. Then, you can obtain the cGFuZGEuanBn watermark string. 
    String style = "image/watermark,image_cGFuZGEuanBn";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-watermarkimage.jpg and save the image to your local computer. 
    ossClient.getObject(request, new File("D:\\localpath\\example-watermarkimage.jpg"));
    
    // Shut down the OSSClient instance. 
    ossClient.shutdown();
    ```

-   Use multiple IMG parameters to process an image and save the image as the local image

    When you use multiple IMG parameters to process an image, separate these parameters with forward slashes \(/\).

    ```
    // Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    String endpoint = "yourEndpoint";
    // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    String accessKeyId = "yourAccessKeyId";
    String accessKeySecret = "yourAccessKeySecret";
    // Specify the bucket name. 
    String bucketName = "examplebucket";
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    String objectName = "exampleobject.jpg";
    
    // Create an OSSClient instance. 
    OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
    
    // After you resize the image to the height and width of 100 pixels, rotate the image 90 degrees. 
    String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
    GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
    request.setProcess(style);
    // Name the processed image example-new.jpg and save the image to your local computer. 
    // Specify the full path of the local file. If the specified local file exists, the processed object replaces the local file. Otherwise, the local file is created. 
    // By default, if the local path is not specified for the processed object, the processed object is saved to the path of the project to which the sample program belongs. 
    ossClient.getObject(request, new File("D:\\localpath\\example-new.jpg"));
    
    // Shut down the OSSClient instance. 
    ossClient.shutdown();
    ```


## Use image styles to process images

You can create an image style in the OSS console and encapsulate multiple IMG parameters in the style. Then, you can use the style to process an image. For more information, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

The following code provides an example on how to use an image style to process an image:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name. 
String bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
String objectName = "exampleobject.jpg";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);

// Use the custom style to process an image. 
// Set yourCustomStyleName to the name of the image style you create in the OSS console. 
String style = "style/yourCustomStyleName";
GetObjectRequest request = new GetObjectRequest(bucketName, objectName);
request.setProcess(style);
// Name the processed image example-new.jpg and save the image to your local computer. 
// Specify the full path of the local file. If the specified local file exists, the processed object replaces the local file. Otherwise, the local file is created. 
// By default, if the local path is not specified for the processed object, the processed object is saved to the path of the project to which the sample program belongs. 
ossClient.getObject(request, new File("D:\\localpath\\example-new.jpg"));

// Shut down the OSSClient instance. 
ossClient.shutdown();
```

## Save processed images

By default, IMG does not save processed images. You can call the ImgSaveAs operation to save the images to the bucket where the source images are saved.

The following code provides an example on how to save a processed image:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name. 
String bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
String sourceImage = "exampleimage.png";
// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
try {
    // Resize the image to the height and width of 100 pixels. 
    StringBuilder sbStyle = new StringBuilder();
    Formatter styleFormatter = new Formatter(sbStyle);
    String styleType = "image/resize,m_fixed,w_100,h_100";
    // Name the processed image example-resize.png and save the image to the current bucket. 
    // Specify the full path of the object. The full path of the object cannot contain bucket names. 
    String targetImage = "example-resize.png";
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

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. IMG parameters cannot be added to a signed URL. If you want to process a private object, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
// Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
String endpoint = "yourEndpoint";
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
String accessKeyId = "yourAccessKeyId";
String accessKeySecret = "yourAccessKeySecret";
// Specify the bucket name. 
String bucketName = "examplebucket";
// Specify the full path of the object. The full path of the object cannot contain bucket names. 
String objectName = "exampleobject.jpg";

// Create an OSSClient instance. 
OSS ossClient = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
// After you resize the image to the height and width of 100 pixels, rotate the image 90 degrees. 
String style = "image/resize,m_fixed,w_100,h_100/rotate,90";
// Set the validity period to 10 minutes. 
Date expiration = new Date(new Date().getTime() + 1000 * 60 * 10 );
GeneratePresignedUrlRequest req = new GeneratePresignedUrlRequest(bucketName, objectName, HttpMethod.GET);
req.setExpiration(expiration);
req.setProcess(style);
URL signedUrl = ossClient.generatePresignedUrl(req);
System.out.println(signedUrl);

// Shut down the OSSClient instance. 
ossClient.shutdown();
```

