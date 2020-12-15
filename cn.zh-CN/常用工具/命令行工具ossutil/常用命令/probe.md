# probe

probe命令是针对OSS访问的探测命令，可用于排查本地与OSS之间的网络状态、上传下载带宽、本地符号链接（软链接）状态等。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 使用说明

当您需要了解本地与OSS之间的网络异常、查看上传下载带宽、上传大量软链接文件时，都可以使用probe进行探测。

-   探测网络异常

    ossutil通过上传或下载的方式探测本地与OSS之间的网络状况。如果您在探测的同时需要将指定文件上传或下载到指定位置，建议您使用实际的文件进行上传下载探测；若您仅需要探测网络异常，可以在上传或下载探测时不添加文件名，ossutil会使用临时文件进行探测，并在探测结束后删除这些文件。

-   查看上传下载带宽

    探测上传下载带宽时，OSS会根据您的设备CPU及您的带宽给出一个上传下载的并发建议值。您可以按照ossutil的建议设置并发，以最大限度地利用您的带宽。

-   探测本地软链接状态

    当您上传大量软链接文件时，若某个链接出现异常会导致上传中断。建议您在上传前，先探测本地软链接文件是否存在异常。若存在异常，请修复异常文件后再进行批量上传。


probe命令运行后，您可以查看到任务执行的步骤及结果。

-   上传和下载探测
    -   执行步骤出现√表示本项检测通过，×表示未通过。
    -   若探测成功，ossutil会给出文件的大小、上传下载时间、文件路径等信息。
    -   若探测失败，ossutil会给出导致错误的原因或错误码，您可以根据提示排查问题原因。

        关于错误码的介绍，请参见[OSS错误响应](/cn.zh-CN/开发指南/错误处理/错误响应.md)。

    -   探测结束后会在ossutil的安装目录生成一个格式为`logOssProbe+探测结束时间.log`的日志文件，里面包含此次probe命令执行的详细信息。
-   指定项目探测

    探测上传下载带宽、本地软链接状态时，ossutil会直接给出探测结果和可能的建议。


## 上传文件并输出探测报告

ossutil上传文件到目标存储空间（Bucket）的方式探测本地和目标Bucket之间的网络状态。

-   命令格式

    ```
    ./ossutil probe {--upload [file_name]} {--bucketname bucket_name} [--object object_name] [--addr domain_name] [--upmode]
    ```

    参数说明如下：

    |参数|是否必选|说明|
    |--|----|--|
    |--upload|是|指定探测方式为上传探测。|
    |file\_name|否|填写要上传至目标Bucket的本地文件完整路径；置空此项时，ossutil会生成一个临时文件进行上传探测。|
    |--bucketname|否|目标Bucket名称。|
    |--object|否|增加此项并指定文件名称，ossutil会将上传文件按指定名称保存在Bucket中；若不增加此项，ossutil会在探测结束后删除上传的文件。|
    |--addr|否|增加此项并填写正确的网络地址，ossutil会通过ping操作验证本地到目标地址的网络连通性。默认值：`www.aliyun.com` |
    |--upmode|否|指定文件的上传方式。取值：

    -   normal（默认值）：简单上传
    -   append：追加上传
    -   multipart：分片上传 |

