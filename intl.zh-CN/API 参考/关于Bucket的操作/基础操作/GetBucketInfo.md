# GetBucketInfo

GetBucketInfo接口用于查看存储空间（Bucket）的相关信息。只有Bucket的拥有者才能查看Bucket的信息。

**说明：** 该请求可以从任何一个OSS的Endpoint发起。

## 请求语法

```
GET /?bucketInfo HTTP/1.1
Host: BucketName.oss.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素

|名称|类型|描述|
|:-|:-|:-|
|BucketInfo|容器|保存Bucket信息的容器。子节点：Bucket节点

父节点：无 |
|Bucket|容器|保存Bucket信息的容器。父节点：BucketInfo节点 |
|CreationDate|时间|Bucket的创建时间。时间格式：2013-07-31T10:56:21.000Z

父节点：BucketInfo.Bucket |
|ExtranetEndpoint|字符串|Bucket的外网域名。父节点：BucketInfo.Bucket |
|IntranetEndpoint|字符串|同地域ECS访问Bucket的内网域名。父节点：BucketInfo.Bucket |
|Location|字符串|Bucket所在的地域。父节点：BucketInfo.Bucket |
|Name|字符串|Bucket名称。父节点：BucketInfo.Bucket |
|Owner|容器|存放Bucket拥有者信息的容器。父节点：BucketInfo.Bucket |
|ID|字符串|Bucket拥有者的用户ID。父节点：BucketInfo.Bucket.Owner |
|DisplayName|字符串|Bucket拥有者的名称（目前和用户ID一致）。父节点：BucketInfo.Bucket.Owner |
|AccessControlList|容器|Bucket读写权限（ACL）信息的容器。关于Bucket ACL的更多信息，请参见[设置存储空间读写权限（ACL）](/intl.zh-CN/开发指南/存储空间（Bucket）/设置存储空间读写权限（ACL）.md)。

父节点：BucketInfo.Bucket |
|Grant|枚举字符串|Bucket的ACL权限。有效值：private、public-read、public-read-write

父节点：BucketInfo.Bucket.AccessControlList |
|DataRedundancyType|枚举字符串|Bucket的数据容灾类型。有效值：LRS、ZRS

父节点：BucketInfo.Bucket |
|StorageClass|字符串|Bucket的存储类型。有效值：Standard、IA、Archive和ColdArchive

关于存储类型的更多信息，请参见[存储类型介绍](/intl.zh-CN/开发指南/存储类型/存储类型介绍.md)。 |
|Versioning|字符串|Bucket的版本控制状态。有效值：Enabled、Suspended

关于版本控制状态的更多信息，请参见[PutBucketVersioning](/intl.zh-CN/API 参考/关于Bucket的操作/版本控制（Versioning）/PutBucketVersioning.md)。

父节点：BucketInfo.Bucket |
|ServerSideEncryptionRule|容器|服务器端加密方式的容器。关于服务器端加密方式的更多信息，请参见[服务器端加密](/intl.zh-CN/开发指南/数据安全/数据加密/服务器端加密.md)。

父节点：BucketInfo.Bucket |
|ApplyServerSideEncryptionByDefault|容器|服务器端默认加密方式的容器。父节点：BucketInfo.Bucket |
|SSEAlgorithm|字符串|显示服务器端默认加密方式。有效值：KMS、AES256 |
|KMSMasterKeyID|字符串|显示当前使用的KMS密钥ID。仅当SSEAlgorithm为KMS，且指定了密钥ID时返回取值。其他情况下，返回为空。|
|CrossRegionReplication|字符串|显示Bucket的跨区域复制状态。有效值：Enabled、Disabled

父节点：BucketInfo.Bucket |
|TransferAcceleration|字符串|显示Bucket的传输加速状态。有效值：Enabled、Disabled

父节点：BucketInfo.Bucket |
|HierarchicalNamespace|字符串|显示Bucket的分层命名空间状态。有效值：Enabled

父节点：BucketInfo.Bucket |

## 示例

请求示例

```
Get /?bucketInfo HTTP/1.1
Host: oss-example.oss.aliyuncs.com  
Date: Sat, 12 Sep 2015 07:51:28 GMT
Authorization: OSS qn6qrrqxo2oawuk53otf****: BuG4rRK+zNhH1AcF51NNHD39****
                
```

返回示例

-   成功获取Bucket信息的返回示例

    ```
    HTTP/1.1 200
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 531  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <BucketInfo>
      <Bucket>
        <CreationDate>2013-07-31T10:56:21.000Z</CreationDate>
        <ExtranetEndpoint>oss-cn-hangzhou.aliyuncs.com</ExtranetEndpoint>
        <IntranetEndpoint>oss-cn-hangzhou-internal.aliyuncs.com</IntranetEndpoint>
        <Location>oss-cn-hangzhou</Location>
        <StorageClass>Standard</StorageClass>
        <TransferAcceleration>Disabled</TransferAcceleration>
        <CrossRegionReplication>Disabled</CrossRegionReplication>
        <HierarchicalNamespace\>Enabled</HierarchicalNamespace\>
        <Name>oss-example</Name>
        <Owner>
          <DisplayName>username</DisplayName>
          <ID>27183473914****</ID>
        </Owner>
        <AccessControlList>
          <Grant>private</Grant>
        </AccessControlList>  
        <Comment>test</Comment>
      </Bucket>
    </BucketInfo>
    ```

-   获取不存在的Bucket信息的返回示例

    ```
    HTTP/1.1 404 
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 308  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>NoSuchBucket</Code>
      <Message>The specified bucket does not exist.</Message>
      <RequestId>568D547F31243C673BA1****</RequestId>
      <HostId>nosuchbucket.oss.aliyuncs.com</HostId>
      <BucketName>nosuchbucket</BucketName>
    </Error>
    ```

-   获取没有权限访问的Bucket信息的返回示例

    ```
    HTTP/1.1 403
    x-oss-request-id: 534B371674E88A4D8906****
    Date: Sat, 12 Sep 2015 07:51:28 GMT
    Connection: keep-alive
    Content-Length: 209  
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>AccessDenied</Code>
      <Message>AccessDenied</Message>
      <RequestId>568D5566F2D0F89F5C0E****</RequestId>
      <HostId>test.oss.aliyuncs.com</HostId>
    </Error>
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/intl.zh-CN/SDK 示例/Java/存储空间/获取存储空间的信息.md)
-   [Python](/intl.zh-CN/SDK 示例/Python/存储空间/获取存储空间信息.md)
-   [Go](/intl.zh-CN/SDK 示例/Go/存储空间/获取存储空间的信息.md)
-   [C++](/intl.zh-CN/SDK 示例/C++/存储空间/获取存储空间的信息.md)
-   [C](/intl.zh-CN/SDK 示例/C/存储空间/获取存储空间的信息.md)
-   [Node.js](/intl.zh-CN/SDK 示例/Node.js/存储空间/获取存储空间的信息.md)
-   [Android](/intl.zh-CN/SDK 示例/Android/存储空间/获取存储空间信息.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|NoSuchBucket|404|请求的目标Bucket不存在。|
|AccessDenied|403|没有查看该Bucket信息的权限。只有Bucket的拥有者才能查看Bucket的信息。|

