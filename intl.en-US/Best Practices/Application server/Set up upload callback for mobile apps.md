# Set up upload callback for mobile apps

This topic describes how to set up an OSS-based direct data transfer service for mobile apps and configure upload callback.

## Background information

[Set up direct data transfer for mobile apps](/intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md) describes how to set up an OSS-based direct data transfer service for a mobile app. When you set up an OSS-based direct transfer service for an Android or iOS app, an STS token is used to authorize the app to connect to the app server. Users can perform multiple upload operations as long as the token is valid. The app server cannot identify the data uploaded by the users because the same STS token is used. This method complicates the management of data that is uploaded by using apps. To address this need, OSS provides the upload callback function.

## Process

After OSS receives an upload request sent from an Android or iOS app \(Step 5\), OSS triggers an upload callback task \(Step 7\) to send a callback request to the app server. OSS then obtains the response from the app server and sends the response to the Android or iOS app \(Step 6\). For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) in OSS API Reference.

## Purposes

-   Upload callback allows the app server to obtain the basic information about the uploaded object.

    The following table describes the variables returned in the basic information. The format of the returned content is specified when the object is uploaded by using the Android or iOS app.

    |System variable|Description|
    |:--------------|:----------|
    |bucket|The bucket to which the object is uploaded by using the mobile app.|
    |object|The name of the object after the object is uploaded to OSS by using the mobile app.|
    |etag|The ETag of the uploaded object. The ETag is contained in the ETag field returned to the mobile app user.|
    |size|The size of the uploaded object.|
    |mimeType|The type of the resource.|
    |imageInfo.height|The height of an image.|
    |imageInfo.width|The width of an image.|
    |imageInfo.format|The format of the image. Examples: JPG and PNG.|

-   Customize upload callback parameters for the app server to obtain callback information.

    If you are a developer and you want to obtain information about the user such as the app version, operating system version, location, and mobile phone model, you can customize the following parameters when you upload an object by using the Android or iOS app:

    -   x:version: the app version
    -   x:system: the operating system version
    -   x: gps: the GPS information.
    -   x: phone: the mobile phone model
    The preceding parameters are included when you upload the object by using the Android or iOS app. OSS then includes these parameters in callbackBody sent to the app server. This method allows the app server to receive the information.


## Requirements on the app server for upload callback

-   A service that can receive a POST request has been deployed. This service has been configured with a public IP address such as `www.abc.com/callback.php` or `11.22.33.44/callback.php`.
-   This service can respond to OSS in the JSON format. You can customize the content in the response. OSS returns the response from the app server to the Android or iOS app. For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) in OSS API Reference.

## Configure upload callback on the mobile app

To trigger upload callback when OSS receives an upload request, include the following content in the upload request on the mobile app:

-   callbackUrl: specifies the URL of the server to which the callback request is sent such as `http://abc.com/callback.php`. This URL must be accessible over the Internet.
-   callbackBody: specifies the system variables OSS returns to the app server. You can specify one or more variables in the upload request.

Assume that you are a developer and the URL of the app server is `http://abc.com/callback.php`. You want to obtain the object name and size, mobile phone model, and operating system version.

Sample requests for upload callback:

-   Sample request for upload callback on the iOS app:

    ```
    OSSPutObjectRequest * request = [OSSPutObjectRequest new];
    request.bucketName = @"<bucketName>";
    request.objectKey = @"<objectKey>";
    request.uploadingFileURL = [NSURL fileURLWithPath:@<filepath>"];
    // Set callback parameters.
    request.callbackParam = @{
                              @"callbackUrl": @"http://abc.com/callback.php",
                              @"callbackBody": @"filename=${object}&size=${size}&photo=${x:photo}&system=${x:system}"
                              };
    // Customize variables.
    request.callbackVar = @{
                            @"x:phone": @"iphone6s",
                            @"x:system": @"ios9.1"
                            };
    ```

-   Sample request for upload callback on the Android app:

    ```
    PutObjectRequest put = new PutObjectRequest(testBucket, testObject, uploadFilePath);
    ObjectMetadata metadata = new ObjectMetadata();
    metadata.setContentType("application/octet-stream");
    put.setMetadata(metadata);
    put.setCallbackParam(new HashMap<String, String>() {
        {
            put("callbackUrl", "http://abc.com/callback.php");
            put("callbackBody", "filename=${object}&size=${size}&photo=${x:photo}&system=${x:system}");
        }
    });
    put.setCallbackVars(new HashMap<String, String>() {
         {
             put("x:phone", "IPOHE6S");
             put("x:system", "YunOS5.0");
         }
    });
    ```


## Sample callback requests received by the app server

The callback requests received by the app server differ in URLs and callback content. Example:

```
POST /index.html HTTP/1.0
Host: 121.43.113.8
Connection: close
Content-Length: 81
Content-Type: application/x-www-form-urlencoded
User-Agent: ehttp-client/0.0.1
authorization: kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWE****G0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==
x-oss-pub-key-url: aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNv****YWxsYmFja19wdWJfa2V5X3YxLnBlbQ==
filename=test.txt&size=5&photo=iphone6s&system=ios9.1
```

For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) in OSS API Reference.

## The app server determines whether the callback request is sent from OSS

If your app server is attacked and receives an invalid callback request, the app server needs to determine whether the callback request is sent from OSS.

To determine whether the callback request is sent from OSS, use the `x-oss-pub-key-url` and `authorization` parameters in the header returned from OSS for RSA signature verification. Only requests that pass RSA signature verification are sent from OSS. The sample programs described in this topic provide the parameters for RSA signature verification for your reference.

## The app server processes the callback request

After the app server determines that the request is sent from OSS, the format for the response of the app server to OSS is specified. Example:

```
filename=test.txt&size=5&photo=iphone6s&system=ios9.1
```

The app server can parse the returned content of OSS to obtain the desired data. After the data is obtained, the app server can store the data for subsequent management.

## OSS processes the response from the app server

Two scenarios:

-   OSS sends the callback request to the app server. The app server fails to receive the request or is inaccessible. OSS sends status code 203 to the Android or iOS app. In this case, data has been stored in OSS.
-   The app server receives the callback request from OSS and responds to OSS properly. OSS returns status code 200 to the Android or iOS app and returns the response from the app server to the Android or iOS app.

## Download sample programs

The sample programs only check the signature received by the app server. You need to manually add the code to parse the format of the content received by the app server.

-   Java
    -   Download link: [Click here](https://gosspublic.alicdn.com/images/AppCallbackServer.zip).
    -   To run the sample program, decompress the downloaded package and run the java -jar oss-callback-server-demo.jar 9000 command. 9000 is the default port. You can change the port number.

        **Note:** The JAR package example runs on JDK 1.7. If problems occur, modify the code based on the code sample. This package contains the code for a Maven project.

-   PHP
    -   Download link: [Click here](https://gosspublic.alicdn.com/callback-php-demo.zip).
    -   To run the sample program, decompress the downloaded package to an Apache environment. This specific environment is required to obtain some headers due to PHP-specific features. Modify the sample code based on the actual environment.
-   Python
    -   Download link: [Click here](https://gosspublic.alicdn.com/images/callback_app_server.py.zip).
    -   To run the sample program, decompress the downloaded package and run the python callback\_app\_server.py command. This program implements a simple HTTP server. To run this program, you may need to install the dependency for RSA.
-   Ruby
    -   Download link: [Click here](https://github.com/rockuw/oss-callback-server).
    -   To run the sample program, run the ruby aliyun\_oss\_callback\_server.rb command.

