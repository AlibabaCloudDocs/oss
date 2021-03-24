# Installation

This topic describes how to install OSS SDK for Browser.js.

## Download OSS SDK for Browser.js

The demos included in the documentation provided on the Alibaba Cloud official site are based on the 6.x version of OSS SDK for Browser.js. If your version of OSS SDK for Browser.js is earlier than 6.x, visit [GitHub](https://github.com/ali-sdk/ali-oss/blob/5.x/README.md) to see the Developer Guide for 5.x versions. To upgrade OSS SDK for Browser.js to the 6.x version, see the [upgrade documentation](/intl.en-US/Product Introduction/What is OSS?.md).

-   [GitHub](https://github.com/ali-sdk/ali-oss)
-   [API documents](https://github.com/ali-sdk/ali-oss#summary)
-   [ChangeLog](https://github.com/ali-sdk/ali-oss/blob/master/CHANGELOG.md)

## Environment

OSS SDK for Browser.js uses [Browserify](http://browserify.org/) and [Babel](https://babeljs.io/) to generate and complie code inside your browser. However, the following features are not supported in browsers:

-   Streaming upload: Chunked encoding cannot be configured in browsers. We recommend that you use multipart upload instead of streaming upload.
-   On-premises file management: On-premises file systems cannot be managed in browsers. We recommend that you use signed URLs to download objects.
-   OSS does not support bucket-related requests across regions. We recommend that you perform bucket-related operations in the OSS console.

## Prerequisites

-   A RAM user or an STS credential is created to access OSS.

    The AccessKey pair of an Alibaba Cloud account has permissions on all API operations. Therefore, we recommend that you follow the best security practices of Alibaba Cloud. If you deploy your service on the server, you can use a RAM user or an STS credential to call API operations or perform routine operations. If you deploy your service on the client, use an STS credential to call API operations. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

-   A cross-origin resource sharing \(CORS\) rule is configured.

    To use a browser to access OSS, configure the CORS rule based on the following requirements:

    -   **Sources**: Specify an exact domain name, such as `www.aliyun.com`. You can also specify a wildcard domain name by using an asterisk \(\*\), such as `*.aliyun.com`.
    -   **Allowed Methods**: Select the methods based on your requirements. For example, you can select **PUT** if you want to perform multipart upload and select **DELETE** if you want to delete objects.
    -   **Allowed Headers**: Set the value to `*`.
    -   **Exposed Headers**: Set the value to `ETag`, `x-oss-request-id`, and `x-oss-version-id`.
    ![cors](../images/p232679.jpg)


## Install OSS SDK for Browser.js

-   OSS SDK for Browser.js supports the following browsers:

    -   Internet Explorer 10 and later versions and Microsoft Edge
    -   Major Google Chrome/Firefox/Safari versions
    -   The default browsers of official Android/iOS/Windows Phone releases
    You can install OSS SDK for Browser.js by using the following methods:

-   Installation methods
    -   Import OSS SDK for Browser.js by using the browser

        **Note:** Some browsers do not provide native support for promise. In this case, you must import a promise library. For example, you must import the promise-polyfill library for Internet Explorer 10 and 11.

        ```
        <! -- Import online resources -->
        <script src="https://gosspublic.alicdn.com/aliyun-oss-sdk-x.x.x.min.js"></script>
                                    
        ```

        ```
        <! -- Import on-premises resources  -->
        <script src="./aliyun-oss-sdk-x.x.x.min.js"></script>
                                    
        ```

        **Note:**

        -   In the preceding code, x.x.x indicates the version number of OSS SDK for Browser.js. For more information about the versions of OSS SDK for Browser.js, visit [GitHub](https://github.com/ali-sdk/ali-oss).
        -   When you import online resources, the download speed may be affected by the network quality of Alibaba Cloud Content Delivery Network \(CDN\) servers. We recommend that you import the SDK from on-premises disks or build the SDK by yourself.
        The following code provides an example on how to use an `OSS` object:

        ```
        <script type="text/javascript">
          let client = new OSS({
            region: '<oss region>',
            // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you create and use STS credentials to call API operations following the security best practices.
            accessKeyId: '<Your accessKeyId(STS)>',
            accessKeySecret: '<Your accessKeySecret(STS)>',
            stsToken: '<Your securityToken(STS)>',
            bucket: '<Your bucketName>'
          });
        </script>
                                    
        ```

    -   Use [npm](https://www.npmjs.com/) to install the package of OSS SDK for Browser.js

        ```
        npm install ali-oss
                                    
        ```

        After the installation is complete, you can use the import or require syntax to use OSS SDK for Browser.js. The `require` syntax is not supported by browsers. Therefore, you must use package mangers such as `webpack` or `Browserify` in the development environment.

        ```
        let OSS = require('ali-oss');
        
        let client = new OSS({
          region: '<oss region>',
            // Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you follow the best practices for security to create and use STS credentials to call API operations.
            accessKeyId: '<Your accessKeyId(STS)>',
            accessKeySecret: '<Your accessKeySecret(STS)>',
            stsToken: '<Your securityToken(STS)>',
          bucket: '<Your bucketName>'
        });
        ```

    -   Use [Bower](http://bower.io/) to install OSS SDK for Browser.js

        For web projects, you can also use [Bower](http://bower.io/) to install OSS SDK for Browser.js.

        ```
        bower install ali-oss
                                    
        ```

        Add the following field to the web page:``

        ```
        <script src="bower_components/ali-oss/dist/aliyun-oss-sdk.min.js"></script>
                                    
        ```


## Use OSS SDK for Browser.js

You can use OSS SDK for Browser.js in synchronous and asynchronous modes.

-   Synchronous mode: The `async/await` method defined in JavaScript ES7 are used to synchronize asynchronous operations.
-   Asynchronous mode: Asynchronous operations are performed in a way similar to callback. The API operation returns a [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) object for a request. The `.then()` method is used to process the returned result, and the `.catch()` method is used to handle errors.

**Note:** `new OSS()` is used to create OSSClient instances in synchronous and asynchronous modes.

The following code provides an example on how to upload and download an object in synchronous and asynchronous modes:

-   Synchronous mode

    ```
    // Create an OSSClient instance.
    let client = new OSS(...);
    
    async function put () {
      try {
        // object indicates the name of the uploaded object.
        // file indicates the name of the file that you want to upload in the browser. Supports HTML5 and Blob files.
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
    
    // object indicates the name of the uploaded object.
    // file indicates the name of the file that you want to upload in the browser. Supports HTML5 and Blob files.
    client.put('object', file).then(function (r1) {
      console.log('put success: %j', r1);
      return client.get('object');
    }).then(function (r2) {
      console.log('get success: %j', r2);
    }).catch(function (err) {
      console.error('error: %j', err);
    });
                        
    ```


