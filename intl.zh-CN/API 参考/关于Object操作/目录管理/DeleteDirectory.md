# DeleteDirectory

DeleteDirectory接口用于删除目录（Directory）。只有开启分层命名空间的Bucket支持调用此接口。

## 注意事项

-   目录支持如下删除方式，请根据实际选择。
    -   递归删除（Recursively Delete）：清空目录中的所有文件和目录。
    -   非递归删除：只能删除空目录。
-   使用的删除方式不同时需要的权限不同。
    -   使用递归删除方式删除目录时，您必须有目录以及该目录下所有文件和目录的DeleteObject权限。

        例如要递归删除desktop目录下oss目录，您必须有desktop/oss和desktop/oss/\*的DeleteObject权限。

    -   使用非递归删除方式删除目录时，您必须有目录的DeleteObject权限。

        例如要非递归删除desktop目录下的dir目录，您必须有desktop/dir的DeleteObject权限。

-   使用递归删除方式删除目录时，如果同时存在向目录的并发写请求，则可能导致目录删除失败。

## 请求语法

```
POST /objectName?x-oss-delete HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求头

|名称|类型|是否必选|描述|
|--|--|----|--|
|x-oss-delete-recursive|字符串|否|是否递归删除目录。-   不指定x-oss-delete-recursive或者指定x-oss-delete-recursive为false时，表示非递归删除，即只能删除空目录。
-   指定x-oss-delete-recursive为true时，表示递归删除，即清空目录中的所有文件和目录。

默认值：false |
|x-oss-delete-token|字符串|否|下一个删除目标的标记。此选项仅当x-oss-delete-recursive为true时有效。第一次调用DeleteDirectory接口时为空。 |

此接口还需要包含Host、Date等公共请求头。关于公共请求头的更多信息，请参见[公共请求头（Common Request Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应头

此接口只包含公共响应头。关于公共响应头的更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 响应元素

|名称|类型|描述|
|--|--|--|
|DeleteDirectoryResult|容器|保存被成功删除的Object的容器。父节点：None |
|DirectoryName|字符串|删除的目录名称。父节点：DeleteDirectoryResult |
|DeleteNumber|字符串|删除的文件和目录数量。父节点：DeleteDirectoryResult |
|NextDeleteToken|字符串|下一次删除起始位置的标记。父节点：DeleteDirectoryResult |

## 示例

-   非递归删除目录

    请求示例

    ```
    POST /desktop/oss/a?x-oss-delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    ```

    返回示例

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

-   递归删除目录

    请求示例

    ```
    POST /desktop/oss/a?x-oss-delete HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:CTkuxpLAi4XZ+WwIfNm0Fmgb****
    x-oss-delete-recursive: true
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 559CC9BDC755F95A64485981
    Date: Thu, 29 Apr 2021 05:21:12 GMT
    Connection: keep-alive
    Server: AliyunOSS
    <DeleteDirectoryResult>
        <DirectoryName>desktop/oss/a</DirectoryName>
        <DeleteNumber>100</DeleteNumber>
        <NextDeleteToken>Cg9kZXNrdG9wL29zcy9hLzk-</NextDeleteToken>
    </DeleteDirectoryResult>
    ```


## SDK

[Java SDK：删除目录]()

## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|AccessDenied|403|返回该错误的可能原因如下：-   删除目录时，用户对设置的Bucket没有访问权限。
-   删除目录时，用户对目录没有删除权限。 |
|NoSuchKey|404|删除目录时，设置的目录不存在。|
|FileAlreadyExists|409|返回该错误的可能原因如下：-   使用非递归删除方式删除目录时，目录不为空。
-   使用递归删除方式删除目录时，同时存在向目录的并发写请求。 |
|InvalidArgument|400|使用递归删除方式删除目录时，传入的x-oss-delete-token格式错误。|

