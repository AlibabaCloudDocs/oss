# sync

The sync command is used to synchronize files from the source to the destination.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Usage notes

The sync and cp commands are used in a similar way but have the following differences:

-   The sync command supports the --delete option. When you add this option to the command, if the destination is an Object Storage Service \(OSS\) bucket, ossutil deletes all objects that do not exist in the source from the destination bucket and retains only the synchronized objects. If the destination is a local disk, you must add the --backup-dir option in the command to migrate files that do not exist in the source but do exist in the destination local disk to the specified folder.

    **Warning:** To add the --delete option to the command, we recommend that you enable [versioning](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md) for the destination bucket to prevent data from being accidentally deleted.

-   The sync command synchronizes files in the specified folder by recursively synchronizing all files and subfolders in the folder.
-   If the source or destination of the synchronization is an OSS bucket, and the specified folder name does not end with a forward slash, ossutil automatically adds a forward slash \(/\) to the end of the folder name.
-   The sync command does not support the --version-id option and as a result cannot be used to synchronize previous versions in versioned buckets.

Except for the preceding differences, the sync command can be used in the same way as the cp command. For more information about how to use the cp command, see [cp](/intl.en-US/Tools/ossutil/Common commands/cp/Overview.md).

## Synchronize local files to OSS

-   Command syntax

    ```
    ./ossutil sync file_url cloud_url  [-f] [-u] [--delete] [--backup-dir <value>] [--enable-symlink-dir] [--disable-all-symlink] [--disable-ignore-error] [--only-current-dir] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--snapshot-path=sdir] [--payer requester]
    ```

    The preceding command contains the following parameters:

    -   file\_url: the local path in which the files to synchronize are stored. Examples: `/localfolder/` in Linux and `D:\localfolder\` in Windows.
    -   cloud\_url: the path of the OSS folder to which the local files are synchronized. The value of this parameter is in the following format: `oss://bucketname/path/`. Example: `oss://examplebucket/exampledir/`. If the value of `cloud_url` does not end with a forward slash \(/\), ossutil automatically adds a forward slash to the end of the value.
-   Examples

    An OSS bucket named examplebucket contains a folder named example, which contains two objects named a.txt and b.txt and a subfolder named C. In the root folder of the local disk, a folder named example contains a file named d.txt. The following figure shows the structures of the source bucket and the destination local disk.

    ```
    examplebucket           root folder of the local disk
    └── example/             └── example/
           ├── a.txt                └── d.txt
           ├── b.txt
           └── C/
    ```

    The following examples show how to use the sync command to synchronize local files to OSS in different scenarios:

    -   Synchronize the example folder in the local disk to the destination OSS bucket.

        ```
        ./ossutil sync example/  oss://examplebucket/example/
        ```

        After the command is run, the d.txt file is added to the example folder of the examplebucket bucket as an object named d.txt.

        ```
        examplebucket           root folder of the local disk
        └── example/             └── example/
               ├── a.txt                └── d.txt
               ├── b.txt
               ├── d.txt
               └── C/
        ```

    -   Synchronize the example folder in the local disk to the destination OSS bucket without confirmation.

        When you synchronize a local folder to an OSS bucket, if the OSS bucket contains objects that have the same name as files in the local folder, OSS prompts you to confirm whether you want to overwrite the existing objects. If you confirm that the objects can be overwritten, you can add the -f, --force option in the command to force the overwrite operation. You can run the following command to synchronize the example folder in the local disk to the destination OSS bucket without confirmation:

        ```
        ./ossutil sync localfolder/ oss://examplebucket/destfolder/ -f
        ```

        After the command is run, the d.txt file is added to the example folder of the examplebucket bucket as an object named d.txt.

        ```
        examplebucket           root folder of the local disk
        └── example/             └── example/
               ├── a.txt                └── d.txt
               ├── b.txt
               ├── d.txt
               └── C/
        ```

    -   Synchronize the example folder in the local disk to the destination OSS bucket and delete all objects in that folder of the examplebucket bucket that do not exist within the same folder in the local disk.

        If the destination of the synchronization is an OSS bucket, you can add the --delete option in the command to delete all objects that do not exist in the source from the destination bucket and retains only the synchronized objects.

        ```
        ./ossutil sync example/  oss://examplebucket/example/ --delete
        ```

        After the command is run, the example folder in the local disk is synchronized to the examplebucket bucket. The a.txt and b.txt objects and the subfolder C within the example folder of the examplebucket bucket are deleted. Only the synchronized d.txt file is retained in the example folder of the examplebucket bucket.

        ```
        examplebucket           root folder of the local disk
        └── example/             └── example/
               └── d.txt                └── d.txt
        ```


