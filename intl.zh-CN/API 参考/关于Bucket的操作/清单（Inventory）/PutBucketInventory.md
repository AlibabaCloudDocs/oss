# PutBucketInventory

PutBucketInventory接口用于为某个存储空间（Bucket）配置清单（Inventory）规则。

## 注意事项

您可以通过清单获取Bucket中Object的各类信息，包括Object的数量、大小、存储类型、加密状态等。配置清单规则时，有如下注意事项：

-   只有Bucket的拥有者以及被授予PutBucketInventory权限的用户才能发起配置清单规则的请求。
-   配置清单规则前需生成一个RAM角色，该角色需要拥有读取源Bucket所有文件和向目标Bucket写入文件的权限。首次使用清单功能时，建议您通过OSS控制台进行配置。清单规则配置完成后，您可以获取拥有所有权限的RAM角色。有关配置清单规则中RAM角色的权限说明，请参见[存储空间清单](/intl.zh-CN/开发指南/存储空间（Bucket）/存储空间清单.md)。
-   单个Bucket最多只能配置1000条清单规则。
-   配置清单的源Bucket与存放导出的清单文件所在的目标Bucket必须位于同一个Region。

## 请求语法

```
  <InventoryConfiguration>
     <Id>report1</Id>
     <IsEnabled>true</IsEnabled>
     <Filter>
        <Prefix>filterPrefix</Prefix>
     </Filter>
     <Destination>
        <OSSBucketDestination>
           <Format>CSV</Format>
           <AccountId>1000000000000000</AccountId>
           <RoleArn>acs:ram::1000000000000000:role/AliyunOSSRole</RoleArn>
           <Bucket>acs:oss:::destination-bucket</Bucket>
           <Prefix>prefix1</Prefix>
           <Encryption>
              <SSE-KMS>
                 <KeyId>keyId</KeyId>
              </SSE-KMS>
           </Encryption>
        </OSSBucketDestination>
     </Destination>
     <Schedule>
        <Frequency>Daily</Frequency>
     </Schedule>
     <IncludedObjectVersions>All</IncludedObjectVersions>
     <OptionalFields>
        <Field>Size</Field>
        <Field>LastModifiedDate</Field>
        <Field>ETag</Field>
        <Field>StorageClass</Field>
        <Field>IsMultipartUploaded</Field>
        <Field>EncryptionStatus</Field>
     </OptionalFields>
  </InventoryConfiguration>
```

## 请求元素

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Id|字符串|是|report1|由用户指定的清单名称，清单名称在当前Bucket下必须全局唯一。|
|IsEnabled|布尔|是|true|清单功能是否启用的标识。 有效值：

-   true：启用清单功能。
-   false：不启用清单功能。 |
|Filter|容器|否|不涉及|清单筛选的前缀。指定前缀后，清单将筛选出符合前缀设置的对象。|
|Prefix|字符串|否|Pics|筛选规则的匹配前缀。 父节点：Filter |
|Destination|容器|是|不涉及|存放清单结果。|
|OSSBucketDestination|容器|是|不涉及|清单结果导出后存放的Bucket信息。 父节点：Destination |
|Format|字符串|是|CSV|导出清单文件的文件格式。 有效值：CSV

父节点：OSSBucketDestination |
|AccountId|字符串|是|100000000000000|Bucket所有者授予的账户ID。 父节点：OSSBucketDestination |
|RoleArn|字符串|是|acs:ram::100000000000000:role/AliyunOSSRole|具有读取源Bucket所有文件和向目标Bucket写入文件权限的角色名，格式为`acs:ram::uid:role/rolename`。父节点：OSSBucketDestination |
|Bucket|字符串|是|acs:oss:::bucket\_0001|存放导出的清单文件的Bucket。 父节点：OSSBucketDestination |
|Prefix|字符串|否|prefix1|清单文件的存储路径前缀。 父节点：OSSBucketDestination |
|Encryption|容器|否|不涉及|清单文件的加密方式。 有效值：

