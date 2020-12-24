# STS临时授权访问OSS

OSS可以通过阿里云STS（Security Token Service）进行临时授权访问。通过STS，您可以为第三方应用或子用户（即用户身份由您自己管理的用户）颁发一个自定义时效和权限的访问凭证。

## 使用场景

对于您本地身份系统所管理的用户，例如您的App的用户、您的企业本地账号、第三方App的用户，将这部分用户称为联盟用户。此外，联盟用户还可以是您创建的能访问您的阿里云资源应用程序的用户。这些联盟用户可能需要直接访问OSS资源。

对于这部分联盟用户，通过阿里云STS服务为阿里云账号（或RAM用户）提供临时访问权限管理。您不需要透露云账号（或RAM用户）的长期密钥（如登录密码、AccessKey），只需要生成一个临时访问凭证给联盟用户使用即可。这个凭证的访问权限及有效期限都可以由您自定义。您不需要关心权限撤销问题，临时访问凭证过期后会自动失效。

通过STS生成的临时访问凭证包括安全令牌 （SecurityToken）、临时访问密钥STS AK（AccessKeyId和AccessKeySecret）。使用AccessKey方法与您在使用阿里云账户或RAM用户AccessKey发送请求时的方法相同。需要注意的是在每个向OSS发送的请求中必须携带安全令牌。

## 实现原理

以一个移动App举例。假设您是一个移动App开发者，打算使用阿里云OSS服务来保存App的终端用户数据，并且要保证每个App用户之间的数据隔离，防止一个App用户获取到其他App用户的数据。您可以使用STS授权用户直接访问OSS。

使用STS授权用户直接访问OSS的流程如下：

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4647559951/p983.png)

1.  App用户登录。App用户和云账号无关，它是App的终端用户，App服务器支持App用户登录。对于每个有效的App用户来说，需要App服务器能定义出每个App用户的最小访问权限。
2.  App服务器请求STS服务获取一个安全令牌（SecurityToken）。在调用STS之前，App服务器需要确定App用户的最小访问权限（用RAM Policy来自定义授权策略）以及凭证的过期时间。然后通过扮演角色（AssumeRole）来获取一个代表角色身份的安全令牌（SecurityToken）。
3.  STS返回给App服务器一个临时访问凭证，包括一个安全令牌（SecurityToken）、临时访问密钥（AccessKeyId和AccessKeySecret）以及过期时间。
4.  App服务器将临时访问凭证返回给App客户端，App客户端可以缓存这个凭证。当凭证失效时，App客户端需要向App服务器申请新的临时访问凭证。例如，临时访问凭证有效期为1小时，那么App客户端可以每30分钟向App服务器请求更新临时访问凭证。
5.  App客户端使用本地缓存的临时访问凭证去请求OSS API。OSS收到访问请求后，会通过STS服务来验证访问凭证，正确响应用户请求。

## 操作步骤

您可以通过OSS SDK与STS SDK的结合使用，实现使用STS临时授权访问OSS。假设有一个名为ram-test的Bucket用于存储用户数据，现将通过用户子账号（RAM用户）结合STS实现OSS权限控制访问。

1.  创建子账号。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
    3.  单击**新建用户**。

        **说明：** 单击**添加用户**，可一次性创建多个RAM用户。

    4.  输入**登录名称**和**显示名称**。
    5.  在**访问方式**区域下，选择**编程访问**。
    6.  单击**确定**，然后单击**复制**保存访问密钥（AccessKey ID 和 AccessKey Secret）。
    7.  选中创建的子账号，单击**添加权限**。
    8.  在添加权限页面，为已创建的子账号添加**AliyunSTSAssumeRoleAccess**权限。

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4647559951/p35411.jpg)

        **说明：** 不要赋予子账号其他权限，因为在扮演角色的时候会自动获得被扮演角色的所有权限。

    9.  单击**确定**。
