# request-payment（请求者付费）

请求者付费模式是指由请求者支付访问存储空间（Bucket）内数据时产生的费用，而Bucket拥有者仅支付存储费用。当您希望共享数据，但又不希望支付因共享数据产生的额外费用时，您可以使用request-payment命令设置请求者付费模式。

**说明：** 本文各命令行示例均基于Linux 64位系统，其他系统请将命令开头的./ossutil64替换成对应的Binary名称。详情请参见[命令行工具ossutil快速入门](/cn.zh-CN/快速入门/命令行工具ossutil快速入门.md)。

有关请求者付费模式的更多信息，请参见[请求者付费模式](/cn.zh-CN/开发指南/存储空间（Bucket）/请求者付费模式.md)。

## 设置请求者付费模式

-   命令格式

    ```
    ./ossutil64 request-payment --method put oss://bucketname paymentvalue
    ```

    参数说明如下：

    |参数|说明|
    |--|--|
    |bucketname|填写待设置请求者付费模式的目标Bucket名称。|
    |paymentvalue|第三方用户访问目标Bucket内的数据产生费用时的付费模式。取值如下：

    -   Requester：访问此Bucket内的数据产生的所有费用由请求者支付。

启用请求者付费模式后，不允许匿名访问此Bucket。请求方必须提供身份验证信息，以便OSS能够识别请求方，从而对请求方而非Bucket拥有者收取请求所产生的费用。当请求者是通过扮演阿里云RAM角色来请求数据时，该角色所属的账户将为此请求付费。

    -   BucketOwner：访问此Bucket内资源产生的所有费用由Bucket拥有者支付。 |

-   使用示例

    为目标存储空间examplebucket设置请求者付费模式。

    ```
    ./ossutil64 request-payment --method put oss://examplebucket Requester
    ```

    为目标存储空间examplebucket设置Bucket拥有者付费模式。

    ```
    ./ossutil64 request-payment --method put oss://examplebucket BucketOwner
    ```

    以下输出结果表明已成功设置请求付费模式。

    ```
    0.106852(s) elapsed
    ```


## 获取请求者付费模式

-   命令格式

    ```
    ./ossutil64 request-payment --method get oss://bucketname
    ```

    bucketname填写待获取请求者付费模式的目标Bucket。

-   使用示例

    获取目标存储空间examplebucket的请求者付费模式。

    ```
    ./ossutil64 request-payment --method get oss://examplebucket
    ```

    以下输出结果表明examplebucket已开启请求者付费模式。

    ```
    Requester
    0.072024(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东2（上海）地域名为testbucket的存储空间开启请求者付费模式，命令如下：

```
./ossutil64 request-payment --method put oss://testbucket -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA**** -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

