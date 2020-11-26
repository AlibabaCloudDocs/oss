# Add signatures on the server for object upload

This topic describes how to add signatures on the server and use form upload to upload data to OSS.

**Note:** You cannot add signatures on the server for multipart upload and resumable upload.

## Background information

Security risks may arise when you use JavaScript-based signatures on the client for object upload because AccessKey IDs and AccessKey secrets may be exposed on the frontend. For more information about JavaScript-based signatures on the client for object upload, see [Add signatures on the client by using JavaScript and upload data to OSS](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the client by using JavaScript and upload data to OSS.md). To minimize the risks, OSS allows you to add signatures on the server for object upload.

## Process

In this example, the client requests a signature from the server and directly uploads an object. This method is secure and reliable. However, the server cannot obtain information such as the number of objects the user uploads and the names of the objects. To stay updated on the information, add signatures on the server, configure upload callback, and directly upload data. For more information, see [Add signatures on the server, configure upload callback, and directly transfer data](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server, configure upload callback, and directly transfer data.md).

## Process and code analysis

The source code of adding signatures on the server for object upload is similar to that of adding signatures on the server, configuring upload callback, and directly uploading data. For more information, see [Add signatures on the server, configure upload callback, and directly transfer data](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server, configure upload callback, and directly transfer data.md).

## Examples

For more information about the code for various programming languages that is used to add signatures on the server, configure upload callback, and directly upload data, see the following topics:

-   [Java]()
-   [Python]()
-   [PHP]()
-   [Go]()
-   [Ruby]()
-   [.NET]()

This example adds signatures on the server and uploads data without performing upload callback. To add signatures on the server and upload data, you need only to open the `upload.js` file in the downloaded source code of the client, and find the following snippet:

```
{
  'key' : g_object_name,
  'policy': policyBase64,
  'OSSAccessKeyId': accessid,
  'success_action_status' : '200', // Set success_action_status to 200 so that 200 is returned if the request is successful. If this parameter is not specified, 204 is returned.
  'callback' : callbackbody,
  'signature': signature,
}
```

Comment out `'callback' : callbackbody` to disable the upload callback function. This way, you can add signatures on the server and upload data without performing upload callback.

**Note:** To enable the upload callback function, ensure that the `callbackbody` value is correctly calculated. For more information, see [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md).

