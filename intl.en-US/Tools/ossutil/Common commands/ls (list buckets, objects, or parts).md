# ls \(list buckets, objects, or parts\)

You can run the ls command to list buckets, objects, or parts.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## List buckets

-   Command syntax

    ```
    ./ossutil64 ls [-s] [--limited-num] [--marker] 
    ```

    The following table describes the options you can configure in the command to list buckets.

    |Parameter|Description|
    |---------|-----------|
    |-s|Specifies that only bucket names are listed.|
    |--limited-num|The maximum number of results to return. You can specify this parameter together with the marker parameter to display returned results by page.|
    |--marker|The position from which the list operation starts. Buckets whose names are alphabetically after the value of marker are listed.|

-   Examples
    -   List all buckets that belong to the current Alibaba Cloud account.

        ```
        ./ossutil64 ls
        ```

        You can also run the following command to list all buckets:

        ```
        ./ossutil64 ls oss://
        ```

        The following output result indicates that all buckets that belong to the current Alibaba Cloud account are listed. Listed information includes the names, creation time, regions, storage classes, and number of the buckets.

        ```
        2016-10-21 16:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://examplebucketA
        2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucketB
        2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://examplebucketC
        2016-10-21 17:31:27 +0800 CST       oss-cn-hangzhou         Archive    oss://examplebucketD
        Bucket Number is:4
        0.252174(s) elapsed  
        ```

    -   List all buckets in simple mode

        ```
        ./ossutil64 ls -s
        ```

        The following output result indicates that all buckets that belong to the current Alibaba Cloud account are listed. Listed information includes only the names and number of the buckets.

        ```
        oss://examplebucketA
        oss://examplebucketB
        oss://examplebucketC
        oss://examplebucketD
        Bucket Number is:4
        0.235104(s) elapsed  
        ```

    -   List buckets whose names are alphabetically after examplebucketA, which is specified by the marker parameter.

        ```
        ./ossutil64 ls oss:// --limited-num=2 -s --marker examplebucketA
        ```

        The following output result indicates that two buckets whose names are alphabetically after examplebucketA are listed.

        ```
        2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucketB
        2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://examplebucketC
        Bucket Number is:2
        0.132174(s) elapsed                        
        ```


## List objects

-   Command syntax

    ```
    ./ossutil64 ls oss://bucketname[/prefix] [-s] [-d] [--limited-num] [--marker] [--include] [--exclude]  [--version-id-marker] [--all-versions]
    ```

    The following table describes the options you can configure in the command to list objects.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket in which objects you want to list are stored.|
    |prefix|The prefix of objects that you want to list. Specify this parameter when you want to list objects whose names contain the specified prefix.|
    |-s|Specifies that only object names are listed.|
    |-d|Specifies that only objects and subdirectories in the root directory of the bucket are listed. Objects in subdirectories are not listed.|
    |--limited-num|The maximum number of results to return. You can specify this parameter together with the marker parameter to display returned results by page.|
    |--marker|The position from which the list operation starts. Objects whose names are alphabetically after the value of marker are listed.|
    |--include|Specifies that objects that meet the specified conditions are listed. For example, a value of `*.jpg` indicates that all objects in the JPG format are listed. For more information, see [Upload multiple objects that meet specified conditions](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).|
    |--exclude|Specifies that objects that do not meet the specified conditions are listed. For example, a value of `*.txt` indicates that all objects that are not in the TXT format are listed.|
    |--version-id-marker|The position from which the list operation starts. Object versions whose IDs are alphabetically after the value of marker are listed. You can specify this parameter only when versioning is enabled for the bucket.|
    |--all-versions|Specifies that all versions of objects in the bucket are listed. You can specify this parameter only when versioning is enabled for the bucket.|

