# ls \(list\)

The ls command is used to list buckets, objects, or parts.

**Note:** The commands involved in this topic are based on Linux 64-bit systems. If you use a Windows 64-bit system, replace ./ossutil64 in the following example with ossutil64.exe.

## List buckets

**Note:** You cannot list buckets when you use the accelerate endpoint. We recommend that you use the regular endpoint. For more information about transfer acceleration, see [Transfer acceleration](/intl.en-US/Developer Guide/Buckets/Transfer acceleration.md).

-   Command syntax

    ```
    ./ossutil64 ls [-s] [--limited-num] [--marker] 
    ```

    The following parameters are used in the preceding code:

    |Parameter|Description|
    |---------|-----------|
    |-s|Only the name of the bucket is returned in the list results.|
    |--limited-num|Specifies the maximum number of results to return. You can use this parameter in combination with marker to paginate the returned results.|
    |--marker|List buckets whose names are after marker in alphabetical order.|

-   Examples
    -   List all buckets

        ```
        ./ossutil64 ls
        ```

        Or

        ```
        ./ossutil64 ls oss://
        ```

        The following output result indicates that all buckets in the current account are listed. The list includes the name, creation time, region, storage class,and number of buckets.

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

        The following output result indicates that all buckets in the account, including only the bucket name and number of buckets are listed.

        ```
        oss://examplebucketA
        oss://examplebucketB
        oss://examplebucketC
        oss://examplebucketD
        Bucket Number is:4
        0.235104(s) elapsed  
        ```

    -   List buckets in alphabetical order after the specified marker is examplebucketA

        ```
        ./ossutil64 ls oss:// --limited-num=2 -s --marker examplebucketA
        ```

        The following output result indicates that two buckets after examplebucketA are listed.

        ```
        2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucketB
        2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://examplebucketC
        Bucket Number is:2
        0.132174(s) elapsed                        
        ```


## List objects

-   Command syntax

    ```
    ./ossutil64 ls oss://bucket\_name[/prefix] [-s] [-d] [--limited-num] [--marker] [--include] [--exclude]  [--version-id-marker] [--all-versions]
    ```

    The following parameters are used in the preceding code.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket.|
    |prefix|The prefix of the object. Add this parameter when you list the objects that have specified prefixes in the bucket.|
    |-s|Only the name of the object is returned in the list results.|
    |-d|Only the objects and subfolders are listed. The objects in the subfolders are ignored.|
    |--limited-num|Specifies the maximum number of results to return. You can use this parameter in combination with --marker to paginate the returned results.|
    |--marker|Lists objects whose names are after marker in alphabetical order.|
    |--include|Lists objects that meet specified conditions. For example, `*.jpg` indicates that all objects in the JPG format are listed. For more information, see [Upload multiple objects that meet specified conditions](/intl.en-US/Tools/ossutil/Common commands/cp/Upload objects.md).|
    |--exclude|Lists objects that do not meet the specified conditions. For example, `*.txt` indicates that all objects that are not in the TXT format are listed.|
    |--version-id-marker|Lists versions of objects whose version IDs are after marker in alphabetical order. This parameter is available only when versioning is enabled for the bucket.|
    |--all-versions|List all versions of an object. This parameter is available only when versioning is enabled for the bucket.|

