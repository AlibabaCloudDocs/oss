# Set up direct data transfer for mobile apps

This topic describes how to set up a direct data transfer service for a mobile app within 30 minutes by using the STS policy. Direct data transfer allows a mobile app to directly connect to OSS for data uploads and downloads, while only the control flow is sent to the app server.

OSS is activated. One or more buckets are created.

-   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
-   For more information about how to create a bucket, see [Create buckets](/intl.en-US/Developer Guide/Buckets/Create buckets.md).

In the mobile Internet era, increasing amounts of data are uploaded through mobile apps. Developers can hand off their data storage concerns to OSS and focus more on the development of app logic.

OSS-based direct data transfer for mobile apps has the following benefits:

-   Data security: allows mobile apps to upload and download data based on flexible authorization and authentication methods, which delivers better security.
-   Cost-effectiveness: Fewer app servers are required, which minimizes costs. The mobile app is directly connected to OSS for data uploads and downloads, while only the control flow is sent to the app server.
-   High concurrency: supports concurrent access from a large number of users.
-   Elastic scalability: provides storage space that can be scaled as needed.
-   Data processing: used with Image Processing \(IMG\) and ApsaraVideo Media Processing to facilitate flexible data processing.

## Process

Role analysis:

-   Android or iOS app: the app on the user's mobile phone that applies for and uses STS credentials from the app server.
-   OSS: processes data requests from mobile apps.
-   RAM/STS: generates temporary upload credentials \(or tokens\).
-   App server: the backend service developed for Android or iOS apps. The app server is used to manage the tokens used for data uploads and downloads by using the app and the metadata of the app-uploaded data.

Procedure:

1.  The mobile app applies for a temporary upload credential from the app server.

    AccessKey pairs cannot be stored in Android or iOS apps due to security concerns. Therefore, the app must apply for a token from the app server. The token is valid only for a specified period of time. If the app server developer sets the validity period of the token to 30 minutes, the Android or iOS app can use this token to upload data to or download data from OSS within 30 minutes after the token is issued. After 30 minutes, the app must request a new token to upload or download data.

2.  The app server checks the validity of the request and then returns a token to the app.

3.  The Android or iOS app uses this token to upload data to or download data from OSS.


The following section describes how to use the app server to generate a token and how to use the Android or iOS app to obtain a token.

## Step 1: Activate STS

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Overview**.

3.  In the **Basic Settings** section, choose **Security Token \(Authorization for RAM User\)** \> **RAM Console**.

4.  Click **Start Authorization**. Complete authorization.

5.  After authorization is complete, save the AccessKey ID, AccessKey secret, and RoleArn parameters.

    -   If you have not created an AccessKey pair, the system automatically creates an AccessKey pair. Save AccessKey ID, AccessKey secret, and RoleArn.
    -   If you have created an AccessKey pair, click **View** to create an AccessKey pair.

## Step 2: Configure the app server

