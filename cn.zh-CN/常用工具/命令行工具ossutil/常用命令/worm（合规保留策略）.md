# worm（合规保留策略）

对象存储OSS支持基于时间的合规保留策略，您可以在合规保留策略的保留周期内以“不可删除、不可篡改”的方式保存和使用OSS数据。本文介绍如何使用worm命令管理Bucket的合规保留策略。

**说明：**

-   本文涉及命令均基于Linux 64位系统。如果您使用Windows 64位系统，请将如下示例中的./ossutil64替换为ossutil64.exe。
-   有关合规保留策略的更多信息，请参见[合规保留策略](/cn.zh-CN/开发指南/数据安全/合规保留策略.md)。

## 创建并锁定合规保留策略

若您希望使用合规保留策略保护您Bucket中的文件，您需要为Bucket创建一条合规保留策略，然后锁定该策略。

1.  创建合规保留策略
    -   命令格式

        ```
        ./ossutil64 worm init oss://BucketName days
        ```

        参数说明：

        |参数|说明|
        |--|--|
        |BucketName|配置合规保留策略的目标Bucket名称。|
        |days|指定合规保留策略的保留周期。在保留周期内，您无法对目标Bucket中的Object进行任何修改和删除操作。        -   单位：天
        -   取值范围：1~25550 |

    -   使用示例

        为examplebucket创建合规保留策略，并指定其保留周期为180天。

        ```
        ./ossutil64 worm init oss://examplebucket 180
        ```

        以下输出结果表明已成功创建合规保留策略。

        ```
        init success,worm id is 581D8A7FFA064C80827CAB4076A93A78
        ```

2.  锁定合规保留策略
    -   命令格式

        ```
        ./ossutil64 worm complete oss://BucketName WormId
        ```

        参数说明：

        |参数|说明|
        |--|--|
        |BucketName|指定要锁定合规保留策略的目标Bucket名称。|
        |WormId|填写创建合规保留策略时返回的策略ID。|

    -   使用示例

        锁定examplebucket的合规保留策略。

        ```
        ./ossutil64 worm complete oss://examplebucket 581D8A7FFA064C80827CAB4076A93A78
        ```

        以下输出结果表明已锁定合规保留策略。

        ```
        0.073810(s) elapsed
        ```


## 延长保留周期

合规保留策略锁定后，在保留周期未到期之前，任何人都无法修改和删除Bucket中的Object。若当前的保留周期无法满足数据保护的需求，您可以通过以下命令延长保留周期。

-   命令格式

    ```
    ./ossutil64 worm extend oss://BucketName days WormId
    ```

-   使用示例

    将examplebucket的合规保留策略的保留周期延长到360天。

    ```
    ./ossutil64 worm extend oss://examplebucket 360 581D8A7FFA064C80827CAB4076A93A78
    ```

    以下输出结果表明保留周期已延长到360天。

    ```
    0.067810(s) elapsed
    ```


## 查询合规保留策略配置

对于已创建的合规保留策略，您可以通过以下命令查询配置参数。

-   命令格式

    ```
    ./ossutil64 worm get oss://BucketName
    ```

-   使用示例

    查询examplebucket的合规保留策略。

    ```
    ./ossutil64 worm get oss://examplebucket
    ```

    以下输出结果表明已查询到合规保留策略的配置参数，结果中包含策略ID、状态、保留天数、策略创建时间。

    ```
    <WormConfiguration>
          <WormId>581D8A7FFA064C80827CAB4076A93A78</WormId>
          <State>Locked</State>
          <RetentionPeriodInDays>360</RetentionPeriodInDays>
          <CreationDate>2021-01-19T03:36:53.000Z</CreationDate>
      </WormConfiguration>
    ```


## 删除合规保留策略

在合规保留策略未锁定前，您可以删除该策略。

-   命令格式

    ```
    ./ossutil64 worm abort oss://BucketName
    ```

-   使用示例

    删除为examplebucket配置的合规保留策略。

    ```
    ./ossutil64 worm abort oss://examplebucket
    ```

    以下输出结果表明策略已删除。

    ```
    0.067810(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东1（杭州）名为test的Bucket创建合规保留策略，命令如下：

```
./ossutil64 worm init oss://test -e oss-cn-hangzhou.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

