# PutBucketLifecycle

PutBucketLifecycle接口用于设置存储空间（Bucket）的生命周期规则。生命周期规则开启后，OSS将按照配置规则指定的时间，自动转换与规则相匹配的文件（Object）的存储类型或将其删除。

## 注意事项

调用PutBucketLifecycle接口时，有如下注意事项：

-   只有Bucket的拥有者以及被授予PutBucketLifecycle权限的RAM用户才能发起配置生命周期规则的请求。
-   如果Bucket此前没有设置过生命周期规则，此操作会创建一个新的生命周期规则；如果Bucket此前设置过生命周期规则，此操作会覆写先前的规则配置。
-   PutBucketLifecycle是覆盖语义。当您需要追加生命周期规则时，请先调用GetBucketLifecycle接口获取当前生命周期规则配置，然后追加新的生命周期规则配置，最后调用PutBucketLifecycle接口更新生命周期规则配置。
-   PutBucketLifecycle操作可以对Object以及Part（以分片方式上传，但最后未提交的分片）设置过期时间。

## 请求语法

```
PUT /?lifecycle HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Authorization: SignatureValue 
Host: BucketName.oss.aliyuncs.com
<?xml version="1.0" encoding="UTF-8"?>
<LifecycleConfiguration>
  <Rule>
    <ID>RuleID</ID>
    <Prefix>Prefix</Prefix>
    <Status>Status</Status>
    <Expiration>
      <Days>Days</Days>
    </Expiration>
    <Transition>
      <Days>Days</Days>
      <StorageClass>StorageClass</StorageClass>
    </Transition>
    <AbortMultipartUpload>
      <Days>Days</Days>
    </AbortMultipartUpload>
  </Rule>
</LifecycleConfiguration>
```

## 请求头

