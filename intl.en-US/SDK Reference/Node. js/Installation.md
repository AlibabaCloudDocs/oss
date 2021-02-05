# Installation

This topic describes how to install OSS SDK for Node.js.

## Prerequisites

-   Alibaba Cloud OSS is activated.
-   The AccessKey ID and AccessKey secret of the RAM user are created.

    Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use the AccessKey pair of a RAM user. If you deploy your service on the server, you can use a RAM user or STS credentials to call API operations or perform routine operations. If you deploy your service on the client, use STS credentials to call API operations. For more information, see [Resource Access Management](https://www.alibabacloud.com/help/product/28625.htm).


## Background information

OSS SDK for Node.js is built based on the [Node.js](https://nodejs.org/) environment.

Example code in the official document is from OSS SDK for Node.js 6.x. For more information about OSS SDK for Node.js earlier than 6.x, visit [OSS SDK for Node.js 5.x](https://github.com/ali-sdk/ali-oss/blob/5.x/README.md). For more information about how to upgrade OSS SDK for Node.js to 6.x, visit [Upgrading Notes \(5.x to 6.x\)](https://github.com/ali-sdk/ali-oss/blob/master/UPGRADING.md).

For more information about the source code of OSS SDK for Node.js, visit [GitHub](https://github.com/ali-sdk/ali-oss). For more information, visit [documents](https://github.com/ali-sdk/ali-oss#summary).

## Install OSS SDK for Node.js

OSS supports Node.js 8 and later. To use OSS SDK for Node.js in Node.js versions earlier than 8.0.0, use OSS SDK for Node.js V4.x.

Run npm install ali-oss --save to install the SDK package. For more information, visit [npm](https://www.npmjs.com/).

If you encounter any network problems when you use npm, we recommend that you use [cnpm](https://npm.taobao.org/), which is the npm image provided by Taobao.

## Use OSS SDK for Node.js

You can use OSS SDK for Node.js in synchronous and asynchronous modes. `new OSS()` is used to create OSS clients in synchronous and asynchronous modes.

-   Synchronous mode: The `async` and `await` methods are used to synchronize asynchronous operations.
-   Asynchronous mode: Perform asynchronous operations in a way similar to callback. The API operation returns a [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise) object for a request. The `.then()` method is used to process the returned result, and the `.catch()` method is used to handle errors.

The following code provides an example on how to upload an object in synchronous mode and download an object in asynchronous mode:

-   Synchronous mode

    ```
    let client = new OSS(...);
    async function put () {
      try {
        // object specifies the name of the object uploaded to OSS. localfile specifies the name of the local file or file path.
        let r1 = await client.put('object','localfile'); 
        console.log('put success: %j', r1);
        let r2 = await client.get('object');
        console.log('get success: %j', r2);
      } catch(e) {
        console.error('error: %j', e);
      }
    }
    put();
    ```

-   Asynchronous mode

    ```
    let client = new OSS(...);
    
    // object specifies the name of the object downloaded from OSS. localfile specifies the name of the local file or file path.
    client.put('object', 'localfile').then(function (r1) {
      console.log('put success: %j', r1);
      return client.get('object');
    }).then(function (r2) {
      console.log('get success: %j', r2);
    }).catch(function (err) {
      console.error('error: %j', e);
    });
                        
    ```


