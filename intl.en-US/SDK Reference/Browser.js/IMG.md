# IMG

OSS Image Processing \(IMG\) is a secure, cost-effective, and highly reliable image processing service that can process large amounts of data. After you upload source images to OSS, you can call RESTful APIs to process the images anytime, anywhere, and on any Internet device.

For more information about the IMG parameters, see [Parameters](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md).

## Use the IMG parameters to process images

-   Use a single IMG parameter to process images

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
    // Specify the AccessKey pair that has write permissions on the bucket. High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions on all resources. We recommend that you log on as a RAM user to call API operations or perform routine O&M.
      accessKeyId: 'YourAccessKeyId',
      accessKeySecret: 'YourAccessKeySecret',
    // Specify the bucket name.
      bucket: 'YourBucketName',
    // Specify the endpoint of the region in which the bucket is located. In this example, the endpoint of the China (Hangzhou) region is used.
      endpoint: 'http://oss-cn-hangzhou.aliyuncs.com'
    });
    
    // Resize the image to a height and width of 100 pixels.
    async function scale () {
      try {
        let result = await client.signatureUrl('YourObjectName', {expires: 3600, process: 'image/resize,m_fixed,w_100,h_100'})
      } catch (e) {
        console.log(e);
      }
    }
    
    scale();
                     
    ```

-   Use multiple IMG parameters to process images

    When you use multiple IMG parameters to process the image, separate these parameters with forward slashes \(/\).

    ```
    let OSS = require('ali-oss');
    
    let client = new OSS({
    // Specify the AccessKey pair that has write permissions on the bucket. High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions on all resources. We recommend that you log on as a RAM user to call API operations or perform routine O&M.
      accessKeyId: 'YourAccessKeyId',
      accessKeySecret: 'YourAccessKeySecret',
    // Specify the bucket name.
      bucket: 'YourBucketName',
    // Specify the endpoint of the region in which the bucket is located. In this example, the endpoint of the China (Hangzhou) region is used.
      endpoint: 'http://oss-cn-hangzhou.aliyuncs.com'
    });
    
    // After you resize the image to a height and width of 100 pixels, rotate the image 90°.
    async function resize () {
      try {
        let result = await client.signatureUrl('YourObjectName', {expires: 3600, process: 'image/resize,m_fixed,w_100,h_100/rotate,90'})
      } catch (e) {
        console.log(e);
      }
    }
    
    resize();
                                
    ```


## Use image styles to process images

You can encapsulate a variety of IMG parameters within a style, and then use the style to concurrently process multiple images. For more information about how to create an image style, see [Image style](/intl.en-US/Developer Guide/Data Processing/Image Processing/Image style.md).

```
let OSS = require('ali-oss');

let client = new OSS({
// Specify the AccessKey pair that has write permissions on the bucket. High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions on all resources. We recommend that you log on as a RAM user to call API operations or perform routine O&M.
  accessKeyId: 'YourAccessKeyId',
  accessKeySecret: 'YourAccessKeySecret',
// Specify the bucket name.
  bucket: 'YourBucketName',
// Specify the endpoint of the region in which the bucket is located. In this example, the endpoint of the China (Hangzhou) region is used.
  endpoint: 'http://oss-cn-hangzhou.aliyuncs.com'
});
// Use the specified image style to process the image.
async function style () {
  try {
    let result = await client.signatureUrl('YourObjectName', {expires: 3600, process: 'style/YourStyleName'});
  } catch (e) {
    console.log(e);
  }
}

style();
                            
```

## Permanent store a processed image

The following code provides an example on how to save processed images:

```
let OSS = require('ali-oss');

let client = new OSS({
  // Specify the name of the bucket where the source image is stored.
  bucket: 'SourceBucketName',
  // Specify the endpoint of the region in which the bucket is located. In this example, the endpoint of the China (Hangzhou) region is used.
  endpoint: 'http://oss-cn-hangzhou.aliyuncs.com',
  // Security risks may arise if you log on with the AccessKey pair of an Alibaba Cloud account because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
  accessKeyId: 'YourAccessKeyId',
  accessKeySecret: 'YourAccessKeySecret',
});
// Specify the name of the source image.
let sourceImage = 'SourceObjectName';
// Specify the name of the processed image.
let targetImage = 'TargetObjectName';
async function processImage(processStr, targetBucket) {
  let result = await client.processObjectSave(
    sourceImage,
    targetImage,
    processStr,
    targetBucket
  );
  console.log(result.res.status);
}

// Resize the source image to a height and width of 100 pixels and save the image to the specified bucket.
processImage("image/resize,m_fixed,w_100,h_100", "TargetBucketName")
            
```

## Generate a signed object URL that includes IMG parameters

URLs of private objects must be signed. OSS does not allow you to add IMG parameters to a signed URL. If you want to process private objects, add IMG parameters to the signature. The following code provides an example on how to add IMG parameters to the signature:

```
let OSS = require('ali-oss');

let client = new OSS({
// Specify the AccessKey pair that has write permissions on the bucket. High security risks may arise if you use the AccessKey pair of your Alibaba Cloud account because the account has permissions on all resources. We recommend that you log on as a RAM user to call API operations or perform routine O&M.
  accessKeyId: 'YourAccessKeyId',
  accessKeySecret: 'YourAccessKeySecret',
// Specify the bucket name.
  bucket: 'YourBucketName',
// Specify the endpoint of the region in which the bucket is located. In this example, the endpoint of the China (Hangzhou) region is used.
  endpoint: 'http://oss-cn-hangzhou.aliyuncs.com'
});
// Generate a signed object URL that includes IMG parameters. Set the validity period to 600 seconds.
let signUrl = client.signatureUrl('example.jpg', {expires: 600, 'process' : 'image/resize,w_300'});
console.log("signUrl="+signUrl);
```