2.  创建权限策略。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
    3.  单击**新建权限策略**。
    4.  填写**策略名称**和**备注**。
    5.  配置模式选择**可视化配置**或**脚本配置**。

        以脚本配置为例，授权用户可以在ram-test/examplefolder/文件夹中上传文件，在**策略内容**中配置脚本示例如下。

        警告：

        以下仅为参考示例，您需要根据自己的应用需求来配置更细粒度的授权策略，防止出现权限过大的风险。关于如何配置细粒度的授权策略，请参见*基于RAM Policy的权限控制*中的[示例5：通过RAM或STS服务向其他用户授权](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/基于RAM Policy的权限控制.mdsection_lda_vgc_p09)。

        ```
        {
            "Version": "1",
            "Statement": [
             {
                   "Effect": "Allow",
                   "Action": [
                     "oss:PutObject"
                   ],
                   "Resource": [
                     "acs:oss:*:*:ram-test/examplefolder",
                     "acs:oss:*:*:ram-test/examplefolder/*"//限制到最后一级可访问路径。
                   ]
             }
            ]
        }
        ```

3.  创建角色并记录角色ARN。
    1.  云账号登录[RAM控制台](https://ram.console.aliyun.com/)。
    2.  在左侧导航栏，单击**RAM角色管理**。
    3.  单击**新建RAM角色**，选择可信实体类型为**阿里云账号**，单击**下一步**。
    4.  在新建RAM角色页面，填写**RAM角色名称**和**备注**，本示例RAM角色名称为RamOssTest。
    5.  **选择云账号**为**当前云账号**。
    6.  单击**完成**，之后单击**为角色授权**。
    7.  在添加权限页面，选择**自定义权限策略**，添加步骤2中创建的权限策略。

        添加权限策略后，页面如下图所示：

        ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4647559951/p35437.jpg)

    8.  记录角色的**ARN**，即需要扮演角色的ID。
4.  获取临时访问凭证STS AK与SecurityToken。

    您可以通过调用STS服务接口[AssumeRole](/intl.zh-CN/API参考/API 参考（STS）/操作接口/AssumeRole.md) 或者使用[各语言STS SDK](/intl.zh-CN/SDK参考/SDK参考（STS）/STS SDK概览.md)来获取临时访问凭证。

    以Java SDK为例：

    ```
    package com.aliyun.sts.sample;
    import com.aliyuncs.DefaultAcsClient;
    import com.aliyuncs.exceptions.ClientException;
    import com.aliyuncs.http.MethodType;
    import com.aliyuncs.profile.DefaultProfile;
    import com.aliyuncs.profile.IClientProfile;
    import com.aliyuncs.sts.model.v20150401.AssumeRoleRequest;
    import com.aliyuncs.sts.model.v20150401.AssumeRoleResponse;
    public class StsServiceSample {
        public static void main(String[] args) {        
            String endpoint = "<sts-endpoint>";
            String AccessKeyId = "<access-key-id>";
            String accessKeySecret = "<access-key-secret>";
            String roleArn = "<role-arn>";
            String roleSessionName = "<session-name>";
            String policy = "{\n" +
                    "    \"Version\": \"1\", \n" +
                    "    \"Statement\": [\n" +
                    "        {\n" +
                    "            \"Action\": [\n" +
                    "                \"oss:*\"\n" +
                    "            ], \n" +
                    "            \"Resource\": [\n" +
                    "                \"acs:oss:*:*:*\" \n" +
                    "            ], \n" +
                    "            \"Effect\": \"Allow\"\n" +
                    "        }\n" +
                    "    ]\n" +
                    "}";
            try {
                // 添加endpoint（直接使用STS endpoint，前两个参数留空，无需添加region ID）
                DefaultProfile.addEndpoint("", "", "Sts", endpoint);
                // 构造default profile（参数留空，无需添加region ID）
                IClientProfile profile = DefaultProfile.getProfile("", AccessKeyId, accessKeySecret);
                // 用profile构造client
                DefaultAcsClient client = new DefaultAcsClient(profile);
                final AssumeRoleRequest request = new AssumeRoleRequest();
                request.setMethod(MethodType.POST);
                request.setRoleArn(roleArn);
                request.setRoleSessionName(roleSessionName);
                request.setPolicy(policy); // 若policy为空，则用户将获得该角色下所有权限
                request.setDurationSeconds(1000L); // 设置凭证有效时间
                final AssumeRoleResponse response = client.getAcsResponse(request);
                System.out.println("Expiration: " + response.getCredentials().getExpiration());
                System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
                System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
                System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
                System.out.println("RequestId: " + response.getRequestId());
            } catch (ClientException e) {
                System.out.println("Failed：");
                System.out.println("Error code: " + e.getErrCode());
                System.out.println("Error message: " + e.getErrMsg());
                System.out.println("RequestId: " + e.getRequestId());
            }
        }
    }
    ```

    参数说明如下：

    -   endpoint：STS接入地址，例如sts.cn-hangzhou.aliyuncs.com。各地域的STS接入地址请参见[接入地址](/intl.zh-CN/API参考/API 参考（STS）/接入地址.md)。
    -   AccessKeyId、AccessKeySecret：步骤1中保存的访问密钥。
    -   RoleArn：步骤3中保存的角色ARN。
    -   RoleSessionName：用来标识临时访问凭证的名称，建议使用不同的应用程序用户来区分。
    -   Policy：在扮演角色的时候额外添加的权限限制。请参见[基于RAM Policy的权限控制](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/基于RAM Policy的权限控制.md)。

        **说明：** 这里传入的Policy是用来限制扮演角色之后的临时访问凭证的权限。临时访问凭证最后获得的权限是角色的权限和这里传入的Policy的交集。扮演角色的时候传入Policy的原因是为了授权的灵活性，例如上传文件的时候可以根据不同的用户添加对于上传文件路径的限制等。

    -   DurationSeconds：设置临时访问凭证的有效期，单位是秒，最小值为900，最大值以当前角色设定的最大会话时间为准。详情请参见[设置角色最大会话时间](/intl.zh-CN/角色管理/设置角色最大会话时间.md)。
