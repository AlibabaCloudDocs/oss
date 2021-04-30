# View options

You can use the -h option to view all options supported by ossutil.

## Command syntax

```
./ossutil -h
```

To view the options supported by a command, run the ossutil help \[command\] command, such as ossutil help cp.

## Common options

The following section describes some common options that can be added to most commands supported by ossutil:

-   -c, --config-file

    Specifies the configuration file path of ossutil. ossutil reads the configuration file during startup, and writes configurations to the file by using the config command. When you manage buckets that belong to different Alibaba Cloud accounts, you can generate multiple configuration files, and specify one of these configuration files as the default configuration file. When you manage a bucket that belongs to other Alibaba Cloud accounts, you can use the -c option to specify the corresponding configuration file.

-   -e, --endpoint

    Specifies the endpoint of a bucket. When you manage buckets across regions, you can use the -e option to specify the corresponding endpoint of a bucket in a region.

-   -i, --access-key-id

    Specifies the AccessKey ID used to access Object Storage Service \(OSS\). When you manage buckets that belong to different Alibaba Cloud accounts, you can use the -i option to specify the corresponding AccessKey IDs.

-   -k, --access-key-secret

    Specifies the AccessKey secret used to access OSS. When you manage buckets that belong to different accounts, you can use the -k option to specify the corresponding AccessKey secrets.

-   -p, --password

    Specifies the AccessKey secret used to access OSS. When you use this option in a command, ossutil reads the AccessKey secret that is entered by using the keyboard and ignores the AccessKey secret configured by using other methods.

-   --loglevel

    Generates the ossutil.log file in the current working directory. The default value is null, which indicates that no log files are generated. Valid values: info and debug.

    -   Default: The value of --loglevel is empty, which indicates that no log files are generated.
    -   info: generates operation logs.

        ```
        ./ossutil [command] --loglevel=info
        ```

    -   debug: generates logs that contain HTTP requests and responses and original signature strings to locate problems.

        ```
        ./ossutil [command] --loglevel=debug
        ```

-   --proxy-host, --proxy-user, and --proxy-pwd

    If your environment requires a proxy server to access websites, you must use these three options to specify the information of the proxy server. ossutil uses the specified information to access OSS through the corresponding proxy server.

    Example:

    ```
    ./ossutil ls oss://bucket1 --proxy-host http://47.88.**.**:3128 --proxy-user test --proxy-pwd test
    ```


## Options

The following table describes all options supported by ossutil.

