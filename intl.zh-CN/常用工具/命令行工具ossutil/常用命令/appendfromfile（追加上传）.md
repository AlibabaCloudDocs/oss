# appendfromfile（追加上传）

appendfromfile命令用于在已上传的追加类型文件（Appendable Object）末尾直接追加内容。

**说明：**

-   本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。
-   有关追加上传的更多信息，请参见[追加上传](/intl.zh-CN/开发指南/对象/文件（Object）/上传文件（Object）/追加上传.md)。

## 命令格式

```
./ossutil64 appendfromfile local\_file\_name oss://bucket\_name object\_name [--meta ]
```

参数说明如下：

|选项|说明|
|--|--|
|local\_file\_name|本地文件完整路径。|
|bucket\_name|目标Bucket名称。|
|object\_name|目标Object名称。追加上传时可直接将本地文件名称保留为Object名称，也可以自定义上传至Bucket后的Object名称。|
|--meta|设置Object的meta信息。仅支持在首次追加上传时附加此选项，例如`--meta "x-oss-object-acl:private"`。Object的meta信息配置完成后，您可以通过[set-meta](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/set-meta.md)命令修改Object的meta信息。 |

## 使用示例

将本地根目录下的文件exampleobject.txt首次追加上传至目标存储空间examplebucket，然后直接在exampleobject.txt文件末尾多次追加内容。

1.  上传文件exampleobject.txt，并指定文件读写权限为私有。

    ```
    ./ossutil64 appendfromfile exampleobject.txt oss://examplebucket/exampleobject.txt --meta "x-oss-object-acl:private"
    ```

    以下输出结果表明exampleobject.txt已追加上传至目标Bucket，此时文件大小为5 Byte。

    ```
    total append 5(100.00%) byte,speed is 0.00(KB/s)
    local file size is 5,the object new size is 5,average speed is 0.04(KB/s)
    ```

2.  在exampleobject.txt文件末尾追加文件dest.txt的内容。

    如果需要在exampleobject.txt文件末尾多次追加内容，请相应替换如下示例中的待追加上传文件dest.txt。

    ```
    ./ossutil64 appendfromfile dest.txt oss://examplebucket/exampleobject.txt
    ```

    以下输出结果已在exampleobject.txt文件后追加了内容，此时文件大小为150 Byte。

    ```
    total append 150(100.00%) byte,speed is 0.00(KB/s)
    local file size is 150,the object new size is 150,average speed is 1.19(KB/s)
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要向另一个阿里云账号下，华东2（上海）下名为examplebucket的存储空间追加上传exampleobject.txt文件，命令如下：

```
./ossutil64 appendfromfile exampleobject.txt oss://examplebucket/exampleobject.txt -e shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