5.  通过临时访问凭证访问OSS。

    以通过Java SDK上传本地文件到OSS为例，代码如下：

    ```
    // Endpoint以杭州为例，其它Region请按实际情况填写。
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
    String AccessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    
    // 用户拿到STS临时凭证后，通过其中的安全令牌（SecurityToken）和临时访问密钥（AccessKeyId和AccessKeySecret）生成OSSClient。
    // 创建OSSClient实例。注意，这里使用到了上一步生成的临时访问凭证（STS访问凭证）。
    OSSClient ossClient = new OSSClient(endpoint, AccessKeyId, accessKeySecret, securityToken);
    // 创建PutObjectRequest对象。
    PutObjectRequest putObjectRequest = new PutObjectRequest("<yourBucketName>", "<yourObjectName>", new File("<yourLocalFile>"));
    
    // 如果需要上传时设置存储类型与访问权限，请参考以下示例代码。
    // ObjectMetadata metadata = new ObjectMetadata();
    // metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
    // metadata.setObjectAcl(CannedAccessControlList.Private);
    // putObjectRequest.setMetadata(metadata);
    
    // 上传文件。
    ossClient.putObject(putObjectRequest);
    
    // 关闭OSSClient。
    ossClient.shutdown();
    ```

    更多语言的SDK示例请参见：

    -   [Java SDK](/intl.zh-CN/SDK 示例/Java/授权访问.md)
    -   [Python SDK](/intl.zh-CN/SDK 示例/Python/授权访问.md)
    -   [Go SDK](/intl.zh-CN/SDK 示例/Go/授权访问.md)
    -   [C++ SDK](/intl.zh-CN/SDK 示例/C++/授权访问.md)
    -   [PHP SDK](/intl.zh-CN/SDK 示例/PHP/授权访问.md)
    -   [C SDK](/intl.zh-CN/SDK 示例/C/授权访问.md)
    -   [.NET SDK](/intl.zh-CN/SDK 示例/.NET/授权访问.md)
    -   [Node.js SDK](/intl.zh-CN/SDK 示例/Node.js/授权访问.md)
    -   [Browser.js SDK](/intl.zh-CN/SDK 示例/Browser.js/授权访问.md)
    -   [Android SDK](/intl.zh-CN/SDK 示例/Android/授权访问.md)
    -   [iOS SDK](/intl.zh-CN/SDK 示例/iOS/授权访问.md)

## 常见问题

获取STS时出现NoSuchBucket报错如何处理？

出现这种报错一般是STS的`endpoint`填写错误。请根据您的地域，填写正确的STS接入地址。各地域的STS接入地址请参见[接入地址](/intl.zh-CN/API参考/API 参考（STS）/接入地址.md)。

