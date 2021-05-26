# Installation

This topic describes how to install Object Storage Service \(OSS\) SDK for Browser.js.

## Preparations

-   A RAM user or a Security Token Service \(STS\) credential is created to access OSS.

    The AccessKey pair of an Alibaba Cloud account has permissions on all API operations. Therefore, we recommend that you follow the best practices of Alibaba Cloud for security. If you deploy your service on the server, you can use a RAM user or an STS credential to call API operations or perform routine operations. If you deploy your service on the client, use an STS credential to call API operations. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

-   A cross-origin resource sharing \(CORS\) rule is configured.

    To access OSS by using a browser, configure the CORS rule based on the following requirements:

    -   **Sources**: Specify an accurate domain name, such as `www.aliyun.com`. You can also specify a domain name that contains an asterisk \(\*\) as the wildcard. Example: `*.aliyun.com`.
    -   **Allowed Methods**: Select the methods based on your requirements. For example, you can select **PUT** to perform multipart upload and select **DELETE** to delete objects.
    -   **Allowed Headers**: Set the value to `*`.
    -   **Exposed Headers**: Set the values to `ETag`, `x-oss-request-id`, and `x-oss-version-id`.
    ![cors](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3416202261/p232679.jpg)


## Upload limits

OSS SDK for Browser.js uses [Browserify](http://browserify.org/) and [Babel](https://babeljs.io/) to generate and compile code for your browser. However, the following features are not supported in browsers:

-   Streaming upload: Chunked encoding cannot be configured in browsers. We recommend that you use multipart upload instead of streaming upload.
-   Local file management: Local file systems cannot be managed in browsers. We recommend that you use signed URLs to download objects.
-   OSS does not support bucket-related requests across regions. We recommend that you perform bucket-related operations in the OSS console.

## Download OSS SDK for Browser.js

Demos in documents on the official website are based on OSS SDK V6.X. For more information about versions earlier than 6.X, visit [oss-nodejs-sdk](https://github.com/ali-sdk/ali-oss/blob/5.x/README.md). To upgrade your SDK version to 6.X, see [What is OSS?](/intl.en-US/Product Introduction/What is OSS?.md).

-   [GitHub](https://github.com/ali-sdk/ali-oss)
-   [API documents](https://github.com/ali-sdk/ali-oss#summary)
-   [ChangeLog](https://github.com/ali-sdk/ali-oss/blob/master/CHANGELOG.md)

## Install OSS SDK for Browser.js

-   OSS SDK for Browser.js supports the following browsers:

    -   Internet Explorer 10 and later and Microsoft Edge
    -   Major Google Chrome, Firefox, and Safari versions
    -   Default browsers of Android, iOS, and Windows Phone of major versions
    You can install OSS SDK for Browser.js by using the following methods.

-   Installation methods
    -   Import OSS SDK for Browser.js by using a browser

        **Note:** Some browsers do not provide native support for promise. In this case, you must import a promise library. For example, you must import the promise-polyfill library for Internet Explorer 10 and 11.

        ```
        <!-- Import online resources -->
        <script src="https://gosspublic.alicdn.com/aliyun-oss-sdk-x.x.x.min.js"></script>                           
        ```

        ```
        <!-- Import local resources -->
        <script src="./aliyun-oss-sdk-x.x.x.min.js"></script>                           
        ```

        **Note:**

        -   In the preceding code, x.x.x specifies the version number of OSS SDK for Browser.js. For more information about the versions of OSS SDK for Browser.js, visit [GitHub](https://github.com/ali-sdk/ali-oss).
        -   If you import online resources, the speed depends on the stability of Alibaba Cloud Content Delivery Network \(CDN\) servers. We recommend that you import the SDK from local resources or build the SDK by yourself.
        The following code provides an example on how to use an `OSS` object:

        ```
        <script type="text/javascript">
          let client = new OSS({
            // Set yourRegion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
            region: 'yourRegion',
            // Obtain a temporary access credential from STS. The temporary access credential contains a security token and a temporary AccessKey pair that consists of an AccessKey ID and an AccessKey secret. 
            accessKeyId: 'yourAccessKeyId',
            accessKeySecret: 'yourAccessKeySecret',
            stsToken: 'yourSecurityToken',
            // Specify the bucket name. 
            bucket: 'examplebucket'
          });
        </script>
                                    
        ```

    -   Use [npm](https://www.npmjs.com/) to install the package of OSS SDK for Browser.js

        ```
        npm install ali-oss                           
        ```

        After the package is installed, you can use the import or require syntax to use OSS SDK for Browser.js. Browsers do not provide native support for the `require` syntax. Therefore, you must use a packaging tool such as `webpack` or `browserify` in the development environment.

        **Note:**

        For more information about how to set up an STS, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md) in OSS Developer Guide. You can call the [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md) operation or use [STS SDKs in various programming languages](/intl.en-US/SDK Reference/STS SDK Reference/STS SDK overview.md) to obtain the temporary access credential.

        ```
        let OSS = require('ali-oss');
        
        let client = new OSS({
            // Set yourRegion to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourRegion to oss-cn-hangzhou. 
            region: 'yourRegion',
            // Obtain a temporary access credential from STS. The temporary access credential contains a security token and a temporary AccessKey pair that consists of an AccessKey ID and an AccessKey secret. 
            accessKeyId: 'yourAccessKeyId',
            accessKeySecret: 'yourAccessKeySecret',
            stsToken: 'yourSecurityToken',
            // Specify the bucket name. 
            bucket: 'examplebucket'
        });
        ```


## Use OSS SDK for Browser.js

You can use the synchronous or asynchronous mode when you use OSS SDK for Browser.js.

-   Synchronous mode: The `async/await` method defined in JavaScript ES7 is used to synchronize asynchronous operations.
-   Asynchronous mode: Asynchronous operations are performed in a way similar to callback. An API operation returns a [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) object for a request. The `then()` method is used to process the returned result, and the `catch()` method is used to handle errors.

**Note:** `new OSS()` is used to create OSS clients when the synchronous or asynchronous mode is used.

The following code provides examples on how to upload and download an object in synchronous and asynchronous modes:

-   Synchronous mode

    ```
    // Create an OSSClient instance. 
    let client = new OSS(...);
    
    async function put () {
      try {
        // object specifies the name of the uploaded object. 
        // file specifies the name of the file that you want to upload from the browser. Files of the HTML5 and Blob types are supported. 
        let r1 = await client.put('object', file);
        console.log('put success: %j', r1);
        let r2 = await client.get('object');
        console.log('get success: %j', r2);
      } catch (e) {
        console.error('error: %j', e);
      }
    }
    
    put();                    
    ```

-   Asynchronous mode

    ```
    // Create an OSSClient instance. 
    let client = new OSS(...);
    
    // object specifies the name of the uploaded object. 
    // file specifies the name of the file that you want to upload from the browser. Files of the HTML5 and Blob types are supported. 
    client.put('object', file).then(function (r1) {
      console.log('put success: %j', r1);
      return client.get('object');
    }).then(function (r2) {
      console.log('get success: %j', r2);
    }).catch(function (err) {
      console.error('error: %j', err);
    });                    
    ```