-   使用示例
    -   上传随机文件并指定ping操作的目标地址

        上传的目标Bucket名为examplebucket，ping操作的目标网络地址为`aliyun.com`。命令如下：

        ```
        ./ossutil probe --upload --bucketname examplebucket --add aliyun.com
        ```

        输出如下：

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(normal)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:122880(byte)
        upload time consuming:245(ms)
        (only the time consumed by probe command)
        
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201173031.log
        ```

    -   使用默认上传方式上传指定文件，并在探测结束后删除该文件

        将本地根目录下的文件example.txt上传到名为examplebucket的Bucket。命令如下：

        ```
        ./ossutil probe --upload example.txt --bucketname examplebucket 
        ```

        输出如下：

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(normal)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:7(byte)
        upload time consuming:224(ms)
        (only the time consumed by probe command)
        upload object is example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201173841.log
        ```

    -   使用追加上传方式上传指定文件，并在探测结束后保留该文件

        将本地根目录下的文件example.txt上传到名为examplebucket的Bucket。命令如下：

        ```
        ./ossutil probe --upload example.txt --bucketname examplebucket --object example.txt --upmode append
        ```

        输出如下：

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(append)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:7(byte)
        upload time consuming:171(ms)
        (only the time consumed by probe command)
        
        upload object is example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201174126.log
        ```


## 通过文件URL下载文件并输出探测报告

ossutil通过使用文件URL下载目标文件到本地的方式探测本地和目标Bucket之间的网络状态。

-   命令格式

    ```
    ./ossutil probe {--download} {--url http_url} [--addr=domain_name] [file_name]
    ```

    参数说明如下：

    |参数|是否必选|说明|
    |--|----|--|
    |--download|是|指定探测方式为下载探测。|
    |--url|是|填写文件URL，ossutil会将URL对应的文件下载到本地。    -   公共读文件：直接填写文件URL。例如`https://examplebucket.oss-cn-beijing.aliyuncs.com/example.jpg`。
    -   私有文件：填写带签名的文件URL，URL需使用双引号（“”）包裹。例如`“https://examplebucket.oss-cn-beijing.aliyuncs.com/example.jpg?Expires=1552015472&OSSAccessKeyId=TMP.******5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3A******oCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D”`。 |
    |--addr|否|增加此项并填写正确的网络地址，ossutil会通过ping操作验证本地到目标地址的网络连通性。默认值：`www.aliyun.com` |
    |file\_name|否|设置文件下载到本地后的保存路径。    -   仅指定文件名，未指定目录：ossutil会将文件按指定名称保存到ossutil的安装目录。
    -   仅指定目录，未指定文件名：ossutil会将文件按原名称保存到指定目录。
    -   置空此项：ossutil会将文件按原名称保存到ossutil的安装目录。 |

-   使用示例
    -   通过文件URL下载目标文件并重命名

        文件URL为`https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt`，指向examplebucket中一个公共读文件`example.txt`，下载到本地后重命名为`/localfile/test.txt`。命令如下：

        ```
        ./ossutil probe --download --url https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt  /localfile/test.txt
        ```

        输出如下：

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1246(ms)
        (only the time consumed by probe command)
        
        download file is /localfile/test.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202171639.log
                                        
        ```

    -   通过文件URL下载目标文件并指定ping操作的目标地址

        文件URL为`https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt`，指向examplebucket中一个公共读文件`example.txt`，ping操作的目标地址为`aliyun.com`。命令如下：

        ```
        ./ossutil probe --download --url https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt --add aliyun.com
        ```

        输出如下：

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1344(ms)
        (only the time consumed by probe command)
        
        download file is /root/example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202172232.log                                
        ```


## 直接下载指定文件并输出探测报告

ossutil通过直接下载目标Bucket中的文件到本地的方式探测本地和目标Bucket之间的网络状态。

-   命令格式

    ```
    ./ossutil probe {--download} {--bucketname bucket_name} [--object object_name] [--add domain_name] [file_name]
    ```

    参数说明如下：

    |参数|是否必选|说明|
    |--|----|--|
    |--download|是|指定探测方式为下载探测。|
    |--bucketname|是|目标Bucket名称。|
    |--object|否|增加此项并指定文件名称，ossutil会将指定文件下载到本地；置空此项，ossutil会生成一个临时文件上传到目标Bucket后再将其下载，探测结束后会将该临时文件从目标Bucket中删除。|
    |--addr|否|增加此项并填写正确的网络地址，ossutil会通过ping操作验证本地到目标地址的网络连通性。默认值：`www.aliyun.com` |
    |file\_name|否|设置文件下载到本地后的保存路径。    -   仅指定文件名，未指定目录：ossutil会将文件按指定名称保存到ossutil的安装目录。
    -   仅指定目录，未指定文件名：ossutil会将文件按原名称保存到指定目录。
    -   置空此项：ossutil会将文件按原名称保存到ossutil的安装目录。 |

