# ls（列举）

您可以通过ls命令列举存储空间（Bucket）、对象（Object）和碎片（Part）信息。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 列举Bucket

-   命令格式

    ```
    ./ossutil64 ls [-s] [--limited-num] [--marker] 
    ```

    参数说明如下：

    |选项|说明|
    |--|--|
    |-s|列举结果仅返回Bucket的名称。|
    |--limited-num|设定返回结果的最大个数。您可以使用此项结合marker对返回结果进行分页展示。|
    |--marker|列举名称字母序排在marker之后的Bucket。|

-   使用示例
    -   列举所有Bucket

        ```
        ./ossutil64 ls
        ```

        或

        ```
        ./ossutil64 ls oss://
        ```

        以下输出结果表明已成功列举当前账号下所有Bucket，包括Bucket名称、创建时间、所在地域、存储类型、数量等信息。

        ```
        2016-10-21 16:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://examplebucketA
        2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucketB
        2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://examplebucketC
        2016-10-21 17:31:27 +0800 CST       oss-cn-hangzhou         Archive    oss://examplebucketD
        Bucket Number is:4
        0.252174(s) elapsed  
        ```

    -   以精简模式列举所有Bucket

        ```
        ./ossutil64 ls -s
        ```

        以下输出结果表明已成功列举当前账号下所有Bucket，仅包含Bucket名称以及Bucket数量。

        ```
        oss://examplebucketA
        oss://examplebucketB
        oss://examplebucketC
        oss://examplebucketD
        Bucket Number is:4
        0.235104(s) elapsed  
        ```

    -   列举字母序排在指定marker为examplebucketA之后的Bucket

        ```
        ./ossutil64 ls oss:// --limited-num=2 -s --marker examplebucketA
        ```

        以下输出结果表明已成功列举examplebucketA之后的2个Bucket。

        ```
        2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucketB
        2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://examplebucketC
        Bucket Number is:2
        0.132174(s) elapsed                        
        ```


## 列举Object

-   命令格式

    ```
    ./ossutil64 ls oss://bucket\_name[/prefix] [-s] [-d] [--limited-num] [--marker] [--include] [--exclude]  [--version-id-marker] [--all-versions]
    ```

    参数说明如下：

    |选项|说明|
    |--|--|
    |bucket\_name|目标Bucket名称。|
    |prefix|目标Object前缀。当您列举目标Bucket中指定前缀的Object时添加此项。|
    |-s|列举结果仅返回Object的名称。|
    |-d|仅列举Object和子目录，忽略子目录下的Object。|
    |--limited-num|设定返回结果的最大个数。您可以使用此项结合--marker，对返回结果进行分页展示。|
    |--marker|列举名称字母排序在marker之后的Object。|
    |--include|列举符合指定条件的Object。例如`*.jpg`表示列举所有JPG格式的文件。更多信息，请参见[批量上传符合条件的文件](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/cp/上传文件.md)。|
    |--exclude|列举不符合指定条件的Object。例如`*.txt`表示列举所有非TXT格式的文件。|
    |--version-id-marker|列举Version ID字母排序在marker之后的Object版本。仅当Bucket开启版本控制后可用。|
    |--all-versions|列举Object的所有版本，仅当Bucket开启版本控制后可用。|

