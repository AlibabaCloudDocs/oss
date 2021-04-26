# object-tagging（对象标签）

OSS支持以标签的方式对存储的对象（Object）进行分类，方便您管理拥有相同标签的Object，例如通过生命周期规则为相同标签的Object指定过期天数或转换其存储类型。object-tagging命令用于添加、修改、获取和删除对象标签。

## 注意事项

-   Binary说明

    本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

-   标签同步规则

    进行跨区域复制时，源Bucket中拥有指定标签的Object会同步到目标Bucket。详情请参见[设置跨区域复制](/intl.zh-CN/控制台用户指南/存储空间管理/冗余与容错/设置跨区域复制.md)。


有关对象标签功能介绍，请参见开发指南的[对象标签](/intl.zh-CN/开发指南/对象/文件（Object）/管理文件/对象标签.md)。

## 命令格式

```
./ossutil64 object-tagging oss://bucketname[/prefix][key#value]
--method <value>
[--encoding-type <value>]
[-r，--recursive]
[--payer <value>]
[--version-id <value>] 
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|bucketname|Bucket名称。|
|prefix|Bucket下的资源，例如目录、文件等。|
|key|对象标签使用一组键值对（Key-Value）标记对象。单个文件可设置最多10个标签，Key不可重复。设置Key时，需满足以下条件：-   每个Key长度不超过128字符，且区分大小写。
-   Key的合法字符集包括大小写字母、数字、空格和以下符号：

+=.\_:/ |
|value|设置Value时，需满足以下条件：-   每个Value长度不超过256字符，且区分大小写。
-   Value的合法字符集包括大小写字母、数字、空格和以下符号：

+=.\_:/ |
|--method|请求类型。取值如下：-   put：添加或修改对象标签。
-   get：获取对象标签。
-   delete：删除对象标签。 |
|--encoding-type|对`oss://bucket_name`之后的prefix进行编码，取值为url。如果不指定该选项，则表示prefix未经过编码。|
|-r，--recursive|如果指定该选项时，ossutil将为Bucket下所有符合prefix条件的Object设置标签。如果不指定该选项，则ossutil只为指定Object设置标签。|
|--version-id|Object的指定版本。仅适用于已开启或暂停版本控制状态Bucket下的Object。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|

## 添加或修改Object标签

只有Bucket拥有者以及被授予PutObjectTagging的RAM用户拥有设置或修改Object标签的权限。

添加或修改Object标签示例如下：

**说明：** 若Object未设置标签，执行如下操作将为Object添加指定的标签；若Object已配置标签，执行如下操作将覆盖Object原有标签。

-   为目标存储空间examplebucket下的exampleobject.txt文件设置key为tagkey，value为tagvalue的标签信息。

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.txt tagkey#tagvalue
    ```

-   为目标存储空间examplebucket下的exampleobject.png文件设置两组标签信息。其中一组标签信息为tagkey1\#tagvalue1，另一组标签信息为tagkey2\#tagvalue2。

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.png.txt tagkey1#tagvalue1 tagkey2#tagvalue2
    ```

-   为目标存储空间examplebucket下与前缀test匹配的多个文件设置三组标签信息，标签信息分别为tagkey3\#tagvalue3、tagkey4\#tagvalue4和tagkey5\#tagvalue5。

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/test -r tagkey3#tagvalue3 tagkey4#tagvalue4 tagkey5#tagvalue5
    ```

-   为已开启版本控制的存储空间examplebucket下的exampleobject.txt的指定版本设置key为tagkey6，value为tagvalue6的标签信息。

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.txt tagkey6#tagvalue6 --version-id CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
    ```

    有关获取Object所有版本的具体操作，请参见[ls（列举）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/ls（列举）.md)。

-   以上示例操作成功后，返回结果中将包含设置标签信息所用时长，示例如下：

    ```
    0.106852(s) elapsed
    ```


## 获取Object标签信息

只有Bucket拥有者以及被授予GetObjectTagging的RAM用户拥有获取Object标签信息的权限。

获取Object标签信息的示例如下：

-   获取单个Object的标签信息

    获取目标存储空间examplebucket下exampleobject.txt的标签信息。

    ```
    ./ossutil64 object-tagging --method get oss://examplebucket/exampleobject.txt
    ```

    以下输出结果表明已成功获取exampleobject.txt的标签信息，标签信息的key为tagkey，value为tagvalue。

    ```
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "tagkey"  "tagvalue"      oss://examplebucket/exampleobject.txt
    
    0.068156(s) elapsed
    ```

-   获取多个Object的标签信息

    获取目标存储空间examplebucket下与前缀test匹配的文件的标签信息。

    ```
    ./ossutil64 object-tagging --method get oss://examplebucket/test -r
    ```

    以下输出结果表明已成功获取与前缀test匹配的所有文件的标签信息，分别为tagkey3\#tagvalue3、tagkey4\#tagvalue4和tagkey5\#tagvalue5。

    ```
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "tagkey3" "tagvalue3"     oss://examplebucket/test
    1              1              "tagkey4" "tagvalue4"     oss://examplebucket/test
    1              2              "tagkey5" "tagvalue5"     oss://examplebucket/test
    
    0.093040(s) elapsed
    ```


## 删除Object标签信息

只有Bucket拥有者以及被授予DeletetObjectTagging的RAM用户拥有删除Object标签信息的权限。

删除Object标签信息的示例如下：

-   删除单个Object的标签信息

    删除目标存储空间examplebucket下exampleobject.txt的标签信息。

    ```
    ./ossutil64 object-tagging --method delete oss://examplebucket/exampleobject.txt
    ```

-   删除多个Object的标签信息

    删除目标存储空间examplebucket下与前缀test匹配的所有文件的标签信息。

    ```
    ./ossutil64 object-tagging --method delete oss://examplebucket/test -r
    ```

-   以上示例操作成功后，返回结果中将包含删除标签信息所用时长，示例如下：

    ```
    0.148970(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东2（上海）地域下目标存储空间testbucket下的exampletest.png文件设置key为tagkey7、value为tagvalue7的标签信息，命令如下：

```
./ossutil64 object-tagging --method put oss://testbucket/exampletest.png tagkey7#tagvalue7 -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

