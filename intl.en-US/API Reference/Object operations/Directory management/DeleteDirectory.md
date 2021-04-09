# DeleteDirectory

You can call this operation to delete a directory. This operation is applicable only to buckets that have the hierarchical namespace feature enabled.

## Usage notes

-   To delete a directory, use one of the following methods:
    -   Recursive delete: All objects and subdirectories are deleted from the directory.
    -   Non-recursive delete: The directory can be deleted if the directory is empty.
-   Different permissions are required when a different deletion method is used.
    -   When you use the recursive delete method to delete a directory, you must have the DeleteObject permission on the directory and all objects and subdirectories in this directory.

        For example, to recursively delete the oss directory from the desktop directory, you must have the DeleteObject permission on the desktop/oss directory and all objects and subdirectories in the desktop/oss directory.

    -   To use non-recursive delete to delete a directory, you must have the DeleteObject permission on the directory.

        To use non-recursive delete to delete the dir directory from the desktop directory, you must have the DeleteObject permission on the desktop/dir directory.

-   You cannot use the recursive delete method to delete the root directory of a bucket.
-   When you use the recursive delete method to delete a directory, the directory may fail to be deleted if concurrent requests are sent to write data to the directory at the same time.

## Request structure

```
POST /objectName?x-oss-delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Request headers

|Header|Type|Required|Description|
|------|----|--------|-----------|
|x-oss-delete-recursive|String|No|Specifies whether to recursively delete a directory. -   If you do not specify x-oss-delete-recursive or set x-oss-delete-recursive to false, the non-recursive delete method is used. The directory can be deleted only when the directory is empty.
-   If you set x-oss-delete-recursive to true, the recursive delete method is used. The directory as well as all objects and subdirectories in this directory is deleted.

Default value: false |
|x-oss-delete-token|String|No|The name of the object or directory after which the next delete operation begins. This option is valid only when x-oss-delete-recursive is set to true. This option is empty when you call the DeleteDirectory operation on the bucket for the first time. |

This API operation must also include common request headers such as Host and Date. For more information about common request headers, see [Common request headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response headers

The response headers involved in this API operation contain only common response headers. For more information about common response headers involved in this API operation, see [Common response headers](/intl.en-US/API Reference/Common HTTP headers.md).

## Response elements

|Element|Type|Description|
|-------|----|-----------|
|DeleteDirectoryResult|Container|The container that stores the deleted objects. Parent nodes: none |
|DirectoryName|String|The name of the deleted directory. Parent nodes: DeleteDirectoryResult |
|DeleteNumber|String|The number of deleted objects and directories. Parent nodes: DeleteDirectoryResult |
|NextDeleteToken|String|The name of the object or directory after which the delete operation begins. Parent nodes: DeleteDirectoryResult |

## Examples

-   Use non-recursive delete to delete a directory

    Sample requests

    ```
    POST /desktop/oss/a?x-oss-delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Connection: keep-alive
    Server: AliyunOSS
    <DeleteDirectoryResult>
        <DirectoryName>desktop/oss/a</DirectoryName>
        <DeleteNumber>1</DeleteNumber>
    </DeleteDirectoryResult>
    ```

-   Recursively delete a directory

    Sample requests

    ```
    POST /desktop/oss/a?x-oss-delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    x-oss-delete-recursive: true
    ```

    Sample success responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Connection: keep-alive
    Server: AliyunOSS
    <DeleteDirectoryResult>
        <DirectoryName>desktop/oss/a</DirectoryName>
        <DeleteNumber>100</DeleteNumber>
        <NextDeleteToken>CgJiYw--</NextDeleteToken>
    </DeleteDirectoryResult>
    ```


## SDK

OSS SDK for Java: [Delete directories]()

## Error codes

|Error code|HTTP status code|Description|
|----------|----------------|-----------|
|AccessDenied|403|Possible causes:-   When you delete the directory, you do not have the permissions to access the specified bucket.
-   When you delete the directory, you do not have the permissions to delete the directory. |
|NoSuchKey|404|When you delete the directory, the specified directory does not exist.|
|FileAlreadyExists|409|Possible causes:-   When you use the non-recursive delete method to delete the directory, the directory is not empty.
-   When you use the recursive delete method to delete the directory, concurrent requests are sent to write data to the directory at the same time. |
|InvalidArgument|400|When you use the recursive delete method to delete the directory, the format of the x-oss-delete-token value is invalid.|