此接口仅涉及公共请求头。更多信息，请参见[公共请求头（Common Request Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 请求元素

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|LifecycleConfiguration|容器|是|不涉及|Lifecycle配置的容器，最多可容纳1000条规则。子节点：Rule

父节点：无 |
|Rule|容器|是|不涉及|生命周期规则的容器。-   不支持Archive Bucket创建转储规则。
-   Object设置过期时间必须大于转储为IA或者Archive存储类型的时间。

子节点：ID、Prefix、Status、Expiration

父节点：LifecycleConfiguration |
|ID|字符串|否|rule1|标识规则的唯一ID。最多由255个字节组成。如没有指定，或者该值为空时，OSS会自动生成一个唯一ID。子节点：无

父节点：Rule |
|Prefix|字符串|是|tmp/|指定规则所适用的前缀（Prefix）。Prefix不可重复。-   若指定了Prefix，则表示此规则仅适用于Bucket中与Prefix匹配的Object。
-   若Prefix置空，则表示此规则适用于Bucket中的所有Object。

子节点：无

父节点：Rule |
|Status|字符串|是|Enabled|如果值为Enabled，那么OSS会定期执行该规则；如果为Disabled，那么OSS会忽略该规则。父节点：Rule

有效值：Enabled、Disabled |
|Expiration|容器|否|不涉及|指定Object生命周期规则的过期属性。 对于受版本控制的Bucket，指定的过期属性只对Object的当前版本生效。子节点：Days、CreatedBeforeDate或ExpiredObjectDeleteMarker

父节点：Rule |
|Days|正整数|Days与CreatedBeforeDate互斥|1|指定生命周期规则在距离Object最后更新多少天后生效。父节点：Expiration或AbortMultipartUpload |
|CreatedBeforeDate|字符串|CreatedBeforeDate与Days互斥|2002-10-11T00:00:00.000Z|指定一个日期，OSS会对最后更新时间早于该日期的数据执行生命周期规则。日期必须服从ISO8601的格式，且要求是UTC的零点。父节点：Expiration或者AbortMultipartUpload |
|ExpiredObjectDeleteMarker|字符串|否|true|指定是否自动移除过期删除标记。 取值：

-   true：表示自动移除过期删除标记。取值为true时，不支持指定Days或CreatedBeforeDate。
-   false：表示不会自动移除过期删除标记。取值为false时，则必须指定Days或CreatedBeforeDate。

父节点：Expiration |
|Transition|容器|否|不涉及|指定Object在有效生命周期中，OSS何时将Object转储为IA、Archive和ColdArchive存储类型 。Standard Bucket中的Standard Object可以转储为IA、Archive或ColdArchive存储类型，但转储Archive存储类型的时间必须比转储IA存储类型的时间长。例如Transition IA设置Days为30，Transition Archive设置Days必须大于30。

父节点：Rule

子节点：Days、CreatedBeforeDate和StorageClass

**说明：** Days或CreatedBeforeDate只能二选一。 |
|StorageClass|字符串|如果父节点Transition或NoncurrentVersionTransition已设置，则必选|IA|指定Object转储的存储类型。**说明：** IA Bucket中的Object可以转储为Archive或者ColdArchive存储类型，但不支持转储为Standard存储类型。

取值：IA、Archive或ColdArchive

父节点：Transition |
|AbortMultipartUpload|容器|否|不涉及|指定未完成分片上传的过期属性。子节点：Days或CreatedBeforeDate

父节点：Rule |
|Tag|容器|否|不涉及|指定规则所适用的对象标签，可设置多个。 父节点：Rule

子节点：Key， Value |
|Key|字符串|若父节点Tag已设置，则必选|TagKey1|Tag Key 父节点：Tag |
|Value|字符串|若父节点Tag已设置，则必选|TagValue1|Tag Value 父节点：Tag |
|NoncurrentDays|字符串|若父节点NoncurrentVersionTransition或NoncurrentVersionExpiration已设置，则必选|5|指定生命周期规则在Object成为非当前版本多少天后生效。 父节点：NoncurrentVersionTransition、NoncurrentVersionExpiration |
|NoncurrentVersionTransition|容器|否|不涉及|在有效的生命周期规则中，OSS何时将指定Object的非当前版本转储为IA或者Archive存储类型 。 Standard类型的Object转储为Archive类型的时间必须大于转储为IA类型的时间。

子节点：NoncurrentDays、StorageClass |
|NoncurrentVersionExpiration|容器|否|不涉及|指定Object非当前版本生命周期规则的过期属性。 子节点：NoncurrentDays |

## 响应头

此接口仅涉及公共响应头。更多信息，请参见[公共响应头（Common Response Headers）](/cn.zh-CN/API 参考/公共HTTP头定义.md)。

## 示例

-   不受版本控制的生命周期配置请求示例

    ```
    PUT /?lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 443
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otf****:PYbzsdWAIWAlMW8luk****
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>delete objects and parts after one day</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>1</Days>
        </Expiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit objects to IA after 30, to Archive 60, expire after 10 years</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <Transition>
          <Days>60</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
        <Expiration>
          <Days>3600</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>transit objects to Archive after 60 days</ID>
        <Prefix>important/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>6</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
      <Rule>
        <ID>delete created before date</ID>
        <Prefix>backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </Expiration>
        <AbortMultipartUpload>
          <CreatedBeforeDate>2017-01-01T00:00:00.000Z</CreatedBeforeDate>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>r1</ID>
        <Prefix>rule1</Prefix>
        <Tag><Key>xx</Key><Value>1</Value></Tag>
        <Tag><Key>yy</Key><Value>2</Value></Tag>
        <Status>Enabled</Status>
        <Expiration>
          <Days>30</Days>
        </Expiration>
      </Rule>
      <Rule>
        <ID>r2</ID>
        <Prefix>rule2</Prefix>
        <Tag><Key>xx</Key><Value>1</Value></Tag>
        <Status>Enabled</Status>
        <Transition>
          <Days>60</Days>
          <StorageClass>Archive</StorageClass>
        </Transition>
      </Rule>
    </LifecycleConfiguration>            
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674A4D890****
    Date: Thu , 8 Jun 2017 13:08:38 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```

-   受版本控制的生命周期配置请求示例

    ```
    PUT /?lifecycle HTTP/1.1
    Host: oss-example.oss.aliyuncs.com
    Content-Length: 336
    Date: Mon , 6 May 2019 15:23:20 GMT
    Authorization: OSSWnjl3fg9fdv8fg4b****:Phuu8bBhS8dsff2a****
    <?xml version="1.0" encoding="UTF-8"?>
    <LifecycleConfiguration>
      <Rule>
        <ID>delete example</ID>
        <Prefix>logs/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <ExpiredObjectDeleteMarker>true</ExpiredObjectDeleteMarker>     
        </Expiration>
        <NoncurrentVersionExpiration>
          <NoncurrentDays>5</NoncurrentDays>
        </NoncurrentVersionExpiration>
        <AbortMultipartUpload>
          <Days>1</Days>
        </AbortMultipartUpload>
      </Rule>
      <Rule>
        <ID>transit example</ID>
        <Prefix>data/</Prefix>
        <Status>Enabled</Status>
        <Transition>
          <Days>30</Days>
          <StorageClass>IA</StorageClass>
        </Transition>
        <NoncurrentVersionTransition>
          <NoncurrentDays>10</NoncurrentDays>
          <StorageClass>IA</StorageClass>
        </NoncurrentVersionTransition>
      </Rule>
    </LifecycleConfiguration>
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 7D3435J59A9812B*****
    Date: Mon , 6 May 2019 15:23:20 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    ```


## SDK

此接口所对应的各语言SDK如下：

-   [Java](/cn.zh-CN/SDK 示例/Java/存储空间/生命周期.md)
-   [Python](/cn.zh-CN/SDK 示例/Python/存储空间/生命周期.md)
-   [Go](/cn.zh-CN/SDK 示例/Go/存储空间/生命周期.md)
-   [C++](/cn.zh-CN/SDK 示例/C++/存储空间/生命周期.md)
-   [PHP](/cn.zh-CN/SDK 示例/PHP/存储空间/生命周期.md)
-   [C](/cn.zh-CN/SDK 示例/C/存储空间/生命周期.md)
-   [.NET](/cn.zh-CN/SDK 示例/.NET/存储空间/生命周期.md)
-   [Node.js](/cn.zh-CN/SDK 示例/Node.js/存储空间/生命周期.md)
-   [Ruby](/cn.zh-CN/SDK 示例/Ruby/存储空间/生命周期.md)

## 错误码

|错误码|HTTP状态码|描述|
|:--|:------|:-|
|AccessDenied|403|没有操作权限。仅支持拥有PutBucketLifecycle权限的用户配置生命周期规则。|
|InvalidArgument|400|-   OSS支持Standard Bucket中Standard Objects转储为IA、Archive存储类型。Standard Bucket可以针对一个Object同时配置转储IA和Archive存储类型规则，但转储Archive存储类型必须晚于转储IA存储类型的时间。
-   指定Object的过期时间必须晚于转储为IA或者Archive存储类型的时间。 |

