# Common options

You can use the -h option to view the common options supported by ossfs.

## Command syntax

```
./ossfs -h
```

You must use this command in the directory where ossfs is located. The directory is /usr/local/bin/ by default and is subject to the actual installation environment.

## Common options

ossfs is implemented based on Filesystem in Userspace \(FUSE\) and supports the fuse options in addition to the ossfs options. When you attach a bucket, you can set different startup options. The options can be in one of the following formats:

```
-o option_name[=option_value]  or  -ooption_name[=option_value]
```

For example, you can use the following option to specify the uid and gid parameters when you attach a bucket:

```
ossfs bucket_name mount_point -ourl=endpoint -ouid=uid -ogid=gid
```

The following section describes common ossfs options.

-   url: specifies the endpoint used to access a bucket. Format: `url=endpoint`. The default request protocol is HTTP.

    Examples:

    ```
    -ourl=oss-cn-hangzhou.aliyuncs.com
    -ourl=http://oss-cn-hangzhou.aliyuncs.com
    -ourl=https://oss-cn-hangzhou.aliyuncs.com
    ```

-   passwd\_file: specifies the object that stores the AccessKey pair used to access a bucket. Default value: /etc/passwd-ossfs. Ensure that the permission on this object is configured correctly. If the object is /etc/passwd-ossfs, you can set the permission to 640. If the object is not /etc/passwd-ossfs, you must set the permission to 600. The content of the object is in the following format: `${bucket}:${access-key-id}:{access-key-secret}`.

    Examples:

    ```
    echo bucket-test:LTAIbZcdVCmQ****:MOk8x0y9hxQ31coh7A5e2MZEUz**** > /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    
    echo bucket-test:LTAIbZcdVCmQ****:MOk8x0y9hxQ31coh7A5e2MZEUz**** > /passwd-path/passwd-ossfs
    chmod 600 /passwd-path/passwd-ossfs
    
    -opasswd_file=/passwd-path/passwd-ossfs
    ```

-   max\_stat\_cache\_size: specifies the maximum number of objects whose metadata can be cached. Default value: 1000. When a directory contains a large number of objects, you can set this option to accelerate object listing when you use the ls command to list objects. You can also set the value of this option to 0 to disable metadata caching.
-   allow\_other: authorizes other users to access the directory to which the bucket is attached, but not objects in the folder. To modify the access permission on the objects in the directory, you must use the chmod command. No value is available for this option. To grant permissions to other users, use the -oallow\_other option.
-   dbglevel: specifies the log level. Valid values: critical, error, warn, info, and debug. Default value: critical. For example, to enable info log collection, use the -odbglevel=info option. Logs are written to system logs. For example, in CentOS, logs are written to /var/log/messages.
-   -f: runs ossfs as a foreground program instead of a daemon. In this case, logs are displayed on the screen of a terminal. This option is used in debugging.
-   -d: enables logging. This option is also used in fuse. In ossfs, this option is equivalent to the -odbglevel=info setting.

## Options

Unless otherwise specified, options are in one of the following formats: `-ooption_name=option_value` or `-o option_name=option_value`.

-   ossfs options

    |Option|Description|
    |------|-----------|
    |default\_acl|Specifies the ACL of an object when the object is uploaded to OSS. Default value: private. Valid values:     -   private
    -   public-read
    -   public-read-write
 For more information about ACLs, see [ACL](/intl.en-US/Developer Guide/Data security/Access and control/ACL.md).|
    |retries|Specifies the number of retry attempts when a request fails. Default value: 2.|
    |storage\_class|Specifies the storage class of an object when the object is uploaded to OSS. Default value: Standard. Valid values:     -   Standard
    -   IA
    -   Archive
 For more information about storage classes, see [Overview](/intl.en-US/Developer Guide/Storage classes/Overview.md).|
    |public\_bucket|Allows users to access buckets as anonymous users. This option is applicable only to buckets whose ACLs are public read/write. Valid values:     -   0: disallows users to access buckets as anonymous users. This is the default value.
    -   1: allows users to access buckets as anonymous users. |
    |passwd\_file|Specifies the object that stores the AccessKey pair used to access a bucket. Default value: /etc/passwd-ossfs.|
    |connect\_timeout|Specifies the timeout period in seconds for connections. Default value: 300.|
    |readwrite\_timeout|Specifies the timeout period in seconds for read or write requests. Default value: 60.|
    |max\_stat\_cache\_size|Specifies the maximum number of objects whose metadata can be cached. Default value: 1000. The metadata of 1,000 objects consumes about 4 MB of cache space.|
    |stat\_cache\_expire|Specifies the expiration time for the object metadata cache. By default, the metadata cache does not expire.|
    |no\_check\_certificate|Specifies that the server certificates are not validated. This option is valid only when the request protocol is HTTPS. By default, certificate validation is enabled. No values are available for this option. To disable certificate validation, use the -ono\_check\_certificate option.|
    |multireq\_max|Specifies the maximum number of concurrent requests to access object metadata during object listing. Default value: 20.|
    |parallel\_count|Specifies the number of parts that can be uploaded concurrently when multipart upload is used to upload large objects. Default value: 5.|
    |multipart\_size|Specifies the size of each part in MB when multipart upload is used to upload data. Default value: 10. This option limits the maximum size of the object you want to upload. When multipart upload is used, the maximum number of parts that can be uploaded is 10,000. By default, the maximum size of the object to be uploaded is 100 GB. You can adjust the value of this option to upload larger objects.|
    |url|Specifies the endpoint used to access a bucket.|
    |mp\_umask|Specifies the permission mask for mount points. This option takes effect only when the allow\_other option is set. Default value: 000. This option is used in the same way as the umask command. For example, you can set -oallow\_other -omp\_umask=007 to set the permission of the mount point to 770, and set -oallow\_other -omp\_umask=077 to set the permission of the mount point to 700.|
    |enable\_content\_md5|Specifies whether to set the CONTENT\_MD5 header for uploads. By default, this header is not set. No value is available for this option. To specify CONTENT\_MD5, use the -oenable\_content\_md5 option.|
    |ram\_role|Specifies access to OSS by using instance RAM roles. When you access OSS by using instance RAM roles, the AccessKey ID and AccessKey secret of the key object are ignored.|
    |dbglevel|Specifies the log level. Valid values: critical, error, warn, info, and debug. Default value: critical.|
    |curldbg|Specifies whether to enable libcurl logging. By default, libcurl logging is disabled. No value is available for this option. To obtain libcurl logs, use the -ocurldbg option.|

-   fuse options

    |Option|Description|
    |------|-----------|
    |allow\_other|Modifies the permission of the mount point to allow access from all users. By default, only the root user can set this option. No value is available for this option. To allow access from all users, use the -oallow\_other option.|
    |uid|Specifies the user ID \(UID\) of the owner of a folder.|
    |gid|Specifies the group ID \(GID\) of the owner of a folder.|


