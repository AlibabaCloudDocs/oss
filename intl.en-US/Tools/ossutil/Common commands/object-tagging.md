# object-tagging

The object-tagging command is used to add, modify, query, or delete tagging configurations for objects.

**Note:**

-   The commands described in this topic apply to Linux. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.
-   For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Command syntax

-   Add or modify object tagging configurations

    ```
    ./ossutil object-tagging --method put oss://bucket[/prefix] key#value [--encoding-type url] [-r] [--payer requester] [--version-id versionId] [-c file]
    ```

    If the object has no tags, you can run this command to add a specified tag to the object. If the object has tags, you can run this command to overwrite existing tags.

    **Note:**

    -   A maximum of 10 tags can be set for each object. Each object tag must have a unique tag key.
    -   A tag key can be a maximum of 128 characters in length. A tag value can be a maximum of 256 characters in length.
    -   Keys and values are case-sensitive.
    -   The key and value of the tag can contain letters, digits, spaces, and special characters such as

        + = . \_ : /

    -   Only the bucket owner and authorized users have the read and write permissions on object tags. These permissions are independent of object ACLs.
    -   During cross-region replication, object tags are also replicated to the destination bucket.
-   Query object tagging configurations

    ```
    ./ossutil object-tagging --method get oss://bucket[/prefix] [--encoding-type url] [-r]  [--payer requester] [--version-id versionId] [-c file]
    ```

-   Delete object tagging configurations

    ```
    ./ossuitl object-tagging --method delete oss://bucket[/prefix] [--encoding-type url] [-r] [--payer requester] [--version-id versionId] [-c file]
    ```


## Examples

-   Add object tagging configurations

    ```
    ./ossutil object-tagging --method put oss://bucket1/test.jpg a#1 b#2 c#3
    0.168034(s) elapsed
    ```

-   Query object tagging configurations

    ```
    ./ossutil object-tagging --method get oss://bucket1/test.jpg
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "a"       "1"     123/test.jpg
    1              1              "b"       "2"     123/test.jpg
    1              2              "c"       "3"     123/test.jpg
    
    0.228023(s) elapsed
    ```

-   Delete object tagging configurations

    ```
    ./ossutil object-tagging --method delete oss://bucket1/test.jpg
    0.200020(s) elapsed
    ```

-   Add tags to a specified version of an object in a versioning-enabled bucket

    ```
    ./ossutil object-tagging --method put oss://bucket1/test.png A#B --version-id CAEQCBiBgMCwrPXx8xYiIDA1NDVkZDI3ZDI5NzQwNzg5MWE0NjNiNzg0OWE5ZGNh
    ```

    To use the --version-id option, you must run the [ls --all-versions](/intl.en-US/Tools/ossutil/Common commands/ls.md) command to obtain all available versions of the object.

    **Note:** The --version-id option can only be used for objects in buckets that have versioning enabled. For more information about the command used to enable versioning on a bucket, see [bucket-versioning](/intl.en-US/Tools/ossutil/Common commands/bucket-versioning.md).


## Common options

The following table describes the options you can add to the object-tagging command.

|Option|Description&nbsp;|
|------|-----------------|
|--method|Specifies the HTTP request method. Valid values: -   put: adds or modifies object tagging configurations.
-   get: obtains object tagging configurations.
-   delete: deletes object tagging configurations. |
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. An example of the URL is http://120.79. \*\*.\*\*:3128 or socks5://120.79. \*\*. \*\*:1080.|
|--proxy-user|Specifies the username for the proxy server. The default value is null.|
|--proxy-pwd|Specifies the password for the proxy server. The default value is null.|
|--payer|Specifies the payer of the request. To enable pay-by-requester, set this option to requester.|
|-j,--jobs|Specifies the number of concurrent tasks when multiple objects are operated. Valid values: 1 to 10000. Default value: 3.|
|-r,--recursive|Recursively performs operations on objects in a bucket. If this option is specified, commands that support this option will perform operations on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will only perform operations on a single specified object.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded.|
|--version-id|Specifies the version ID of an object to be downloaded or copied. The bucket in which the object is located must have versioning enabled.|

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

