# 有关multipart的命令 {#concept_drn_rzs_vdb .concept}

## Multipart的相关命令 {#section_spb_xzs_vdb .section}

ossutil提供了列举UploadID、删除指定object的所有UploadID的功能。

关于Multipart的更多介绍请参见[断点续传](../intl.zh-CN/开发指南/上传文件/断点续传.md#)。

**说明：** ossutil在上传/拷贝文件时，对于大文件自动进行分片断点续传，而不提供UploadPart命令。

-   列举UploadID

    使用-m选项来列举指定object下的所有未完成UploadID，使用-a选项来列举object和UploadID。

    ```
    $ ossutil ls oss://bucket1/obj1 -m
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

-   删除指定object的所有UploadID

    使用-m选项删除指定object下的所有未完成UploadID，当同时指定-r选项时，会删除以指定object为前缀的所有object的未完成UploadID。

    假设bucket1下有如下文件：

    ```
    $ ossutil ls oss://bucket1 -a
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-05 14:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-05 14:36:21 +0000 CST        241561      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1
    2016-04-08 14:50:47 +0000 CST       6476984      Standard    4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is: 3
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-13 03:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is: 4
    0.191289(s) elapsed
    ```

    删除obj1的2个UploadID：

    ```
    $./ossutil rm -m oss://bucket1/obj1
    Succeed: Total 2 uploadIds. Removed 2 uploadIds.
    1.922915(s) elapsed
    ```

    删除obj1和obj2的4个UploadID：

    ```
    $./ossutil rm -m oss://bucket1/ob
    Succeed: Total 4 uploadIds. Removed 4 uploadIds.
    1.922915(s) elapsed
    ```

    同时删除obj1，obj1和obj2的3个UploadID：

    ```
    $./ossutil rm  oss://dest1/.a  -a -r -f
    Do you really mean to remove recursively objects and multipart uploadIds of oss://dest1/.a(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


