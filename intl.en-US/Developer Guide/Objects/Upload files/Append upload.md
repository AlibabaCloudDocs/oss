# Append upload

To use append upload, you can call the AppendObject operation of OSS to directly append content to the append objects that are uploaded.

**Note:** For more information about the AppendObject operation, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Scenarios

By using [simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md), [form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md), and [resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md), you can only create normal objects whose content remain unchanged after the upload is completed. They can only be read but cannot be modified. To modify the object content, you must upload an object that has the same name to overwrite the existing object. This feature is a major difference between OSS and typical file systems.

Video data is constantly generated in real time. Therefore, this feature is unsuitable for scenarios such as video surveillance and live video streaming. By using these upload methods, you must split a video stream into small parts and then upload them as new objects. In actual use, these methods have obvious defects:

-   The software architecture is complex. You must consider intricate issues such as object splitting.
-   You must reserve space to store metadata such as the list of created objects. After OSS receives each request, OSS must repeatedly read the metadata and determine whether to create an object. This puts high pressure on the server. In addition, the client must send each request twice, which causes latency.
-   If parts are small, the latency is low. However, too many objects are hard to manage. If parts are large, the latency is high.

To simplify development and reduce costs in such scenarios, OSS allows you to use append upload to append content directly to the end of an object. Objects uploaded by using this method are append objects, whereas objects uploaded by using other methods are normal objects. The appended data can be read immediately after the upload.

By using append upload, the architecture becomes simple in the preceding scenarios. When video data is generated, it is immediately added to the same object by using append upload. The client needs only to regularly retrieve the object length and compare the length with the previous value. If new readable data is found, the client starts a read operation to retrieve the newly uploaded data. This method greatly simplifies the architecture and enhances the scalability.

In addition to video scenarios, append upload can be used to append log data.

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/appendfromfile.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Append upload.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Append upload.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Upload objects/Append upload.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Upload objects/Append upload.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Append upload.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Upload objects/Append upload.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Upload objects/Append upload.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Upload objects/Append upload.md)|

## Upload limits

-   Size: The maximum size of an object is 5 GB in this mode.
-   Naming conventions:
    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 1,023 bytes in length.
    -   The name cannot start with a forward slash \(/\) or backslash \(\\\).
-   Object type: You can append data to objects created only by using append upload. Therefore, you cannot append data to objects created by using simple upload, form upload, or multipart upload.
-   Subsequent operations: You cannot copy objects created by using append upload. However, you can modify the object metadata.

## Security and authorization

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

To authorize third-party users to upload objects, OSS also provides account-based authorization. For more information, see [Authorized third-party upload](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).

## What to do next

Images can be processed after they are uploaded. For more information, see [IMG user guide](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

**Note:** Append upload does not support upload callback.