## Synchronize OSS objects to local disks

-   Command syntax

    ```
    ./ossutil sync cloud_url file_url  [-f] [-u] [--delete] [--backup-dir] [--only-current-dir] [--disable-ignore-error] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--range=x-y] [--payer requester]
    ```

    You can run the preceding sync command to synchronize objects stored in a bucket to local disks. The command contains the following parameters:

    -   file\_url: The local path to which OSS objects are synchronized. Examples: `/localfolder/` in Linux and `D:\localfolder\` in Windows.
    -   cloud\_url: The path of the OSS folder in which the objects to synchronize are stored. The value of this parameter is in the following format: `oss://bucketname/path/`. Example: `oss://examplebucket/exampledir/`.
-   Examples

    The source OSS bucket named examplebucket contains a folder named example, which contains two objects named a.txt and b.txt and a subfolder named C. In the root folder of the destination local disk, a folder named example contains a file named d.txt. The following figure shows the structures of the source bucket and the destination local disk.

    ```
    examplebucket           root folder of the local disk
    └── example/             └── example/
           ├── a.txt                └── d.txt
           ├── b.txt
           └── C/
    ```

    The following examples show how to use the sync command to synchronize OSS objects to local disks in different scenarios:

    -   Synchronize the example folder of the examplebucket bucket to the local folder.

        ```
        ./ossutil sync  oss://examplebucket/example/  example/ 
        ```

        After the command is run, the a.txt and b.txt files and the subfolder C are added to the example folder in the local disk.

        ```
        examplebucket           root folder of the local disk
        └── example/             └── example/
               ├── a.txt                ├── a.txt 
               ├── b.txt                ├── b.txt
               └── C/                   ├── d.txt
                                           └── C/ 
        ```

    -   Synchronize the example folder of the examplebucket bucket to the local folder, and migrate files that do not exist in example folder of the examplebucket bucket but do exist in the destination local folder to the backup folder of the examplebucket bucket.

        ```
        ./ossutil sync oss://examplebucket/example/  example/  --delete  --backup-dir backup/
        ```

        After the command is run, the example folder of the examplebucket bucket is synchronized to the local disk, and the files in the example folder in the local disk are migrated to the backup folder of the examplebucket bucket. In this example, the a.txt and b.txt files and the subfolder C synchronized from OSS are retained in the example folder in the local disk. The d.txt file is added to the backup folder of the examplebucket bucket.

        ```
        examplebucket           root folder of the local disk
        └── example/             ├── example/
               ├── a.txt         │     ├── a.txt 
               ├── b.txt         │     ├── b.txt
               └── C/            │     └── C/                             
                                    └── backup/
                                           └──d.txt
        ```


## Synchronize objects between OSS buckets

-   Command syntax

    ```
    ./ossutil sync cloud_url cloud_url [-f] [-u] [--delete] [--backup-dir] [--only-current-dir] [--disable-ignore-error] [--output-dir=odir] [--bigfile-threshold=size] [--checkpoint-dir=cdir] [--payer requester]
    ```

    You can use the preceding sync command to synchronize data between OSS buckets. The source bucket and the destination bucket must be in the same region. The cloud\_url parameter in the command indicates the paths of the source bucket and destination bucket. The value of this parameter is in the following format: `oss://bucketname/path/`. Example: `oss://examplebucket/exampledir/`.