-   SSE-OSS：使用OSS完全托管密钥进行加解密。
-   SSE-KMS：使用KMS托管的默认CMK（Customer Master Key）或指定CMK进行加解密。

父节点：OSSBucketDestination

有关服务器端加密的更多信息，请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。 |
|SSE-OSS|容器|否|不涉及|保存SSE-OSS加密方式的容器。 父节点：Encryption |
|SSE-KMS|容器|否|不涉及|保存SSE-KMS加密密钥的容器。 父节点：Encryption |
|KeyId|字符串|否|keyId|KMS密钥ID。父节点：SSE-KMS |
|Schedule|容器|是|不涉及|存放清单导出周期信息的容器。|
|Frequency|字符串|是|Daily|清单文件导出的周期。 有效值：

-   Daily：按天导出清单文件。
-   Weekly：按周导出清单文件。

父节点：Schedule |
|IncludedObjectVersions|字符串|是|All|是否在清单中包含Object版本信息。 有效值：

-   All：导出Object的所有版本信息。
-   Current：导出Object的当前版本信息。 |
|OptionalFields|容器|否|不涉及|设置清单结果中包含的配置项。|
|Field|字符串|否|Size|清单结果中包含的配置项。 -   Size：Object的大小。
-   LastModifiedDate：Object的最后修改时间。
-   ETag：Object的ETag值，用于标识Object的内容。
-   StorageClass：Object的存储类型。
-   IsMultipartUploaded：是否为通过分片上传方式上传的Object。
-   EncryptionStatus：Object是否加密。 |

## 响应元素

此接口仅涉及公共响应头，例如Content-Length、Date等。更多信息，请参见[公共响应头（Common Response Headers）](/intl.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   请求示例

    ```
      PUT /?inventory&inventoryId=report1 HTTP/1.1
      Host: BucketName.oss.aliyuncs.com
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Authorization: authorization string
      Content-Length: length
    
      <?xml version="1.0" encoding="UTF-8"?>
      <InventoryConfiguration>
         <Id>report1</Id>
         <IsEnabled>true</IsEnabled>
         <Filter>
            <Prefix>Pics</Prefix>
         </Filter>
         <Destination>
            <OSSBucketDestination>
               <Format>CSV</Format>
               <AccountId>100000000000000</AccountId>
               <RoleArn>acs:ram::100000000000000:role/AliyunOSSRole</RoleArn>
               <Bucket>acs:oss:::bucket_0001</Bucket>
               <Prefix>prefix1</Prefix>
               <Encryption>
                  <SSE-KMS>
                     <KeyId>keyId</KeyId>
                  </SSE-KMS>
               </Encryption>
            </OSSBucketDestination>
         </Destination>
         <Schedule>
            <Frequency>Daily</Frequency>
         </Schedule>
         <IncludedObjectVersions>All</IncludedObjectVersions>
         <OptionalFields>
            <Field>Size</Field>
            <Field>LastModifiedDate</Field>
            <Field>ETag</Field>
            <Field>StorageClass</Field>
            <Field>IsMultipartUploaded</Field>
            <Field>EncryptionStatus</Field>
         </OptionalFields>
      </InventoryConfiguration>
    ```

-   返回示例

    ```
      HTTP/1.1 200 OK
      x-oss-request-id: 56594298207FB3044385****
      Date: Mon, 31 Oct 2016 12:00:00 GMT
      Content-Length: 0
      Server: AliyunOSS
    ```


## 错误码

|错误码|HTTP状态码|描述|
|---|-------|--|
|InvalidArgument|400|传入非法参数。|
|InventoryExceedLimit|400|超出清单配置规则的数量限制。|
|AccessDenied|403|-   发起PutBucketInventory请求时，没有传入用户验证信息。
-   用户无操作权限。 |
|InventoryAlreadyExist|409|请求的清单规则已存在。|

