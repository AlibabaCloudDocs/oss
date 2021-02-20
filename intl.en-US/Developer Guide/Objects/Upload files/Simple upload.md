# Simple upload

For simple upload, you can call the PutObject operation to upload single objects. You can use simple upload in scenarios where you want to upload an object smaller than 5 GB by sending one HTTP request.

**Note:** For more information about PutObject, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.md).

## Implementation modes

|Implementation mode|Description|
|-------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload an object.md)|A user-friendly and intuitive web application|
|[ossbrowser](/intl.en-US/Tools/ossbrowser/Quick start.md)|An easy-to-operate graphical tool|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md)|A high-performance command-line tool|
|[Java SDK](/intl.en-US/SDK Reference/Java/Upload objects/Simple upload.md)|SDK demos for various programming languages|
|[Python SDK](/intl.en-US/SDK Reference/Python/Upload objects/Simple upload.md)|
|[PHP SDK](/intl.en-US/SDK Reference/PHP/Upload objects/Simple upload.md)|
|[Go SDK](/intl.en-US/SDK Reference/Go/Upload objects/Simple upload.md)|
|[C SDK](/intl.en-US/SDK Reference/C/Upload objects/Simple upload.md)|
|[.NET SDK](/intl.en-US/SDK Reference/. NET/Upload objects/Simple upload.md)|
|[Android SDK](/intl.en-US/SDK Reference/Android/Upload objects/Simple upload.md)|
|[iOS SDK](/intl.en-US/SDK Reference/iOS/Upload objects/Simple upload.md)|
|[Node.js SDK](/intl.en-US/SDK Reference/Node. js/Upload objects/Upload a local file.md)|
|[Browser.js SDK](/intl.en-US/SDK Reference/Browser.js/Upload objects.md)|
|[Ruby SDK](/intl.en-US/SDK Reference/Ruby/Upload objects.md)|

## Usage notes

-   Limits on object size

    You cannot upload objects larger than 5 GB. If objects you want to upload is larger than 5 GB, use [resumable upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md).

-   Naming conventions
    -   The name can contain only UTF-8 characters.
    -   The name must be 1 to 1,023 bytes in length.
    -   Object names cannot start with a forward slash \(/\) or a backslash \(\\\).
-   Performance optimization

    If you upload a large number of objects by using sequential prefixes such as timestamps and letters in the object names, multiple object indexes may be stored in a single partition. If too many requests are sent to query these objects, the responsiveness may be slow. We recommend that you do not upload a large number of objects by using sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Prevent objects that have same names from being overwritten

    By default, when you upload an object to OSS, the existing object that has the same object name is overwritten. You can use the following methods to prevent your objects from being unexpectedly overwritten:

    -   Enable versioning

        If versioning is enabled, overwritten objects are saved as previous versions. You can recover the previous versions at any time. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

    -   Include a parameter in the upload request header

        Include the x-oss-forbid-overwrite parameter in the upload request header, and set its value to true. This way, when you upload an object whose object name is the same as that of an existing object, the object cannot be uploaded. OSS returns the `FileAlreadyExists` error. If this parameter is excluded from the request header or the value of this parameter is false, the object with the same object name is overwritten. For more information, see [PutObject](/intl.en-US/API Reference/Object operations/Basic operations/PutObject.mdtable_im4_mkw_bz).


## Configure object metadata

When you use simple upload, you can configure object metadata to describe an object. For example, you can set standard HTTP headers such as Content-Type. You can also set user-defined metadata. For more information, see [Manage object metadata](/intl.en-US/Developer Guide/Objects/Manage files/Manage object metadata.md).

## Security and authorization

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Overview](/intl.en-US/Developer Guide/Data security/Access and control/Overview.md).

To authorize third-party users to upload objects, OSS also provides account-based authorization. For more information, see [Authorized third-party upload](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).

## What to do next

-   After objects are uploaded to OSS, you can use upload callback to submit a callback request to the specified application server and perform subsequent operations. For more information, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md).
-   Images can be processed after they are uploaded. For more information, see [IMG user guide](/intl.en-US/Developer Guide/Data Processing/Image Processing/IMG implementation modes.md).