-   使用示例
    -   下载指定文件并重命名

        将名为examplebucket的Bucket中的文件`/ossfolder/example.txt`下载到本地后重命名为`/localfolder/text.txt`。

        ```
        ./ossutil probe --download --bucketname examplebucket --object /ossfolder/example.txt /localfolder/text.txt
        ```

        输出如下：

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1108(ms)
        (only the time consumed by probe command)
        
        download file is /localfolder/text.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202173032.log
        ```

    -   下载临时文件并指定ping操作的目标地址

        目标Bucket名为examplebucket，ping操作的目标地址为`aliyun.com`。命令如下：

        ```
        ./ossutil probe --download --bucketname examplebucket --add aliyun.com
        ```

        输出如下：

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:122880(byte)
        download time consuming:516(ms)
        (only the time consumed by probe command)
        
        download file is /root/oss-test-probe-1606902911701892000-3ifwj9t0ln
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202173512.log
        ```


## 指定项目探测

ossutil可通过指定探测项目的方式探测本地软链接文件状态、上传和下载带宽，并根据结果给出异常软链接的名称、上传下载并发数建议。

-   命令格式

    ```
    ./ossutil probe {--probe-item item_value} {--bucketname bucket-name} [--object object_name]
    ```

    参数说明如下：

    |参数|是否必选|说明|
    |--|----|--|
    |--probe-item|是|指定探测项目。取值：

    -   cycle-symlink：探测本地是否存在异常软链接。
    -   upload-speed：探测上传带宽。
    -   download-speed：探测下载带宽。 |
    |--bucketname|当--probe-item取值不为`cycle-symlink`时必选|目标Bucket名称。|
    |--object|当--probe-item取值为`download-speed`时必选|填写目标文件的访问路径。目标文件需真实存在且建议大于5 MB。例如`ossfolder/example.txt`。|

-   使用示例
    -   探测本地根目录下localfolder目录是否存在异常软链接

        命令如下：

        ```
        ./ossutil probe --probe-item cycle-symlink /root/localfolder
        ```

        输出如下：

        ```
        Error: stat /root/localfolder/example.jpg: no such file or directory
        ```

        输出内容表示example.jpg为异常软链接。

    -   探测上传带宽

        上传一个临时文件到examplebucket，并根据当前设备的硬件配置及上传带宽给出上传并发数的配置建议。

        命令如下：

        ```
        ./ossutil probe --probe-item upload-speed --bucketname examplebucket
        ```

        输出如下：

        ```
        cpu core count:2
        parallel:2,average speed:679.72(KB/s),current speed:1344.00(KB/s),max speed:1440.00(KB/s))
        parallel:3,average speed:643.31(KB/s),current speed:704.00(KB/s),max speed:1632.00(KB/s))
        parallel:4,average speed:646.62(KB/s),current speed:512.00(KB/s),max speed:1600.00(KB/s))
        
        suggest parallel is 2, max average speed is 679.72(KB/s)
        ```

        输出内容表示设备CPU为双核，上传最大平均带宽为679.72 KB/s，建议上传并发数设置为2。

    -   探测下载带宽

        将examplebucket中的文件`example.txt`下载到本地，并根据当前设备的硬件配置及下载带宽给出下载并发数的配置建议。

        命令如下：

        ```
        ./ossutil probe --probe-item download-speed --bucketname examplebucket --object example.txt
        ```

        输出如下：

        ```
        cpu core count:2
        parallel:2,average speed:12524.93(KB/s),current speed:12288.63(KB/s),max speed:14302.25(KB/s)
        parallel:3,average speed:12564.45(KB/s),current speed:12144.39(KB/s),max speed:14484.24(KB/s)
        parallel:4,average speed:12545.21(KB/s),current speed:12766.58(KB/s),max speed:13534.42(KB/s)
        
        suggest parallel is 3, max average speed is 12564.45(KB/s)
        ```

        输出内容表示设备CPU为双核，下载最大平均带宽为12564.45 KB/s，建议下载并发数设置为3。