|Option|Description|
|------|-----------|
|-s, --short-format|Lists items in the short format. The long format is used if this option is not specified.|
|--bigfile-threshold|Specifies the size threshold for which a large object starts resumable data transfer. Unit: bytes. Valid values: 0 to 9223372036854775807. Default value: 104857600 \(100 MB\).|
|--acl|Configures the access control list \(ACL\) for an object or a bucket.|
|--range|Specifies the byte range of the object to download. Bytes are numbered from 0. -   You can specify a range. For example, 3-9 indicates a range from byte 3 to byte 9, which includes byte 3 and byte 9.
-   You can specify the field from which the download starts. For example, 3- indicates a range from byte 3 to the end of the object, which includes byte 3.
-   You can specify the field with which the download ends. For example, -9 indicates a range from byte 0 to byte 9, which includes byte 9. |
|--all-versions|Specifies all versions of an object.|
|--type|Specifies the algorithm that is used for calculation. Valid values: crc64 and md5. Default value: crc64.|
|-v, --version|Displays the ossutil version and exits.|
|-u, --update|Specifies an update operation.|
|--origin|Specifies the value of the Origin field in an HTTP request header.|
|--upmode|Specifies the upload method. Valid values: normal, append, and multipart. Default value: normal. This option is used with the probe command.|
|--sse-algorithm|Specifies the server-side encryption algorithm. Valid values: KMS and AES-256.|
|--include|Includes objects whose names match a specific string such as `*.jpg`.|
|--exclude|Excludes objects whose names match a specific string such as `*.txt`.|
|-r, --recursive|Performs operations on all objects in a bucket. If you specify this option when you run commands that support this option, operations are performed on all objects in a bucket that meet the specified conditions. If this option is not specified, the commands will perform operations only on the specified object.|
|--addr|Specifies a network address, which is a domain name in most cases. This option is used with the probe command.|
|--kms-masterkey-id|Specifies the CMK ID used for encryption in Key Management Service \(KMS\).|
|--version-id|Specifies the version ID of an object.|
|--version-id-marker|Specifies the object version that you want to start listing from.|
|-m, --multipart|Specifies that operations are performed only on incomplete multipart upload tasks in a bucket instead of normal objects.|
|-d, --directory|Lists objects and subdirectories in the current directory, instead of recursively displaying all objects in all subdirectories.|
|--payer|Specifies the payer of the request. To enable pay-by-requester, set this option to requester.|
|--maxupspeed|Specifies the maximum upload speed. Unit: KB/s. Default value: 0 \(unlimited\).|
|--retry-times|Specifies the number of times an operation is retried if the operation fails. Valid values: 1 to 500. Default value: 10.|
|-c, --config-file|Specifies the configuration file path of ossutil. ossutil reads the configuration file during startup. You can use the config command to write configurations to the configuration file.|
|--download|Downloads an object from OSS. This option is used with the probe command.|
|-j, --jobs|Specifies the number of concurrent jobs performed across multiple objects. Valid values: 1 to 10000. Default value: 3.|
|-a, --all-type|Specifies that operations are to be performed on both the objects and incomplete multipart upload tasks in a bucket.|
|--disable-empty-referer|Specifies that the referer field cannot be empty. This option can be added to the referer command.|
|--method|Specifies the HTTP request method, which can be PUT, GET, or DELETE.|
|--output-dir|Specifies the directory in which output objects are located. Output objects include report objects generated due to errors that occur when you use the cp command to copy multiple objects. For more information about the report objects, see the help information of the cp command. The default value is the ossutil\_output directory in the current directory.|
|--meta|Specifies the metadata of an object in the \[header:value\#header:value...\] format. Example: `Cache-Control: no-cache#Content-Encoding: gzip`.|
|--object|Specifies the name of an object in OSS. This option is used with the probe command.|
|-e, --endpoint|Specifies the endpoint used by ossutil to access OSS. This option value overwrites the corresponding endpoint in the configuration file. For more information about endpoints of different regions, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|--end-time|Specifies the timestamp in Linux or UNIX. If you enter this option, objects whose last update time is later than the timestamp are ignored.|
|--limited-num|Specifies the maximum number of returned results.|
|-L, --language|Specifies the language ossutil uses. Valid values: CH and EN. Default value: CH. To set this option to CH, make sure that your system supports UTF-8 encoding.|
|--delete|Specifies a deletion operation.|
|-b, --bucket|Specifies the bucket on which the specified operation is performed.|
|--disable-crc64|Disables CRC-64. By default, ossutil enables CRC-64 during data transmission.|
|--upload|Uploads an object to OSS. This option is used with the probe command.|
|--part-size|Specifies the part size in bytes. By default, ossutil calculates the appropriate part size based on the object size. You can set this option to optimize performance or special requirements exist. Valid values: 1 to 9223372036854775807.|
|--timeout|Specifies the timeout period of a signed URL request. Unit: seconds. Valid values: 0 to 9223372036854775807. Default value: 60.|
|-k, --access-key-secret|Specifies the AccessKey secret used to access OSS. This option value will overwrite the corresponding configurations in the configuration file.|
|--checkpoint-dir|Specifies the directory in which the checkpoint file is stored. Default value: .ossutil\_checkpoint. When a resumable upload or download task fails, ossutil automatically creates this directory and records the checkpoint information in a checkpoint file in the directory. After the resumable upload or download task is complete, ossutil deletes this directory. If this option is specified, make sure that you have permissions to delete the specified directory.|
|--url|Specifies the URL of an object. This option is used with the probe command.|
|--marker|Specifies the bucket name, object name, or multipart upload task ID from which you want to start listing.|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|--snapshot-path|If you specify the --snapshot-path option when you upload or download multiple objects, ossutil takes a snapshot of the upload or download and stores the snapshot information in a specified directory. The next time the objects are uploaded or downloaded while this option is specified, ossutil reads the snapshot information from the specified directory and performs an incremental upload or download. **Note:**

-   The --snapshot-path option is used to accelerate the incremental upload or download of objects. This option cannot be used to copy objects. This option can be used when the number of objects is large and no other users modify the corresponding objects in OSS between the two uploads.
-   You can use the --snapshot-path option to record the lastModifiedTime of uploaded or downloaded objects on local clients. OSS compares the last modified time with that of objects currently pending upload or download to determine which objects can be skipped. When you use this option, make sure that the corresponding objects in OSS are not modified between the two uploads or downloads. In other scenarios where objects are updated in OSS between the two uploads or downloads, use the --update option to perform incremental upload or download on objects.
-   ossutil does not automatically delete snapshot information from the directory specified by snapshot-path. You can remove the directory if the snapshot information is no longer needed.
-   Additional system resources are required to read and write snapshot information. We recommend that you do not use this option in the following scenarios: The number of objects to upload or download is small. The network is properly connected. Other users need to perform operations on those objects. In this case, you can use the --update option to perform incremental upload or download.
-   The --update and --snapshot-path options can be used together. ossutil first uses the --snapshot-path option to determine whether to skip the upload or download of an object. If the upload or download is not skipped by --snapshot-path, ossutil then uses the --update option to determine whether to skip the upload or download of the object. |
|--start-time|Specifies the timestamp in Linux or UNIX. If you enter this option, objects whose last update time is earlier than the timestamp are ignored.|
|--loglevel|Specifies the log level. This option is empty by default, indicating that no log files are generated. Valid values: -   info: indicates that ossutil generates prompt logs.
-   debug: indicates that ossutil generates detailed logs that contain corresponding HTTP request and response information. |
|--storage-class|Specifies the storage class of an object. Valid values: Standard, IA, Archive, and ColdArchive. Default value: Standard.|
|-i, --access-key-id|Specifies the AccessKey ID used to access OSS. This option value overwrites the corresponding configurations in the configuration file.|
|-t, --sts-token|Optional. This option specifies the Security Token Service \(STS\) token used to access OSS. This option value overwrites the corresponding configurations in the configuration file. This option is required only when you use a temporary STS token to access OSS. Otherwise, you can leave this parameter unspecified. For more information about how to generate an STS token, see [Temporary access credential](/intl.en-US/Developer Guide/Objects/Upload files/Authorized third-party upload.md).|
|--parallel|Specifies the number of concurrent operations performed on a single object. Valid values: 1 to 10000. By default, ossutil automatically sets the value of this option based on the operation type and object size.|
|--partition-download|Specifies the partition to download. The value of this option is in the "partition number: the total number of partitions" format. A value of 1:5 indicates that ossutil downloads partition 1 out of the total five partitions. Partitions are numbered from 1. Partitioning rules for objects are determined by ossutil. This option splits an object to download into multiple partitions that can be downloaded by multiple ossutil commands. Each ossutil command downloads its own partition. You can run multiple ossutil commands on different machines at the same time.|
|--bucketname|Specifies the name of a bucket. This option is used with the probe command.|
|--encoding-type|Specifies the encoding type of the object name. If this option is specified, this value must be url. If this option is not specified, the object name is not encoded. Bucket names cannot be URL-encoded. **Note:** The value of this option must be in the following format: oss: //bucket/url\_encode \(object\). For example, the URL `oss: //bucket/object` must be entered as `oss: //bucket/url_encode (object)`. The `oss: //bucket/` string does not need to be encoded. |
|--origin|Specifies the value of the Origin header in an HTTP request. This option value indicates the source domain of a cross-domain request.|
|--acr-method|Specifies the value of the Access-Control-Request-Method request header. Valid values: GET, PUT, POST, DELETE, and HEAD.|
|--acr-headers|Specifies the value of the Access-Control-Request-Headers request header. The value of this parameter does not include common request headers. To specify multiple headers, separate different headers with commas \(,\) and enclose the headers with double quotation marks \("\). Example: `--acr-headers "header1,header2,header3"`.|
|--upload-id-marker|Specifies the multipart upload ID from which you want to start listing.|
|-h, --help|Displays help information for a specified command.|
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 are supported. Example: http://120.79.\*\*.\*\*:3128and\*\*.socks5://120.79.\*\*:1080.|
|--proxy-user|Specifies the username used to access the proxy server. This parameter is empty by default.|
|--proxy-pwd|Specifies the password used to access the proxy server. This parameter is empty by default.|
|--trafic-limit|Specifies the speed to access the object over HTTP. Unit: bit/s. Valid values: 819200 to 838860800 \(100 KB/s to 100 MB/s\). Default value: 0. A value of 0 indicates that the maximum access speed is unlimited. This option is used with the sign command.|
|--local-host|Enter the local IP address of ossutil. If your computer has multiple IP addresses, you can specify this option so that ossutil can access OSS through the specified IP address. This option is used with the cp command.|
|--enable-symlink-dir|Specifies that the subdirectory to which the symbolic link points is uploaded. By default, subdirectories are not uploaded. The [probe](/intl.en-US/Tools/ossutil/Common commands/probe.md) command can be used to check whether an object or a directory to which the symbolic link points is also a symbolic link.|
|--only-current-dir|Specifies that only objects in the current directory are uploaded, downloaded, or copied. Subdirectories in the current directory are ignored.|
|--disable-dir-object|Specifies that no OSS object is generated for the directory during object uploads.|
|--probe-item|Specifies the items to be checked by using the probe command. Valid values: -   upload-speed: checks the upload bandwidth.
-   download-speed: checks the download bandwidth.
-   cycle-symlink: checks whether a symbolic link in the local file directory points to itself. |
|--redundancy-type|Specifies the data redundancy type of a bucket. Valid values: LRS \(local redundant storage\) and ZRS \(zone-redundant storage\). Default value: LRS.|
|--disable-encode-slash|Specifies that forward slashes \(/\) in the URL path are not encoded.|
|--disable-all-symlink|All objects in the subdirectory to which the symbolic link points and the subdirectory to which the symbolic link points are ignored during object uploads.|
|--tagging|Specifies the object tag when you upload or copy an object in the `"abc=1&bcd=2&……"` format.|
|--disable-ignore-error|Specifies that errors are not ignored during batch operations.|

