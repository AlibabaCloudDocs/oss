# OSS鉴权详解

默认情况下，为保证存储在OSS中数据的安全性，OSS资源（包括Bucket和Object）默认为私有权限，只有资源拥有者或者被授权的用户允许访问。如果要授权他人访问或使用自己的OSS资源，可以通过多种权限控制策略向他人授予资源的特定权限。仅当所有的权限策略通过OSS鉴权后允许访问授权资源。

## 鉴权说明

收到用户请求时，OSS会通过身份验证、基于角色的会话策略、基于身份的策略（RAM Policy）、Bucket Policy、Object ACL、Bucket ACL等鉴权结果来判断是允许或拒绝该请求。

![process](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4838959161/p254657.jpg)

以上鉴权流程包含的权限状态说明如下：

-   Allow：允许访问请求，即比对Policy命中了Allow规则。
-   Explicit Deny：显式拒绝访问请求，即比对Policy命中了Deny规则
-   Implicit Deny：隐式拒绝访问请求，即Policy不存在、或比对Policy没有命中Allow或Deny规则。

## 鉴权流程

1.  检查身份验证是否成功

    用户请求进入OSS后，OSS会对请求携带的签名和服务端计算的签名进行比对。

    -   请求签名不匹配，则拒绝访问。
    -   请求签名匹配，则继续判断是否为基于角色的会话策略。
2.  判断是否为基于角色的会话策略

    如果判断结果是基于角色的会话策略，则OSS会对Session Policy进行权限比对。

    -   比对结果为Explicit Deny或Implicit Deny，则拒绝访问。
    -   比对结果为Allow，则继续检查RAM Policy和Bucket Policy。
    如果判断结果不是基于角色的会话策略，也会继续检查RAM Policy和Bucket Policy。

3.  分别检查RAM Policy和Bucket Policy

    RAM Policy是基于身份的策略。您可以使用RAM Policy控制用户可以访问您名下哪些资源的权限。对于用户级别的访问，需要根据请求的账号类别判断允许或拒绝访问请求。

    -   如果使用阿里云账号AccessKey访问，直接返回Implicit Deny。
    -   如果使用RAM用户AccessKey或STS的AccessKey访问，但是访问的Bucket不属于阿里云账号或者RAM角色Owner，直接返回Implicit Deny。
    -   调用RAM服务提供的鉴权接口对普通请求进行身份鉴权，OSS支持RAM服务通过账号和Bucket所属资源组进行鉴权。检查返回结果为Allow、Explicit Deny或Implicit Deny。
    Bucket Policy是基于资源的授权策略，Bucket Owner可以通过Bucket Policy为RAM用户或其他帐号授权Bucket或Bucket内资源精确的操作权限。

    -   如果未设置Bucket Policy，则直接返回Implicit Deny。
    -   如果设置了Bucket Policy，则检查Bucket Policy返回结果是Allow、Explicit Deny或者Implicit Deny。
4.  合并结果中是否存在Explicit Deny策略

    如果存在，则拒绝访问。如果不存在，则检查是否存在Allow策略。

    1.  检查RAM Policy或Bucket Policy中是否存在Allow策略。

        如果存在Allow策略，则允许访问。如果不存在Allow策略，则判断请求来源。

    2.  判断请求来源。

        如果是管控类API请求，则拒绝访问。如果为数据类API请求，则继续Object ACL或Bucket ACL的鉴权。

        管控类API请求包括Service操作（GetService \(ListBuckets\)）、Bucket相关操作（例如PutBucket、GetBucketLifecycle等）、LiveChannel相关操作（例如PutLiveChannel、DeleteLiveChannel等）。

        数据类API请求包括Object相关操作，例如PutObject、GetObject等。

5.  鉴权Object ACL和Bucket ACL

    根据[Object ACL](/cn.zh-CN/API 参考/关于Object操作/权限控制（ACL)/PutObjectACL.md)进行鉴权时，需要结合请求用户是否为Bucket Owner，以及请求类型为读请求或写请求进行判断。

    -   如果判断结果是Allow，则允许访问。
    -   如果判断结果是Deny，则拒绝访问。
    如果Object ACL为继承Bucket，则继续检查Bucket ACL。

    根据[Bucket ACL](/cn.zh-CN/API 参考/关于Bucket的操作/权限控制（ACL）/PutBucketAcl.md)进行鉴权时，需要结合请求用户是否为Bucket Owner进行判断。

    -   如果判断结果是Allow，则允许访问。
    -   如果判断结果是Deny，则拒绝访问。