1.  Download the app server code.

    |Programming language|Download link|
    |--------------------|-------------|
    |PHP|[Download link](https://gosspublic.alicdn.com/doc31920servercode/sts-server.zip)|
    |Java|[Download link](https://gosspublic.alicdn.com/doc31920servercode/AppTokenServerDemo.zip)|
    |Ruby|[Download link](https://gosspublic.alicdn.com/doc31920servercode/sts-app-server-master.zip)|
    |Node.js|[Download link](https://gosspublic.alicdn.com/doc31920servercode/sts-app-server-node.zip)|
    |Go|[Download link](https://gosspublic.alicdn.com/doc31920servercode/test_token_server.zip)|

2.  Modify the configuration file.

    The following code sample is written in PHP. Each downloaded package for a specific programming language contains a config.json configuration file, which contains the following information:

    ```
    {
    "AccessKeyID" : "",
    "AccessKeySecret" : "",
    "RoleArn" : "",
    "TokenExpireTime" : "900",
    "PolicyFile": "policy/bucket_write_policy.txt"
    }
    ```

    Parameters:

    -   AccessKeyID: Enter the obtained AccessKey ID.
    -   AccessKeySecret: Enter the obtained AccessKey secret.
    -   RoleArn: Enter the obtained RoleArn.
    -   TokenExpireTime: specifies the validity period of the token obtained by using the Android or iOS app. The minimum value is the same as the default value, which is 900s.
    -   PolicyFile: specifies the file that lists the permissions the token grants. The default value can be retained.
    This topic describes two token files that define the most common permissions in the policy directory. To use the token file, replace `$BUCKET_NAME` and `$OBJECT_PREFIX` with specified values.

    -   bucket\_read\_policy.txt: specifies that the token grants permissions to read data from the specified bucket and objects whose names contain the specified prefix for the account.
    -   bucket\_write\_policy.txt: specifies that the token grants permissions to write data to the specified bucket and objects whose names contain the specified prefix for the account.
    For more information about permissions, see examples and implementation of policies in [Overview](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Overview.md). Follow the principle of least privilege \(PoLP\) based on business requirements. Security risks such as data leaks may arise because of excess permissions if you specify all resources \(resource:\*\) or all actions \(action:\*\).

    **Warning:** The code sample is only for reference. In practice, the online system must implement permission isolation for different users or devices. This way, tokens that grant different permissions are generated to avoid excess permissions.

3.  Run the code sample.

    Run the code sample:

    -   Run the sample code in PHP: After the package is downloaded and decompressed, modify the config.jsonfile. Run the php sts.php command to generate a token. Deploy the program to the app server address.
    -   Run the sample code that depends on Java 1.7 in Java: After the package is downloaded and decompressed, modify the config.json file. Reopen the package and run the java -jar app-token-server.jar \(port\) command. If the port is not specified, run the java -jar app-token-server.jar command. The program starts listening on port 7080.
    Sample responses:

    ```
    // Sample success responses.
    {
        "StatusCode":200,
        "AccessKeyId":"STS.3p***dgagdasdg",
        "AccessKeySecret":"rpnwO9***tGdrddgsR2YrTtI",
       "SecurityToken":"CAES+wMIARKAAZhjH0EUOIhJMQBMjRywXq7MQ/cjLYg80Aho1ek0Jm63XMhr9Oc5s˙∂˙∂3qaPer8p1YaX1NTDiCFZWFkvlHf1pQhuxfKBc+mRR9KAbHUefqH+rdjZqjTF7p2m1wJXP8S6k+G2MpHrUe6TYBkJ43GhhTVFMuM3BZajY3VjZWOXBIODRIR1FKZjIiEjMzMzE0MjY0NzM5MTE4NjkxMSoLY2xpZGSSDgSDGAGESGTETqOio6c2RrLWRlbW8vKgoUYWNzOm9zczoqOio6c2RrLWRlbW9KEDExNDg5MzAxMDcyNDY4MThSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzMzMTQyNjQ3MzkxMTg2OTExcglzZGstZGVtbzI=",
       "Expiration":"2017-12-12T07:49:09Z",
    }
    // Sample error responses.
    {
        "StatusCode":500,
        "ErrorCode":"InvalidAccessKeyId.NotFound",
        "ErrorMessage":"Specified access key is not found."
    }
    ```

    A token in a sample success response consists of the following variables:

    -   StatusCode: the status of the operation to obtain the token. If the token is obtained, 200 is returned.
    -   AccessKeyId: the AccessKey ID obtained when the Android or iOS app initializes OSSClient.
    -   AccessKeySecret: the AccessKey secret obtained when the Android or iOS app initializes OSSClient.
    -   SecurityToken: the token obtained when the Android or iOS app initializes OSSClient.
    -   Expiration: the time when the token expires. The Android SDK determines whether the token is valid. If the token becomes invalid, another token is automatically obtained.
    Error codes:

    -   StatusCode: indicates the status of the operation to obtain the token. If the token fails to be obtained, 500 is returned.
    -   ErrorCode: indicates the cause of the error.
    -   ErrorMessage: indicates the specific description of the error.

## Step 3: Download and install the mobile app

1.  Download the app source code.

    Download link:

    -   Android: [Download](https://github.com/aliyun/aliyun-oss-android-sdk?spm=a2c4g.11186623.2.9.FXb5vt)
    -   iOS: [Download](https://github.com/aliyun/aliyun-oss-ios-sdk?spm=a2c4g.11186623.2.10.FXb5vt)
    You can use either of the mobile apps to upload images to OSS. Simple upload and resumable upload are supported. If the network quality is poor, we recommend that you use resumable upload. You can also use Image Processing \(IMG\) to resize an image to obtain a thumbnail and add watermarks to the image.

2.  Open the mobile app and configure the app parameters.

    -   App server: the app server address specified in [Step 2: Configure the app server](#section_jbo_g46_l3p).
    -   Destination bucket: the bucket to which the data is uploaded from a mobile app.
    -   Region: the region where the destination bucket is located.
    -   OSS object name: The name must contain the prefix specified in the policy configuration file of the app server.
3.  Click **Set** to load configurations.


## Step 4: Implement direct data transfer for mobile apps

1.  Open the mobile app.

2.  Click **Select Image**. Select the image to upload and configure the object name.

3.  After the object upload is complete, view the upload result in the console.


## Core code analysis

The code for OSS initialization:

-   Android version

    ```
    // We recommend that you use OSSAuthCredentialsProvider. The token is automatically updated after it expires.
    String stsServer = "App server address such as http://abc.com:8080"
    OSSCredentialProvider credentialProvider = new OSSAuthCredentialsProvider(stsServer);
    //config
    ClientConfiguration conf = new ClientConfiguration();
    conf.setConnectionTimeout(15 * 1000); // The connection timeout period in seconds. Default value: 15.
    conf.setSocketTimeout(15 * 1000); // The socket timeout period in seconds. Default value: 15.
    conf.setMaxConcurrentRequest(5); // The maximum number of concurrent requests. Default value: 5.
    conf.setMaxErrorRetry(2); // The maximum number of retry attempts. Default value: 2.
    OSS oss = new OSSClient(getApplicationContext(), endpoint, credentialProvider, conf);
    ```

-   iOS version

    ```
    OSSClient * client;
    ...
    // We recommend that you use OSSAuthCredentialProvider. The token is automatically updated after it expires.
    id<OSSCredentialProvider> credential = [[OSSAuthCredentialProvider alloc] initWithAuthServerUrl:@"App server address such as http://abc.com:8080"];
    client = [[OSSClient alloc] initWithEndpoint:endPoint credentialProvider:credential];
    ```


