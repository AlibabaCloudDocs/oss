# hash（计算CRC64或MD5）

hash命令用于计算本地文件的CRC64或MD5。

**说明：** 本文命令均以Linux系统为例，实际使用时，请将命令名称改为您实际可执行程序文件的名称。例如Windows 32位系统的帮助命令为ossutil32.exe help。

## 命令格式

```
./ossutil hash file_url [--type=hashtype]
```

--type选项用来控制计算的类型，可选值为crc64和md5，默认值为crc64。

-   OSS文件的CRC64和Content-MD5值可通过[stat（查看信息）](/intl.zh-CN/常用工具/命令行工具ossutil/常用命令/stat（查看信息）.md)命令查看到，参考`X-Oss-Hash-Crc64ecma`字段和`Content-Md5`字段。
-   若文件在OSS支持CRC64功能之前上传，则无法使用stat命令查看CRC64值。
-   对于追加上传和分片上传类型的文件，无法使用stat命令查看Content-MD5值。
-   计算类型为md5时，会同时输出文件的MD5以及Content-MD5值。Content-MD5值其实是先计算5值获得128比特位数字，然后对该数字进行base64编码得到的值。
-   CRC64的计算标准，请参见[ECMA-182标准](http://www.ecma-international.org/publications/standards/Ecma-182.htm)。
-   关于Content-MD5的更多信息， 请参见[RFC1864](https://tools.ietf.org/html/rfc1864)。

## 使用示例

-   计算本地文件的CRC64

    ```
    ./ossutil hash test.txt --type=crc64
    CRC64-ECMA                  : 295992936743767023
    ```

-   计算本地文件的MD5

    ```
    ./ossutil hash test.txt --type=md5
     MD5                         : 01C3C45C03B2AF225EFAD9F911A33D73
     Content-MD5                 : AcPEXAOyryJe+tn5EaM9cw==
    ```


## 常用选项

您可以在使用hash命令时附加如下选项：

|选项名称|描述|
|----|--|
|--type|计算的类型，默认值：crc64，取值范围：crc64、md5。|
|--loglevel|设置日志级别，默认为空，表示不输出日志文件。可选值为： -   info：输出提示信息日志。
-   debug：输出详细信息日志（包括http请求和响应信息）。 |

**说明：** 更多通用选项请参见[查看选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

