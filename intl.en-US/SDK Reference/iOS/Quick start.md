# Quick start

This topic describes how to get started with OSS SDK for iOS.

For more information, see the following examples and cases of this project:

-   [iOS demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/Example/AliyunOSSSDK-iOS-Example)
-   [Mac demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/Example/AliyunOSSSDK-OSX-Example)
-   [Swift demo](https://github.com/aliyun/aliyun-oss-ios-sdk/tree/master/OSSSwiftDemo)
-   [Test cases](https://github.com/aliyun/AliyunOSSiOS/tree/master/AliyunOSSiOSTests)

You can also run the git clone [https://github.com/aliyun/aliyun-oss-ios-sdk](https://github.com/aliyun/aliyun-oss-ios-sdk) command and configure the following parameters:

![ios](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3796498951/p88591.png)

Run the project demo as shown in the following figure.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4796498951/p13694.png)

## Examples

The following code shows you how to upload and download files on the iOS and Mac platforms.

-   iOS platform
    1.  Add the reference.

        ```
        #import <AliyunOSSiOS/OSSService.h>                           
        ```

    2.  Initialize the OSSClient instance.

        To initialize an OSSClient instance, you must configure the Endpoint field, authentication mode, and Client parameters. Three authentication modes are available: plaintext setting, self-signed, and STS authentication. For more information about how to use the STS authentication mode and learn about the basic knowledge of RAM, see [Authorized access](/intl.en-US/SDK Reference/iOS/Authorized access.md). The following code assumes that you have activated RAM and are already familiar with basic operations such as how to obtain the AccessKey ID, AccessKey secret, and role ID of a RAM user.

        Set the AccessKeyId, SecretKeyId, and RoleArn parameters, as shown in [Script files](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/app/sts_local_server/python/sts.py). You can use OSS SDK for Python to start a local HTTP service. Use the client code to access the local service and obtain the StsToken.AccessKeyId, StsToken.SecretKeyId, and StsToken.SecurityToken field values.

        ```
        NSString *endpoint = @"https://oss-cn-hangzhou.aliyuncs.com";
        
        // We recommend that you use STS to initialize the OSSClient instance if you use a mobile device.
        id<OSSCredentialProvider> credential = [[OSSFederationCredentialProvider alloc] initWithFederationTokenGetter:^OSSFederationToken * _Nullable{
            OSSFederationToken *token = [OSSFederationToken new];
            token.tAccessKey = @"AccessKeyId";
            token.tSecretKey = @"AccessKeySecret";
            token.tToken = @"SecurityToken";
            token.expirationTimeInGMTFormat = @"Expiration";
            return token;
        }];
        
        client = [[OSSClient alloc] initWithEndpoint:endpoint credentialProvider:credential];  
                                    
        ```

        The use of the OSSClient instance to initiate upload and download requests is thread-safe. You can run multiple tasks concurrently.

    3.  Upload a file.

        Assume that you have created a bucket in the console. An `OSSTask` is returned for each SDK-relevant operation. You can configure a continuation for the task to be completed asynchronously or call `waitUntilFinished` to wait for the task to complete.

        ```
        OSSPutObjectRequest * put = [OSSPutObjectRequest new];
        put.bucketName = @"<bucketName>";
        // objectKey is equivalent to objectName and indicates the complete path of the object that you want to upload to OSS. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
        put.objectKey = @"<objectKey>";
        put.uploadingData = <NSData *>; // Directly upload NSData.
        put.uploadProgress = ^(int64_t bytesSent, int64_t totalByteSent, int64_t totalBytesExpectedToSend) {
            NSLog(@"%lld, %lld, %lld", bytesSent, totalByteSent, totalBytesExpectedToSend);
        };
        OSSTask * putTask = [client putObject:put];
        [putTask continueWithBlock:^id(OSSTask *task) {
            if (! task.error) {
                NSLog(@"upload object success!") ;
            } else {
                NSLog(@"upload object failed, error: %@" , task.error);
            }
            return nil;
        }];
        // Wait until the task is complete.
        // [putTask waitUntilFinished];
        ```

    4.  Download the specified object.

        The following code provides an example on how to download the specified object as `NSData`:

        ```
        OSSGetObjectRequest * request = [OSSGetObjectRequest new];
        request.bucketName = @"<bucketName>";
        // objectKey is equivalent to objectName and indicates the complete path of the object that you want to download from OSS. The path must include the file extension of the object. For example, you can set objectKey to abc/efg/123.jpg.
        request.objectKey = @"<objectKey>";
        request.downloadProgress = ^(int64_t bytesWritten, int64_t totalBytesWritten, int64_t totalBytesExpectedToWrite) {
            NSLog(@"%lld, %lld, %lld", bytesWritten, totalBytesWritten, totalBytesExpectedToWrite);
        };
        OSSTask * getTask = [client getObject:request];
        [getTask continueWithBlock:^id(OSSTask *task) {
            if (! task.error) {
                NSLog(@"download object success!") ;
                OSSGetObjectResult * getResult = task.result;
                NSLog(@"download result: %@", getResult.downloadedData);
            } else {
                NSLog(@"download object failed, error: %@" ,task.error);
            }
            return nil;
        }];
        // Wait until the task is complete.
        // [task waitUntilFinished];
        ```

-   Mac platform

    Aside from the import method, all operations on the Mac platform are the same as those on the iOS platform.

    ```
    import <AliyunOSSOSX/AliyunOSSiOS.h>                    
    ```


