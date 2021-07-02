# lifecycle（生命周期）

配置生命周期规则后，OSS会定期将对象（Object）转储为低频访问、归档存储或冷归档存储类型，或将过期的Object和碎片删除，从而节省存储费用。本文介绍如何通过lifecycle命令添加、修改、查询、删除生命周期规则配置。

**说明：**

-   本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。
-   有关生命周期规则的更多信息，请参见[生命周期规则介绍](/cn.zh-CN/开发指南/存储空间（Bucket）/生命周期/生命周期规则介绍.md)。

## 添加或修改生命周期规则

添加或修改生命周期规则步骤如下：

1.  创建本地文件，并在文件中配置XML格式的生命周期规则。
2.  ossutil先从本地文件中读取生命周期配置，然后将读取到生命周期配置添加到指定的Bucket。

-   命令格式

    ```
    ./ossutil64 lifecycle --method put oss://bucketname local\_xml\_file
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|添加或修改生命周期规则的存储空间名称。|
    |local\_xml\_file|配置生命周期规则的本地文件名称，例如`localfile.txt`。|

-   使用示例

    **说明：** 您可以为Bucket添加多条生命周期规则，规则名称（ID）是生命周期配置的唯一标识。若添加的规则ID已存在，则返回409错误。

    1.  在本地创建名为`localfile.txt`文件，并根据使用场景写入不同的生命周期规则。

        常见的生命周期规则如下：

        -   示例1

            指定生命周期规则应用于目标存储空间examplebucket内的所有Object（即Prefix为空），在距其最后修改时间超过365天后全部删除；此外还指定了Prefix为test/，指示与前缀test/匹配的Object距其最后修改时间超过30天后转换为Archive存储类型。

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule1</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Expiration>
                  <Days>365</Days>
                </Expiration>
              </Rule>
              <Rule>
                <ID>test-rule2</ID>
                <Prefix>test/</Prefix>
                <Status>Enabled</Status>
                <Transition>
                  <Days>30</Days>
                  <StorageClass>Archive</StorageClass>
                </Transition>
              </Rule>
            </LifecycleConfiguration>
            ```

        -   示例2

            指定生命周期规则应用于目标存储空间examplebucket内的所有Object（即Prefix为空），在其最后修改时间早于2019年12月30日的Object过期。

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule0</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Expiration>
                  <CreatedBeforeDate>2019-12-30T00:00:00.000Z</CreatedBeforeDate>
                </Expiration>
              </Rule>
            </LifecycleConfiguration>
            ```

        -   示例3

            在版本控制状态下，指定目标存储空间examplebucket内的所有Object距其最后修改时间超过10天后转换为IA存储类型，Object成为非当前版本60天后转换为Archive存储类型，Object成为非当前版本90天后删除。

            ```
            <?xml version="1.0" encoding="UTF-8"?>
            <LifecycleConfiguration>
              <Rule>
                <ID>test-rule3</ID>
                <Prefix></Prefix>
                <Status>Enabled</Status>
                <Transition>
                  <Days>10</Days>
                  <StorageClass>IA</StorageClass>
                </Transition>
                <NoncurrentVersionTransition>
                  <NoncurrentDays>60</NoncurrentDays>
                  <StorageClass>Archive</StorageClass>
                </NoncurrentVersionTransition>
                <NoncurrentVersionExpiration>
                  <NoncurrentDays>90</NoncurrentDays>
                </NoncurrentVersionExpiration>
              </Rule>
            </LifecycleConfiguration>
            ```

    2.  为examplebucket添加清单规则。

        ```
        ./ossutil64 lifecycle --method put oss://examplebucket localfile.txt
        ```

        以下输出结果表明已成功添加生命周期规则。

        ```
        0.299514(s) elapsed
        ```


## 获取生命周期规则配置

-   命令格式

    ```
    ./ossutil64 lifecycle --method get oss://bucketname [local\_xml\_file]
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|获取生命周期规则配置的目标Bucket名称。|
    |local\_xml\_file|用于存放生命周期规则配置的本地文件名称，例如`localfile.txt`。如果未指定此参数，则生命周期规则配置将直接输出到屏幕。|

-   使用示例

    获取examplebucket配置的生命周期规则。

    ```
    ./ossutil64 lifecycle --method get oss://examplebucket localfile.txt
    ```

    以下输出结果表明已成功获取生命周期规则配置，并将其写入本地localfile.txt文件。

    ```
    0.212407(s) elapsed
    ```


## 删除生命周期规则配置

-   命令格式

    ```
    ./ossutil64 lifecycle --method delete oss://bucketname
    ```

-   使用示例

    删除examplebucket的生命周期规则配置。

    ```
    ./ossutil64 lifecycle --method delete oss://examplebucket
    ```

    以下输出结果表明已删除examplebucket的生命周期规则配置。

    ```
    0.530750(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为examplebucket的Bucket配置生命周期规则，命令如下：

```
./ossutil64 lifecycle --method put oss://examplebucket localfile.txt -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