-   Examples

    Your Alibaba Cloud account owns two buckets named examplebucket1 and examplebucket2. The examplebucket1 bucket contains the example and test folders. The examplebucket2 bucket contains the example folder. The following figure shows the structures of examplebucket1 and examplebucket2.

    ```
    examplebucket1          examplebucket2
    ├── example/            └── example/
    │      ├── a.txt               ├── a.txt
    │      └── b.txt               └── c.txt
    └── test/     
            └── d.txt       
    ```

    The following examples show how to use the sync command to synchronize data between two OSS buckets in different scenarios:

    -   Synchronize the example folder of the examplebucket1 bucket to the test folder of the same bucket.

        ```
        ./ossutil sync oss://examplebucket1/example/  oss://examplebucket1/test/
        ```

        After the command is run, all objects in the example folder are synchronized to the test folder.

        ```
        examplebucket1          examplebucket2
        ├── example/            └── example/
        │      ├── a.txt               ├── a.txt
        │      └── b.txt               └── c.txt
        └── test/        
                ├── a.txt
                ├── b.txt
                └── d.txt      
        ```

    -   Synchronize the example folder of the examplebucket1 bucket to the example folder of the examplebucket2 bucket.

        ```
        ./ossutil sync oss://examplebucket1/example/ oss://examplebucket2/example/
        ```

        After the command is run, all objects in the example folder of the exmplebucket1 bucket are synchronized to the example folder of the examplebucket2 bucket.

        ```
        examplebucket1          examplebucket2
        ├── example/            └── example/
        │      ├── a.txt               ├── a.txt  
        │      └── b.txt               ├── b.txt
        └── test/                       └── c.txt 
                └── d.txt
        ```

    -   Synchronize all objects in examplebucket1 to examplebucket2, and delete objects that do not exist in examplebucket1 from examplebucket2.

        ```
        ./ossutil sync oss://examplebucket1 oss://examplebucket2 --delete
        ```

        After the command is run, the example and test folders of the examplebucket1 bucket are synchronized to the root folder of the examplebucket2 bucket. Other objects in the examplebucket2 bucket are deleted.

        ```
        examplebucket1          examplebucket2
        ├── example/            ├── example/
        │      ├── a.txt       │      ├── a.txt  
        │      └── b.txt       │      └── b.txt
        └── test/               └── test/  
                └── d.txt               └── d.txt                  
        ```


## Common options

The following table describes the options that you can add in the sync command.

|Option|Description|
|------|-----------|
|-f, --force|Forces an operation without prompting the user for confirmation.|
|-u, --update|Specifies that ossutil synchronizes files from the source only when the files do not exist in the destination or when the last modified time of the files is later than that of the corresponding objects in the destination bucket.|
|--output-dir|Specifies the directory in which output files are stored. Output files include the report files generated due to errors that occur when you use the cp and sync commands to copy or synchronize multiple files.

Default value: The ossutil\_output folder in the current directory. |
|--bigfile-threshold|Specifies a threshold of object size. If the size of an object exceeds the threshold, the object is transmitted by using resumable upload or download. Unit: bytes. Default value: 104857600 \(100 MB\).

Valid values: 0 to 9223372036854775807. |
|--part-size|Specifies the part size. Unit: bytes. By default, ossutil determines an appropriate part size based on the object size. If you have special requirements or want to optimize the performance, you can set the value of this option.

Valid values: 1 to 9223372036854775807. |
|--checkpoint-dir|Specifies the checkpoint folder. Default value: `.ossutil_checkpoint`. If a resumable data transfer task fails, ossutil automatically creates this folder and records the checkpoint information in the folder. If a resumable data transfer task succeeds, ossutil deletes the folder. If this option is specified, make sure that you have the permissions to delete the specified folder.|
|--range|Downloads a specific range of the object as a new object. The minimum start value of the range is 0, which indicates the byte 0 of the object. -   Specify a range

For example, a value of `3-9` indicates a range from byte 3 to byte 9, which includes byte 3 and byte 9.

-   Specify the start value

For example, a value of `3-` indicates a range from byte 3 to the end of the object, which includes byte 3.

-   Specify the end value

For example, a value of `-9` indicates a range from byte 0 to byte 9, which includes byte 9. |
|--encoding-type|Specifies the encoding type of the names of output or input objects. Valid values: url. If this option is not specified in the command, the object name is not encoded.

