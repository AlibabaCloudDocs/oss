# sign（生成签名URL）

当文件（Object）上传至存储空间（Bucket）后，您可以通过sign生成经过签名的文件URL，并将签名URL分享给第三方供其下载或预览。

## 命令格式

```
./ossutil64 sign cloud\_url
[--timeout <value>] 
[--version-id <value>] 
[--trafic-limit <value>] 
[--disable-encode-slash] 
[--payer <value>]
```

参数及选项说明如下：

|配置项|说明|
|---|--|
|cloud\_url|文件所在Bucket的完整路径。|
|--timeout|签名URL过期时间，单位为秒。默认值为60，取值范围为0~9223372036854775807。|
|--version-id|Object的指定版本ID。仅适用于已开启或暂停版本控制状态Bucket下的Object。|
|--trafic-limit|限定HTTP的访问速度，单位为bit/s。缺省值为0，表示不受限制。取值范围为819200~838860800，即100 KB/s~100 MB/s。|
|--disable-encode-slash|不对cloud\_url中携带的正斜线（/）进行编码。|
|--payer|请求的支付方式。如果希望访问指定路径下的资源产生的流量、请求次数等费用由请求者支付，请将此选项的值设置为requester。|

## 使用示例

-   为目标存储空间examplebucket下的文件exampleobject.png生成文件URL，其默认超时时间为60秒。

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png
    ```

-   为目标存储空间examplebucket下的文件exampleobject.png生成文件URL，并指定超时时间为3600秒。

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png --timeout 3600
    ```

-   为目标存储空间examplebucket下的文件exampleobject.png生成文件URL，指定其超时时间为7200秒，并限定其访问速度为100MB/s。

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png --timeout 7200 --trafic-limit 838860800
    ```

-   为已开启版本控制的存储空间examplebucket下的exampleobject.jpg的指定版本文件生成文件URL，并指定超时时间为1800秒。

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.jpg --timeout 1800 --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
    ```

-   以上示例操作成功后，返回结果中将包含生成文件URL所用时长、文件URL过期时间以及签名等信息，示例如下：

    ```
    https://examplebucket.ss-cn-hangzhou.aliyuncs.com/exampleobject.png?Expires=1608282224&OSSAccessKeyId=LTAI4G33piUmgRN1DXx9****&Signature=jo4%2FGykfuc1A4fvyvKRpRyymYH****
    0.368676(s) elapsed
    ```


## 通用选项

当您需要通过命令行工具ossutil管理不同地域的Bucket时，可以通过-e选项切换至指定Bucket所属的Endpoint。当您需要通过命令行工具ossutil管理多个阿里云账号下的Bucket时，可以通过-i选项切换至指定账号的AccessKey ID，并通过-k选项切换至指定账号的AccessKey Secret。

例如您需要为另一个阿里云账号下，华东2（上海）地域下存储空间testbucket下的exampletest.jpg文件生成超时时间为3600秒的文件URL，命令如下：

```
./ossutil64 sign oss://testbucket/exampletest.jpg --timeout 3600 -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

有关此命令的其他通用选项的更多信息，请参见[通用选项](/cn.zh-CN/常用工具/命令行工具ossutil/查看选项.md)。

