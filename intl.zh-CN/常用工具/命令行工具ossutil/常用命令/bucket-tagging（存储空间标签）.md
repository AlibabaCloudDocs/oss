# bucket-tagging（存储空间标签）

您可以通过存储空间（Bucket）的标签功能， 对Bucket进行分类管理，例如对拥有指定标签的Bucket设置访问权限等。bucket-tagging命令用于添加、修改、查询、删除Bucket的标签配置。

**说明：**

-   本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。
-   有关Bucket标签的更多信息，请参见[存储空间标签](/intl.zh-CN/开发指南/存储空间（Bucket）/存储空间标签.md)。

## 添加或修改Bucket标签

存储空间标签使用一组键值对（Key-Value）来标记Bucket，每个Bucket仅允许添加10组标签。只有Bucket的拥有者以及被授予PutBucketTags权限的用户才能为Bucket添加或修改标签，否则返回403 Forbidden错误，错误码为AccessDenied。

-   命令格式

    ```
    ./ossutil64 bucket-tagging --method put oss://bucketname key\#value
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|添加或修改标签信息的Bucket名称。|
    |key\#value|标签信息的Key-Value对。    -   Key和Value以井号（\#）分隔，且Key和Value必须为UTF-8编码。
    -   Key最大长度为64字节，不能以`http://`、`https://`、`Aliyun`为前缀，且不能为空。
    -   Value最大长度为128字节，可以为空。 |

    若Bucket未设置标签，此命令将为Bucket添加指定的标签；若Bucket已设置标签，此命令将覆盖Bucket已有标签。

-   使用示例

    为名为examplebucket的Bucket配置两组标签，其中一组标签的Key为tag1，Value为test1；另一组标签的Key为tag2，Value为test2。

    ```
    ./ossutil64 bucket-tagging --method put oss://examplebucket  tag1#test1 tag2#test2
    ```

    以下输出结果表明已成功添加Bucket标签信息。

    ```
    0.300600(s) elapsed
    ```


## 获取Bucket标签

-   命令格式

    ```
    ./ossutil64 bucket-tagging --method get oss://bucketname
    ```

-   使用示例

    获取examplebucket的标签配置。

    ```
    ./ossutil64 bucket-tagging --method get oss://examplebucket
    ```

    以下输出结果表明examplebucket配置了两组标签，其中一组标签的Key为tag1，Value为test1；另一组标签的Key为tag2，Value为test2。

    ```
    index     tag key       tag value
    ---------------------------------------------------
    0         "tag1"        "test1"
    1         "tag2"        "test2"
    
    0.283359(s) elapsed
    ```


## 删除Bucket标签

-   命令格式

    ```
    ./ossutil64 bucket-tagging --method delete oss://bucketname 
    ```

-   使用示例

    删除examplebucket所有包含的标签信息。

    ```
    ./ossutil64 bucket-tagging --method delete oss://examplebucket
    ```

    以下输出结果表明examplebucket中包含的所有标签信息均已删除。

    ```
    0.530750(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的存储空间配置标签信息，命令如下：

```
./ossutil64 bucket-tagging--method put oss://examplebucket key#value -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

