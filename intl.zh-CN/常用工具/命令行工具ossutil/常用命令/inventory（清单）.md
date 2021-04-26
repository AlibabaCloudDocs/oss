# inventory（清单）

您可以使用清单功能获取存储空间（Bucket）中指定文件（Object）的数量、大小、存储类型、加密状态等信息。相对于GetBucket \(ListObjects\)接口，在海量Object的列举场景中，建议您优先使用清单功能。本文介绍如何通过inventory命令添加、查询、列举或删除Bucket清单规则。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/intl.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   有关Bucket清单功能的更多信息，参见[存储空间清单](/intl.zh-CN/开发指南/存储空间（Bucket）/存储空间清单.md)。

## 添加清单规则

添加清单规则步骤如下：

1.  生成RAM角色，该角色需拥有读取源Bucket所有文件和向目标Bucket写入文件的权限。配置角色的步骤请参见[创建可信实体为阿里云服务的RAM角色](/intl.zh-CN/角色管理/创建RAM角色/创建可信实体为阿里云服务的RAM角色.md)。
2.  创建本地文件，并在文件中配置XML格式的清单规则。
3.  ossutil先从本地文件中读取清单配置，然后将读取到清单配置添加到指定的Bucket。

-   命令格式

    ```
    ./ossutil64 inventory --method put oss://bucketname local\_xml\_file
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|添加清单规则的存储空间名称。|
    |local\_xml\_file|配置清单规则的本地文件名称，例如`localfile.txt`。|

-   使用示例
    1.  创建本地文件localfile.txt，并根据使用场景写入不同的清单规则。

        例如，将清单规则名称设置为inventorytest，以周为单位将清单报告导出至目标存储空间destbucket，清单内容中包含destbucket中所有文件的存储类型、最后更新时间以及分片上传状态等，并使用AES256加密算法加密清单文件。

        ```
        <?xml version="1.0" encoding="UTF-8"?>
          <InventoryConfiguration>
              <Id>inventorytest</Id>
              <IsEnabled>true</IsEnabled>
              <Filter></Filter>
              <Destination>
                  <OSSBucketDestination>
                      <Format>CSV</Format>
                      <AccountId>1746495857602745</AccountId>
                      <RoleArn>acs:ram::174649585760****:role/AliyunOSSRole</RoleArn>
                      <Bucket>acs:oss:::destbucket</Bucket>
                      <Encryption>
                          <SSE-OSS></SSE-OSS>
                      </Encryption>
                  </OSSBucketDestination>
              </Destination>
              <Schedule>
                  <Frequency>Weekly</Frequency>
              </Schedule>
              <IncludedObjectVersions>All</IncludedObjectVersions>
              <OptionalFields>
                  <Field>LastModifiedDate</Field>
                  <Field>StorageClass</Field>
                  <Field>IsMultipartUploaded</Field>
                  <Field>ETag</Field>
                  <Field>EncryptionStatus</Field>
                  <Field>Size</Field>
              </OptionalFields>
          </InventoryConfiguration>
        ```

        **说明：** 您可以为Bucket添加多条清单规则，规则名称（ID）是清单配置的唯一标识。若添加的规则ID已存在，则返回409错误。

    2.  为examplebucket添加清单规则。

        ```
        ./ossutil64 inventory --method put oss://examplebucket localfile.txt
        ```

        以下输出结果表明已成功添加清单规则。

        ```
        0.299514(s) elapsed
        ```


## 查询指定清单规则

-   命令格式

    ```
    ./ossutil64 inventory --method get oss://bucketname inventory\_id [--local\_xml\_file ]
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|查询清单配置的目标Bucket名称。|
    |inventory\_id|清单规则名称。|
    |local\_xml\_file|用于存放清单配置的本地文件名称，例如`localfile.txt`。如果未指定此参数，则清单配置将直接输出到屏幕。|

-   使用示例

    ```
    ./ossutil64 inventory --method get oss://examplebucket inventorytest localfile.txt
    ```

    以下输出结果表明已成功查询examplebucket中配置规则ID为inventorytest的清单内容，并将清单结果写入本地localfile.txt文件。

    ```
    0.212407(s) elapsed
    ```


## 查询所有清单规则

-   命令格式

    ```
    ./ossutil64 inventory --method list oss://bucketname [--local\_xml\_file ] [--marker <value>]
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|获取清单配置的目标Bucket名称。|
    |local\_xml\_file|用于存放清单配置的本地XML文件名称。如果未指定此参数，则清单配置将直接输出到屏幕。|
    |marker|清单过滤条件，只对与指定前缀匹配的Object生成清单文件。如果此项置空，表示对目标Bucket中的所有Object生成清单文件。|

-   使用示例

    ```
    ./ossutil64 inventory --method list oss://examplebucket localfile.txt dest
    ```

    以下输出结果表明已成功查询examplebucket下与dest前缀匹配的文件包含的所有清单规则，并将清单结果写入本地localfile.txt文件。

    ```
    0.216897(s) elapsed
    ```


## 删除指定清单规则

-   命令格式

    ```
    ./ossutil64 inventory --method delete oss://bucketname inventory\_id
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|删除清单配置的目标Bucket名称。|
    |inventory\_id|清单规则名称。|

-   使用示例

    ```
    ./ossutil64 inventory --method delete oss://examplebucket inventorytest
    ```

    以下输出结果表明已成功删除examplebucket配置规则ID为inventorytest的清单内容。

    ```
    0.212407(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的存储空间配置清单规则，命令如下：

```
./ossutil64 inventory --method put oss://examplebucket local_xml_file -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/intl.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

