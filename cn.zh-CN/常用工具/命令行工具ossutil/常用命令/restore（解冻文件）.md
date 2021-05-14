# restore（解冻文件）

访问归档或冷归档类型文件（Object）之前，需要通过restore命令解冻文件。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   有关归档或冷归档文件的解冻状态以及费用说明，请参见[解冻文件](/cn.zh-CN/开发指南/对象/文件（Object）/管理文件/解冻文件.md)。

## 命令格式

```
./ossutil64 restore oss://bucketname[/prefix][local\_xml\_file]
[--encoding-type <value>]
[--payer <value>]
[--version-id <value>]
[-r, --recursive]
[-f, --force] 
[--retry-times <value>]
[-j，--job <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|Bucket名称。|
|prefix|Bucket下的资源，例如目录、文件等。|
|local\_xml\_file|本地XML格式文件，用于保存冷归档类型文件的解冻参数。|
|--encoding-type|对prefix名称进行编码，取值为url。如果不指定该选项，则表示prefix未经过编码。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|
|--version-id|Object的指定版本。仅适用于已开启或暂停版本控制状态Bucket下的Object。|
|-r，--recursive|如果指定该选项时，ossutil将解冻所有与prefix匹配的文件。如果不指定该选项，则ossutil只解冻指定文件。|
|-f, --force|强制操作，不进行询问提示。|
|--retry-times|发生错误后的重试次数。默认值为10，取值范围为1~500。|
|-j，--job|多文件操作时的并发任务数，默认值为3，取值范围为1~10000。|

## 使用示例

-   解冻归档类型Object

    解冻归档类型Object需要1分钟时间，解冻中的Object不支持读取。

    解冻状态默认持续1天。对解冻状态的文件调用restore命令，会将文件的解冻状态延长1天，最多可以延长到7天，之后文件又回到初始时的冷冻状态。

    -   以下示例用于解冻目标存储空间examplebucket下名为exampleobject.txt的归档类型Object。

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.txt
        ```

    -   以下示例通过结合-r选项，解冻目标存储空间examplebucket下与指定前缀dest匹配的所有归档类型Object。

        ```
        ./ossutil64 restore oss://examplebucket/dest -r                         
        ```

    -   以下示例用于解冻目标存储空间examplebucket下归档类型文件exampleobject.txt的指定版本。

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
        ```

        有关获取Object所有版本的具体操作，请参见[ls（列举）](/cn.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)。

-   解冻冷归档类型Object

    在1小时内解冻目标存储空间examplebucket下的exampleobject.jpg文件，并且解冻状态保持3天。操作步骤如下：

    1.  创建本地XML格式文件config.xml，并在文件中配置如下解冻参数。

        ```
        <RestoreRequest>
            <Days>3</Days>
            <JobParameters>
                <Tier>Expedited</Tier>
            </JobParameters>
        </RestoreRequest>
        ```

        配置参数说明如下：

        |参数|说明|
        |--|--|
        |Days|冷归档文件保持解冻状态的时间，单位为天。取值范围：1~7 |
        |Tier|冷归档文件解冻优先级。取值如下：

        -   Expedited（高优先级）：1小时内完成解冻。
        -   Standard（标准）：2~5小时完成解冻。
        -   Bulk（批量）：5~12小时完成解冻。 |

    2.  执行如下命令解冻冷归档文件exampleobject.jpg。

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.jpg config.xml
        ```

    **说明：** 根据解冻文件的大小，实际解冻时间可能会有变化，请以实际解冻时间为准。

-   返回结果

    以上操作成功后，返回结果中将包含发起解冻请求所用时长，示例如下：

    ```
    0.106852(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要解冻另一个阿里云账号下，华东2（上海）地域下目标存储空间testbucket下的exampletest.png文件，命令如下：

```
./ossutil64 restore oss://testbucket/exampletest.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