-   使用示例
    -   列举examplebucket内所有Object

        ```
        ./ossutil64 ls oss://examplebucket
        ```

        以下输出结果表明已成功列举examplebucket内所有Object的信息，包括文件最后更新时间（LastModifiedTime）、以字节为单位统计的文件大小（Size）、ETag值以及文件名称（ObjectName）。

        其中，ETag值用于标识一个Object的内容。对于通过PutObject请求创建的Object，ETag值是其内容的MD5值；对于通过其他方式创建的Object，ETag值是其内容的UUID。

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                    ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:3
        0.007379(s) elapsed                 
        ```

    -   列举examplebucket内前缀为example的Object

        ```
        ./ossutil64 ls oss://examplebucket/example
        ```

        以下输出结果表明已成功列举examplebucket内所有前缀为example的Object。

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                     ObjectName
        2020-12-01 15:06:37 +0800 CST           114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://examplebucket/example.txt
        2020-12-01 15:06:42 +0800 CST        363812      Standard   E7581E5D2EBC56ECCB6FB6050B4C6545        oss://examplebucket/examplefolder/photo.jpg
        Object Number is:2
        0.007379(s) elapsed                 
        ```

    -   列举examplebucket内所有后缀名为.mp4的文件

        ```
        ./ossutil64 ls oss://examplebucket --include *.mp4
        ```

        以下输出结果表明已成功列举examplebucket内所有后缀名为.mp4的文件。

        ```
        LastModifiedTime                    Size(B)  StorageClass   ETag                                     ObjectName
        2020-12-01 15:06:45 +0800 CST      57374182      Standard   BE97B7AD7A2C1277B11221E5C9537544        oss://examplebucket/video.mp4
        Object Number is:1
        0.007379(s) elapsed                 
        ```

    -   仅列举examplebucket根目录下Object和子目录

        ```
        ./ossutil64 ls oss://examplebucket -d
        ```

        以下输出结果表明已成功列举examplebucket根目录下Object和子目录。

        ```
        oss://examplebucket/example.txt
        oss://examplebucket/examplefolder/
        oss://examplebucket/video.mp4
        Object and Directory Number is: 3
        
        0.278489(s) elapsed
        ```

    -   列举examplebucket内所有Object的所有版本

        ```
        ./ossutil64 ls oss://examplebucket --all-versions
        ```

        以下输出结果表明已成功列举examplebucket内所有Object的所有版本。

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

    -   列举examplebucket根目录下example.txt的所有版本

        ```
        ./ossutil64 ls oss://examplebucket/example.txt --all-versions
        ```

        以下输出结果表明已成功列举example.txt的所有版本。

        ```
        LastModifiedTime                   Size(B)  StorageClass  ETag                                   VERSIONID                                                           IS-LATEST   DELETE-MARKER  ObjectName
        2020-12-01 15:06:37 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgICUsOuR2hYiIDI3NWVjNmEyYmM0NTRkZWNiMTkxY2VjMDMwZjFlMDA3    true        false           oss://examplebucket/example.txt
        2016-06-11 10:53:46 +0800 CST         114      Standard   61DE142E5AFF9A6748707D4A77BFBCFB      CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3MDRk    false       false           oss://examplebucket/example.txt
        Object Number is: 2
        
        0.361000(s) elapsed
        ```


## 列举Part

-   命令格式

    ```
    ./ossutil64 ls oss://bucket\_name[/prefix] [-s] [-d] [-m] [-a] [--limited-num] [--upload-id-marker] 
    ```

    参数说明如下：

    |选项|说明|
    |--|--|
    |bucket\_name|目标Bucket名称。当您需要列举指定Bucket中的Object时添加此项。 |
    |prefix|列举指定前缀下的Part。|
    |-s|列举结果仅返回UploadID和Object名称。|
    |-d|列举Object和子目录，忽略子目录下的Object。|
    |-m|列举Part。|
    |-a|列举Object和Part。|
    |--limited-num|设定返回结果的最大个数。您可以使用此项结合--upload-id-marker对返回结果进行分页展示。|
    |--upload-id-marker|列举Upload ID字母排序在marker之后的Part。|

-   使用示例
    -   列举examplebucket内所有Part

        ```
        ./ossutil64 ls oss://examplebucket -m
        ```

        以下输出结果表明已成功列举examplebucket内所有Part。

        ```
        InitiatedTime                     UploadID                           ObjectName
        2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://examplebucket/test.mp4
        2017-01-13 03:45:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://examplebucket/test.mp4
        2017-01-13 03:45:01 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://examplebucket/test.mp4
        2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://examplebucket/object.exe
        UploadId Number is:4
        0.191289(s) elapsed  
        ```

    -   列举examplebucket内所有Object和Part

        ```
        ./ossutil64 ls oss://examplebucket -a
        ```

        以下输出结果表明已成功列举examplebucket内所有Object和Part。

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


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要列举另一个阿里云账号下，华东1（杭州）名为test的Bucket内所有文件，命令如下：

```
./ossutil64 ls oss://test -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

