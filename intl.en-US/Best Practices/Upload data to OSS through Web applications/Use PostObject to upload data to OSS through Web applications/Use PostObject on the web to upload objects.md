# Use PostObject on the web to upload objects

This topic describes how to use form upload on the web to directly upload data to OSS.

When you use the web to upload an object, you can use the browser or app to upload the object to an application server. The application server uploads the object to OSS. During this process, the application server is necessary to transfer the object, which delivers lower transmission efficiency than direct data upload.

Direct upload allows you to call the [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md) operation provided by OSS to upload data by using form upload. The following cases describe how to directly upload data to OSS by using form upload:

-   On the client, add signatures by using JavaScript, and use form upload to upload data to OSS. For more information, see [Add signatures on the client by using JavaScript and upload data to OSS](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the client by using JavaScript and upload data to OSS.md).
-   Add signatures on the server. Use form upload to upload data to OSS. For more information, see [Add signatures on the server for object upload](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server for object upload.md).
-   Add signatures on the server. Configure upload callback on the server. Use form upload to upload data to OSS. After OSS receives the callback response, OSS returns the response to the client. For more information, see [Add signatures on the server, configure upload callback, and directly transfer data](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server, configure upload callback, and directly transfer data.md).