-   Examples
    -   List all objects in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket
        ```

        The following output indicates that all objects in examplebucket are listed. Listed information includes the last modified times, sizes, ETag values, and names of the objects.

        The ETag value of an object is used to identify the content of the object. If an object is created by using a PutObject request, the ETag of the object is the MD5 hash of the object content. If an object is created by using another method, the ETag value is the universally unique identifier \(UUID\) of the object content.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                    ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:3
        0.007379(s) elapsed                 
        ```

    -   List objects whose names contain the "example" prefix in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket/example
        ```

        The following output result indicates that all objects whose names contain the "example" prefix in examplebucket are listed:

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                     ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        Object Number is:2
        0.007379(s) elapsed                 
        ```

    -   List objects whose names contain the ".mp4" suffix in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket --include *.mp4
        ```

        The following output result indicates that objects whose names contain the ".mp4" suffix in examplebucket are listed.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                     ObjectName
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:1
        0.007379(s) elapsed                 
        ```

    -   List only objects and subdirectories in the root directory of a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket -d
        ```

        The following output result indicates that objects and subdirectories in the root directory of examplebucket are listed:

        ```
        oss://examplebucket/example.txt
        oss://examplebucket/examplefolder/
        oss://examplebucket/video.mp4
        Object and Directory Number is: 3
        
        0.278489(s) elapsed
        ```

    -   List all versions of all objects in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket --all-versions
        ```

        The following output result indicates that all versions of all objects in examplebucket are listed:

        ```
        LastModifiedTime                   Size(B)  StorageClass   ETag                                   VERSIONID                                                           IS-LATEST   DELETE-MARKER   ObjectName
        2020-12-01 15:06:37 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgICUsOuR2hYiIDI3NWVjNmEyYmM0NTRkZWNiMTkxY2VjMDMwZjFlMDA3    true        false           oss://examplebucket/example.txt
        2020-06-11 11:03:37 +0800 CST      363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545      CAEQARiBgIDZtvuR2hYiIDNhYjRkN2M5NTA5OTRlN2Q4YTYzODQwMzQ4NDYwZDdm    true        false           oss://examplebucket/examplefolder/photo.jpg
        2021-01-26 13:27:08 +0800 CST           0                                                       CAEQLxiBgIDd7NH0uRciIDA3Yzg0MTZjOWNlYzQ4ODZhMzVkZWE0MmE2NzBlYTYx    true        true            oss://examplebucket/image.png
        2020-12-01 15:06:45 +0800 CST    57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544      CAEQLBiBgMDZiprwthciIDY2NGM0NTNmZDE3ODRmZmVhZGM4YTUwZGQyNGU3ZjQ3    true        false           oss://examplebucket/video.mp4
        2016-06-11 10:53:46 +0800 CST      118076      Standard   FFDB300F053AAF06F4C4C58A4869C427      CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk    false       false           oss://examplebucket/example.txt
        2016-06-11 11:02:05 +0800 CST      345374      Standard   078A9852BCF81DC4811E6EDCBFD121BE      CAEQARiBgICNz_iR2hYiIGJjZTBjNDQxYWRhNTQ2ZTNiNmMzYzQ1YzMzMDA5ZjUw    false       false           oss://examplebucket/examplefolder/photo.jpg
        Object Number is: 6
        
        0.692000(s) elapsed
        ```

    -   List all versions of an object named example.txt in the root directory of a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket/example.txt --all-versions
        ```

        The following output indicates that all versions of example.txt are listed:

        ```
        LastModifiedTime                   Size(B)  StorageClass  ETag                                   VERSIONID                                                           IS-LATEST   DELETE-MARKER  ObjectName
        2020-12-01 15:06:37 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgICUsOuR2hYiIDI3NWVjNmEyYmM0NTRkZWNiMTkxY2VjMDMwZjFlMDA3    true        false           oss://examplebucket/example.txt
        2016-06-11 10:53:46 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk    false       false           oss://examplebucket/example.txt
        Object Number is: 2
        
        0.361000(s) elapsed
        ```


## List parts

-   Command syntax

    ```
    ./ossutil64 ls oss://bucketname[/prefix] [-s] [-d] [-m] [-a] [--limited-num] [--upload-id-marker] 
    ```

    The following table describes the options you can configure in the command to list parts.

    |Parameter|Description|
    |---------|-----------|
    |bucketname|The name of the bucket in which parts you want to list are stored. Specify this parameter when you want to list parts in the specified bucket. |
    |prefix|The prefix that is contained in the names of parts that you want to list.|
    |-s|Specifies that only upload IDs and objects are listed.|
    |-d|Specifies that only objects and subdirectories in the root directory of the bucket are listed. Objects in subdirectories are not listed.|
    |-m|Specifies that the command is run to list parts.|
    |-a|Specifies that the command is run to list objects and parts.|
    |--limited-num|The maximum number of results to return. You can specify this parameter together with the marker parameter to display returned results by page.|
    |--upload-id-marker|The position from which the list operation starts. Parts whose upload IDs are alphabetically after the value of marker are listed.|

-   Examples
    -   List all parts in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket -m
        ```

        The following output result indicates that all parts in examplebucket are listed:

        ```
        InitiatedTime                     UploadID                           ObjectName
        2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://examplebucket/test.mp4
        2017-01-13 03:45:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://examplebucket/test.mp4
        2017-01-13 03:45:01 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://examplebucket/test.mp4
        2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://examplebucket/object.exe
        UploadId Number is:4
        0.191289(s) elapsed  
        ```

    -   List all objects and parts in a bucket named examplebucket.

        ```
        ./ossutil64 ls oss://examplebucket -a
        ```

        The following output result indicates that all objects and parts in examplebucket are listed:

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                     ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:3
        InitiatedTime                     UploadID                           ObjectName
        2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://examplebucket/test.mp4
        2017-01-13 03:45:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://examplebucket/test.mp4
        2017-01-13 03:45:01 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://examplebucket/test.mp4
        2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://examplebucket/object.exe
        UploadId Number is:4
        0.791289(s) elapsed  
        ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by multiple Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to list all objects in a bucket named test, which is located in the China \(Hangzhou\) region and owned by another Alibaba Cloud account.

```
./ossutil64 ls oss://test -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options of this command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

