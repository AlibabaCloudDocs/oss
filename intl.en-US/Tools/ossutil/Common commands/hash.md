# hash

The hash command is used to calculate the CRC64 or MD5 value of a local file.

**Note:** The commands described in this topic apply to Linux. To use the commands in other systems, replace ./ossutil in the command with the actual executable program name. For example, you can use the help command in 32-bit Windows systems by running ossutil32.exe help.

## Command syntax

```
./ossutil hash file_url [--type=hashtype]
```

--type specifies the algorithm that is used for calculation. Valid values: crc64 and md5. Default value: crc64.

**Note:**

-   For more information about CRC64 and Content-MD5 values of OSS objects, [stat](/intl.en-US/Tools/ossutil/Common commands/stat.md) command, see `X-Oss-Hash-Crc64ecma` field and `Content-Md5` field.
-   If the object was uploaded before OSS supports CRC64, the stat command may fail to obtain the CRC64 value.
-   The stat command may fail to obtain the Content-MD5 value of objects that are uploaded through append upload or multipart upload.
-   If --type is set to md5, the MD5 and Content-MD5 values are displayed at the same time. The Content-MD5 value is obtained by calculating the MD5 hash to obtain a 128-bit number and then encoding the number in Base64.
-   The CRC64 value is calculated based on [Standard ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm).
-   For more information about Content-MD5, see [RFC 1864](https://tools.ietf.org/html/rfc1864).

## Examples

-   Calculate the CRC64 value of a local file

    ```
    ./ossutil hash test.txt --type=crc64
    CRC64-ECMA                  : 295992936743767023
    ```

-   Calculate the MD5 hash of a local file

    ```
    ./ossutil hash test.txt --type=md5
     MD5                         : 01C3C45C03B2AF225EFAD9F911A33D73
     Content-MD5                 : AcPEXAOyryJe+tn5EaM9cw==
    ```


## Common options

The following table describes the options you can add to the hash command.

|Option|Description|
|------|-----------|
|--type|Specifies the algorithm that is used for calculation. Valid values: crc64 and md5. Default value: crc64.|
|--loglevel|Specifies the log level. The default value is null, indicating that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