**Note:** Bucket names cannot be URL-encoded. |
|--include|Specifies that objects whose names contain the specified value are matched. For example, if the value of this option is `*.jpg`, objects whose names end with `.jpg` are considered matched.|
|--exclude|Specifies that objects whose names do not contain the specified value are matched. For example, if the value of this option is `*.txt`, objects whose names do not end with `.txt` are considered as matched.|
|--meta|Specifies the metadata of an object in the `[header:value#header:value...]` format. Example: `Cache-Control:no-cache#Content-Encoding:gzip`. For more information, see [set-meta](/intl.en-US/Tools/ossutil/Common commands/set-meta.md).|
|--acl|Configures the access control list \(ACL\) of an object. Valid values: -   default: ACL inherited from the bucket
-   private
-   public-read
-   public-read-write |
|--snapshot-path|If you specify the --snapshot-path option when you synchronize files, ossutil takes a snapshot of the synchronization and stores the snapshot information in a specified folder. When you specify this option in the command to synchronize the files again, ossutil reads the snapshot information from the specified folder and performs incremental synchronization. For more information, see [Generate snapshots for an object when you upload the object](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).|
|--disable-crc64|Disables CRC-64 in data transmission. By default, ossutil performs CRC-64 in data transmission. |
|--maxupspeed|Specifies the maximum upload speed. Unit: KB/s. Default value: 0. The value of 0 indicates that the upload speed is not limited. |
|--payer|Specifies the payer of the request. If pay-by-requester is enabled, you can set this option to `requester`.|
|-j, --jobs|Specifies the number of operations to perform on multiple objects concurrently. Default value: 3.

Valid values: 1 to 10000. |
|--parallel|Specifies the number of operations to perform on a single object concurrently. By default, ossutil sets the value of this option based on the operation type and object size. Valid values: 1 to 10000. |
|--loglevel|Specifies the log level. This option is empty by default, which indicates that no log files are generated. Valid values: -   info: generates prompt logs.
-   debug: generates detailed logs that contain corresponding HTTP request and response information. |
|--retry-times|Specifies the number that ossutil retires an operation when an error occurs. Default value: 10.

Valid values: 1 to 500. |
|--proxy-host|Specifies the URL of the proxy server. HTTP, HTTPS, and SOCKS5 proxies are supported. Examples: `http://120.79.**.**:3128` and `socks5://120.79.**.**:1080`.|
|--proxy-user|Specifies the user name used to access the proxy server. By default, this parameter is empty.|
|--proxy-pwd|Specifies the password used to access the proxy server. This parameter is empty by default.|
|--local-host|Specifies the local IP address of ossutil. If your computer has multiple IP addresses, you can specify this option so that ossutil can access OSS by using the specified IP address.|
|--enable-symlink-dir|Specifies whether to synchronize the subfolders of the symbolic link. By default, the subfolders are not synchronized. The [probe](/intl.en-US/Tools/ossutil/Common commands/probe.md) command can be used to check whether an object or a folder to which the symbolic link points is also a symbolic link.|
|--only-current-dir|Specifies that only objects in the current folder are synchronized. The subfolders in the current folder are not synchronized.|
|--disable-dir-object|Specifies that no OSS objects are generated for the folder during synchronization.|
|--disable-all-symlink|Specifies that all objects in the subfolder and the subfolder to which the symbolic link points are ignored during object synchronization.|
|--tagging|Specifies tags for an object when you synchronize the object. The tags are in the `"abc=1&bcd=2&……"` format.|
|--disable-ignore-error|Specifies that errors are not ignored during batch operations.|
|--delete|When you add this option in the command, if the destination of the synchronization is an OSS bucket, ossutil deletes all files that do not exist in the source from the destination bucket and retains only the synchronized files. If the destination of the synchronization is a local disk, you must also add the --backup-dir option in the command to migrate files that do not exist in the source but do exist in the destination local disk to the specified folder.|
|--backup-dir|This option is specified with the --delete option to back up files that do not exist in the source but exist in the destination local disk to the specified folder when the destination of the synchronization is a local disk.|

**Note:** For more information about common options, see [View options](/intl.en-US/Tools/ossutil/View options.md).

