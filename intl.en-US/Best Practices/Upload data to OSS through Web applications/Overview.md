# Overview

This topic describes how to use frontend web technologies to upload objects to OSS.

## Background information

Each OSS user needs to upload data to OSS. When you use the web to upload an object, you can use the browser or app to upload the object to an application server. The application server uploads the object to OSS.

Compared with direct data transfer, the preceding method has the following disadvantages:

-   Time-consuming: Users need to upload data to the application server before the data can be uploaded to OSS. The time used for network transmission is twice as long as that for direct data transfer. Performance is improved if users directly transfer data to OSS without the need to transfer the data to the application server first. OSS uses the Border Gateway Protocol \(BGP\) network to ensure the speed of transmission between different Internet service providers \(ISPs\).
-   Poor scalability: The increase of users can cause a bottleneck on the application servers.
-   High cost: Multiple application servers must be prepared. You can upload data to OSS free of charge. If you directly upload data to OSS, you can save costs on application servers.

## Technical solutions

The following solutions are available when you use the frontend web technologies to upload objects to OSS:

-   Use OSS SDK for Browser.js to upload objects to OSS

    This solution implements direct data transfer to OSS by using OSS SDK for Browser.js. For more information, see [Upload objects](/intl.en-US/SDK Reference/Browser.js/Upload objects.md). You can use resumable upload to upload large objects when the network conditions are poor. When you use this solution, you must consider compatibility. The following browsers are supported: IE 10 and later, mainstream versions of Microsoft Edge, Google Chrome, Firefox, and Safari, and browsers for mobile phones that support Android, iOS, and Windows Phone. For more information, see [Installation](/intl.en-US/SDK Reference/Browser.js/Installation.md).

-   Use form upload to upload objects to OSS

    Use the [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md) operation provided by OSS to upload an object to OSS by using form upload. This solution supports most browsers. When you fail to upload an object under poor network conditions, you can only retry the upload. For more information, see [Use PostObject on the web to upload objects](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Use PostObject on the web to upload objects.md).


