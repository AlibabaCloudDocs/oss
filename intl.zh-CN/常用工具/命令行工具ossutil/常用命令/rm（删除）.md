# rm（删除）

您可以通过`rm`命令删除不再需要的文件（Object）、碎片（Part）或存储空间（Bucket），以免产生不必要的存储费用。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 rm oss://bucket\_name[/prefix]
[-r，--recursive]
[-b，--bucket]
[-m，--multipart]
[-a，--all-type]
[-f，--force]
[--include <value>]
[--exclude <value>]
[--version-id <value>] 
[--all-versions]
[--payer <value>]
[--encoding-type <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucket\_name|Bucket名称。|
|prefix|Bucket下的资源、例如目录、文件等。|
|-r，--recursive|如果指定该选项时，ossutil将删除Bucket下所有符合prefix条件的Object。如果不指定该选项，则ossutil只删除指定Object。|
|-b，--bucket|仅在删除Bucket时使用此选项。|
|-m，--multipart|指定操作的对象为Bucket中未完成的Multipart事件。|
|-a，--all-type|指定操作的对象为Bucket中符合prefix条件的Object和未完成的Multipart事件。|
|-f，--force|强制操作，不进行询问提示。|
|--include|包含符合指定条件的所有Object。|
|--exclude|不包含任何符合指定条件的Object。|
|--version-id|Object的指定版本。仅适用于已开启或暂停版本控制状态Bucket下的Object。|
|--all-versions|Object的所有版本。仅适用于已开启或暂停版本控制状态Bucket下的Object，且同一个删除示例中仅允许选择--version-id或--all-versions其中一个选项。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|--encoding-type|对`oss://bucket_name`之后的prefix进行编码，取值为url。如果不指定该选项，则表示prefix未经过编码。|

## 删除Object

-   删除示例
    -   删除单个Object

        删除目标存储空间examplebucket下名为exampleobject.txt文件。

        ```
        ./ossutil64 rm oss://examplebucket/exampleobject.txt
        ```

    -   删除目标存储空间examplebucket下与前缀test匹配的所有文件。

        ```
        ./ossutil64 rm oss://examplebucket/test -r
        ```

    -   删除目标存储空间examplebucket下后缀为.png的所有文件。

        ```
        ./ossutil64 rm oss://examplebucket  --include "*.jpg" -r
        ```

    -   删除目标存储空间examplebucket下文件名包含abc，且后缀不是.jpg和.txt的文件。

        ```
        ./ossutil64 rm oss://examplebucket  --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
        ```

    -   删除已开启版本控制的存储空间examplebucket下exampleobject.txt的指定版本。

        ```
        ./ossutil64 rm oss://examplebucket/exampleobject.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
        ```

        有关获取Object所有版本的具体操作，请参见[ls（列举）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)。

    -   删除已开启版本控制的存储空间examplebucket下exampleobject.txt的所有版本。

        ```
        ./ossutil64 rm oss://examplebucket/exampleobject.txt --all-versions
        ```

    -   删除已开启版本控制的存储空间examplebucket下所有Object的所有版本。

        ```
        ./ossutil64 rm oss://examplebucket --all-versions -r
        ```

-   返回结果

    以上示例删除成功后，返回结果中将包含删除的Object个数以及完成删除操作所用时长，示例如下：

    ```
    Succeed: Total 8 objects. Removed 8 objects.
    0.106852(s) elapsed
    ```


## 删除Part

-   删除示例
    -   结合-m选项删除目标存储空间examplebucket下exampleobject.txt中未完成的Multipart事件所产生的Part。

        ```
        ./ossutil64 rm -m oss://examplebucket/exampleobject.txt
        ```

    -   结合-m和-r选项递归删除目标存储空间examplebucket下与指定前缀test匹配的所有文件中未完成的Multipart事件所产生的Part。

        ```
        ./ossutil64 rm -m oss://examplebucket/test -r 
        Do you really mean to remove recursively multipart uploadIds of oss://examplebucket/test(y or N)? y 
        ```

    -   结合-a和-r选项递归删除目标存储空间examplebucket下与指定前缀src匹配的所有已上传完成文件、以及未完成的Multipart事件所产生的Part。

        ```
        ./ossutil64 rm  oss://examplebucket/src -a -r
        Do you really mean to remove recursively objects and multipart uploadIds of oss://examplebucket/src(y or N)? y
        ```

-   返回结果

    以上示例删除成功后，返回结果中将包含删除的Object数量、Part对应的UploadID数量以及完成删除操作所用时长，示例如下：

    ```
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    1.922915(s) elapsed
    ```


## 删除Bucket

-   删除不包含Object或Part的目标存储空间examplebucket。

    ```
    ./ossutil64 rm  oss://examplebucket -b
    Do you really mean to remove the Bucket: examplebucket(y or N)? y
    ```

    返回结果中包含删除的Bucket名称以及完成删除操作所用时长。

    ```
    Removed Bucket: examplebucket
    2.230745(s) elapsed
    ```

-   删除目标存储空间examplebucket及其包含的所有Object和Part。

    **警告：** 此操作将清除Bucket中的所有数据，且删除后不可恢复。请谨慎使用。

    ```
    ./ossutil64 rm  oss://examplebucket -b -a -r
    Do you really mean to remove recursively objects and multipart uploadIds of oss://examplebucket(y or N)? y
    Do you really mean to remove the Bucket: examplebucket(y or N)? y
    ```

    返回结果中包含删除的Object数量、Part对应的UploadID数量、Bucket名称完成删除操作所用时长。

    ```
    Succeed: Total 189 objects, 37 uploadIds. Removed 189 objects, 37 uploadIds.
    Removed Bucket: examplebucket
    9.184193(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要删除另一个阿里云账号下，华东2（上海）地域下目标存储空间testbucket下的exampletest.png文件，命令如下：

```
./ossutil64 rm oss://testbucket/exampletest.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

