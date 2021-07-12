# lrb（列举地域级别下的Bucket）

您可以通过lrb命令获取单个或多个地域（Region）下存储空间（Bucket）的基本信息，包括Bucket的名称、创建时间、存储类型以及个数等。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

## 命令格式

```
./ossutil64 lrb conf\_file [-e <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|conf\_file|如果您希望获取多个Region下的Bucket信息时，请输入一个本地配置文件，并在文件中换行填写多个Region对应的Endpoint。|
|-e|如果您仅希望获取单个Region下的Bucket信息，请通过此选项指定该Region对应的Endpoint。|

## 使用示例

-   获取单个地域的Bucket信息

    获取华东1（杭州）地域下的Bucket信息。

    ```
    ./ossutil64 lrb -e oss-cn-hangzhou.aliyuncs.com
    ```

    获取与ossutil配置文件中指定Endpoint关联Region下的Bucket信息。有关ossutil配置文件中Endpoint的信息，请参见[config命令](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/config（创建配置文件）.md)。

    ```
    ./ossutil64 lrb
    ```

-   获取多个地域的Bucket信息
    1.  创建本地文件，并在文件中配置Endpoint信息。

        在本地创建名为`localfile.txt`文件，并根据使用场景填写待获取Bucket信息所在Region对应的Endpoint。`localfile.txt`配置如下：

        ```
        oss-cn-hangzhou-aliyuncs.com
        oss-cn-shenzhen-aliyuncs.com
        oss-cn-shanghai-aliyuncs.com
        ```

    2.  获取华东1（杭州）、华南1（深圳）以及华东2（上海）地域下的Bucket信息。

        ```
        ./ossutil64 lrb localfile.txt
        ```

-   返回结果

    获取Bucket信息成功后，返回结果中将包含Bucket的名称、创建时间、存储类型以及个数以及获取所用时长。以获取多个Region下的Bucket信息为例，其返回结果如下：

    ```
    CreationTime                                 Region    StorageClass    BucketName
    2021-07-06 14:21:09 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucket1
    2021-07-06 14:21:44 +0800 CST       oss-cn-hangzhou        Standard    oss://examplebucket2
    2021-06-16 18:32:32 +0800 CST       oss-cn-shanghai        Standard    oss://examplebucket3
    2021-06-30 16:04:41 +0800 CST       oss-cn-shanghai        Standard    oss://examplebucket4
    2021-07-07 12:33:35 +0800 CST       oss-cn-shenzhen        Standard    oss://examplebucket5
    
    Bucket Number is: 5
    0.124193(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要获取另一个阿里云账号下，华东2（上海）地域下的Bucket信息，命令如下：

```
./ossutil64 lrb -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