-   Examples
    -   List all objects in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket
        ```

        The following output result indicates that all objects in the examplebucket bucket are listed.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:3
        0.007379(s) elapsed                 
        ```

    -   List objects whose names contain the prefix of example in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket/example
        ```

        The following output result indicates that all objects whose names contain the prefix of example in the examplebucket bucket are listed.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        Object Number is:2
        0.007379(s) elapsed                 
        ```

    -   List all MP4 files in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket --include *.mp4
        ```

        The following output result indicates that all MP4 files in the examplebucket bucket are listed.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:1
        0.007379(s) elapsed                 
        ```

    -   List only objects and subfolders in the examplebucket root folder

        ```
        ./ossutil64 ls oss://examplebucket -d
        ```

        The following output result indicates that objects and subfolders in the examplebucket root folder are listed.

        ```
        oss://examplebucket/example.txt
        oss://examplebucket/examplefolder/
        oss://examplebucket/video.mp4
        Object and Directory Number is: 3
        
        0.278489(s) elapsed
        ```

    -   List all versions of all objects in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket --all-versions
        ```

        The following output result indicates that all versions of all objects in the examplebucket bucket are listed.

        ```
        LastModifiedTime                   Size(B)  StorageClass   ETAG                                  VERSIONID                                                           IS-LATEST   DELETE-MARKER   ObjectName
        2020-12-01 15:06:37 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgICUsOuR2hYiIDI3NWVjNmEyYmM0NTRkZWNiMTkxY2VjMDMwZjFlMDA3    true        false           oss://examplebucket/example.txt
        2020-06-11 11:03:37 +0800 CST      363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545      CAEQARiBgIDZtvuR2hYiIDNhYjRkN2M5NTA5OTRlN2Q4YTYzODQwMzQ4NDYwZDdm    true        false           oss://examplebucket/examplefolder/photo.jpg
        2021-01-26 13:27:08 +0800 CST           0                                                       CAEQLxiBgIDd7NH0uRciIDA3Yzg0MTZjOWNlYzQ4ODZhMzVkZWE0MmE2NzBlYTYx    true        true            oss://examplebucket/image.png
        2020-12-01 15:06:45 +0800 CST    57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544      CAEQLBiBgMDZiprwthciIDY2NGM0NTNmZDE3ODRmZmVhZGM4YTUwZGQyNGU3ZjQ3    true        false           oss://examplebucket/video.mp4
        2016-06-11 10:53:46 +0800 CST      118076      Standard   FFDB300F053AAF06F4C4C58A4869C427      CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk    false       false           oss://examplebucket/example.txt
        2016-06-11 11:02:05 +0800 CST      345374      Standard   078A9852BCF81DC4811E6EDCBFD121BE      CAEQARiBgICNz_iR2hYiIGJjZTBjNDQxYWRhNTQ2ZTNiNmMzYzQ1YzMzMDA5ZjUw    false       false           oss://examplebucket/examplefolder/photo.jpg
        Object Number is: 6
        
        0.692000(s) elapsed
        ```

    -   List all versions of example.txt in the examplebucket root folder

        ```
        ./ossutil64 ls oss://examplebucket/example.txt --all-versions
        ```

        The following output indicates that all versions of example.txt are listed.

        ```
        LastModifiedTime                   Size(B)  StorageClass  ETAG                                  VERSIONID                                                           IS-LATEST   DELETE-MARKER  ObjectName
        2020-12-01 15:06:37 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgICUsOuR2hYiIDI3NWVjNmEyYmM0NTRkZWNiMTkxY2VjMDMwZjFlMDA3    true        false           oss://examplebucket/example.txt
        2016-06-11 10:53:46 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk    false       false           oss://examplebucket/example.txt
        Object Number is: 2
        
        0.361000(s) elapsed
        ```


## List parts

-   Command syntax

    ```
    ./ossutil64 ls oss://bucket\_name[/prefix] [-s] [-d] [-m] [-a] [--limited-num] [--upload-id-marker] 
    ```

    The following parameters are used in the preceding code.

    |Parameter|Description|
    |---------|-----------|
    |bucket\_name|The name of the bucket.Add this parameter when you want to list objects in a specified bucket. |
    |prefix|Lists parts that have a specified prefix.|
    |-s|Only the upload ID and the object name are returned in the list results.|
    |-d|Lists objects and subfolders and ignores the objects in the subfolders.|
    |-m|List parts.|
    |-a|List objects and parts.|
    |--limited-num|Specifies the maximum number of results to return. You can use this parameter in combination with --upload-id-marker to paginate the returned results.|
    |--upload-id-marker|List parts whose upload ID letters are after marker in alphabetical order.|

-   Examples
    -   List all parts in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket -m
        ```

        The following output result indicates that all parts in the examplebucket bucket are listed.

        ```
        InitiatedTime                     UploadID                           ObjectName
        2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://examplebucket/test.mp4
        2017-01-13 03:45:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://examplebucket/test.mp4
        2017-01-13 03:45:01 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://examplebucket/test.mp4
        2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://examplebucket/object.exe
        UploadId Number is:4
        0.191289(s) elapsed  
        ```

    -   List all objects and parts in the examplebucket bucket

        ```
        ./ossutil64 ls oss://examplebucket -a
        ```

        The following output result indicates that all objects and parts in the examplebucket bucket are listed.

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
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

When you use the ossutil command line tool to manage buckets in different regions, you can use the -e option to switch to the endpoint to which a specified bucket belongs. When you use the ossutil command line tool to manage buckets that belong to multiple Alibaba Cloud accounts, you can switch to the AccessKey ID of a specified account by using the -i option and switch to the AccessKey secret of the specified account by using the -k option.

For example, you can run the following command to list all objects in the test bucket that belongs to another Alibaba Cloud account in the China \(Hangzhou\) region:

```
./ossutil64 ls oss://test -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options of this command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

