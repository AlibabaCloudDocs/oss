# RAM和STS介绍 {#concept_nsb_brz_5db .concept}

[RAM](https://help.aliyun.com/product/28625.html?spm=a2c4g.11186623.2.9.e3504c07WriuuS) （Resource Access Management）和 [STS](https://help.aliyun.com/document_detail/28756.html?spm=a2c4g.11186623.2.7.a32ca4bbGdyt2m#reference-ong-5nv-xdb)（Security Token Service）是阿里云提供的权限管理系统。

RAM主要的作用是控制账号系统的权限。您可以使用RAM在主账号的权限范围内创建子用户，给不同的子用户分配不同的权限从而达到授权管理的目的。STS是一个安全凭证（Token）的管理系统。您可以使用STS来完成对于临时用户的访问授权。

## 为什么要使用RAM和STS {#section_htk_hql_jgb .section}

RAM和STS需要解决的一个核心问题是如何在不暴露主账号的AccessKey的情况下安全的授权别人访问。因为一旦主账号的AccessKey暴露出去的话会带来极大的安全风险，别人可以随意操作该账号下所有的资源，盗取重要信息等。

RAM提供一种长期有效的权限控制机制，通过分出不同权限的子账号，将不同的权限分给不同的用户，这样一旦子账号泄露也不会造成全局的信息泄露。但是，由于子账号在一般情况下是长期有效的，因此，子账号的AccessKey也是不能泄露的。

相对于RAM提供的长效控制机制，STS提供的是一种临时访问授权。通过STS可以返回临时的AccessKey和Token，这些信息可以直接发给临时用户用来访问OSS。一般来说，从STS获取的权限会受到更加严格的限制，并且拥有时间限制，因此这些信息泄露之后对于系统的影响也很小。

## 基本概念 {#section_itk_hql_jgb .section}

以下是一些基本概念的简单解释：

-   子账号（RAM account）：从阿里云的主账号中创建出来的子账号，在创建的时候可以分配独立的密码和权限，每个子账号拥有自己AccessKey，可以和阿里云主账号一样正常的完成有权限的操作。一般来说，这里的子账号可以理解为具有某种权限的用户，可以被认为是一个具有某些权限的操作发起者。
-   角色（Role）：表示某种操作权限的虚拟概念，但是没有独立的登录密码和AccessKey。

    **说明：** 子账号可以扮演角色，扮演角色时候的权限是该角色自身的权限。

-   授权策略（Policy）：用来定义权限的规则，比如允许用户读取或写入某些资源。
-   资源（Resource）：代表用户可访问的云资源，比如OSS所有的Bucket、OSS的某个Bucket，或OSS的某个Bucket下面的某个Object等。

子账号和角色可以类比为某个个人和其身份的关系，某人在公司的角色是员工，在家里的角色是父亲，在不同的场景扮演不同的角色，但是还是同一个人。在扮演不同的角色的时候也就拥有对应角色的权限。单独的员工或者父亲概念并不能作为一个操作的实体，只有有人扮演了之后才是一个完整的概念。这里还可以体现一个重要的概念，那就是角色可以被多个不同的个人同时扮演。

**说明：** 完成角色扮演之后，该个人就自动拥有该角色的所有权限。

这里再用一个例子解释一下。

-   某个阿里云用户，名为Alice，其在OSS下有两个私有的Bucket，alice\_a和alice\_b。alice对这两个Bucket都拥有完全的权限。

-   为了避免阿里云账号的AccessKey泄露导致安全风险，Alice使用RAM创建了两个子账号bob和carol，bob对alice\_a拥有读写权限，carol对alice\_b拥有读写权限。bob和carol都拥有独立的AccessKey，这样万一泄露了也只会影响其中一个Bucket，而且Alice可以很方便的在控制台取消泄露用户的权限。

-   现在因为某些原因，需要授权给别人读取alice\_a中的Object，这种情况下不应该直接把bob的AccessKey透露出去，那么，这个时候可以新建一个角色，比如AliceAReader，给这个角色赋予读取alice\_a的权限。但是请注意，这个时候AliceAReader还是没法直接用的，因为并不存在对应AliceAReader的AccessKey，AliceAReader现在仅仅表示一个拥有访问alice\_a权限的一个虚拟实体。

-   为了能获取临时授权，这个时候可以调用STS的AssumeRole接口，告诉STS，bob将要扮演AliceAReader这个角色，如果成功，STS会返回一个临时的AccessKeyId、AccessKeySecret还有SecurityToken作为访问凭证。将这个凭证发给需要访问的临时用户就可以获得访问alice\_a的临时权限了。凭证过期的时间在调用AssumeRole的时候指定。


