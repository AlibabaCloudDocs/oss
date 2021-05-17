# 使用STS临时访问凭证访问OSS

您可以通过STS服务给其他用户颁发一个临时访问凭证。该用户可使用临时访问凭证在规定时间内访问您的OSS资源。临时访问凭证无需透露您的长期密钥，使您的OSS资源访问更加安全。

## 适用场景

假设您是一个移动App开发者，希望使用阿里云OSS服务来保存App的终端用户数据，并且要保证每个App用户之间的数据隔离。此时，您可以使用STS授权用户直接访问OSS。

使用STS授权用户直接访问OSS的流程如下：

![sts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8213221261/p273744.jpg)

1.  App用户登录。App用户和云账号无关，它是App的终端用户，App服务器支持App用户登录。对于每个有效的App用户来说，需要App服务器能定义出每个App用户的最小访问权限。
2.  App服务器请求STS服务获取一个安全令牌（SecurityToken）。在调用STS之前，App服务器需要确定App用户的最小访问权限（用RAM Policy来自定义授权策略）以及凭证的过期时间。然后通过扮演角色（AssumeRole）来获取一个代表角色身份的安全令牌（SecurityToken）。
3.  STS返回给App服务器一个临时访问凭证，包括一个安全令牌（SecurityToken）、临时访问密钥（AccessKeyId和AccessKeySecret）以及过期时间。
4.  App服务器将临时访问凭证返回给App客户端，App客户端可以缓存这个凭证。当凭证失效时，App客户端需要向App服务器申请新的临时访问凭证。例如，临时访问凭证有效期为1小时，那么App客户端可以每30分钟向App服务器请求更新临时访问凭证。
5.  App客户端使用本地缓存的临时访问凭证去请求OSS API。OSS收到访问请求后，会通过STS服务来验证访问凭证，正确响应用户请求。

## 步骤一：创建RAM用户

1.  登录[RAM控制台](https://ram.console.aliyun.com/)。
2.  在左侧导航栏的**人员管理**菜单下，单击**用户**。
3.  单击**新建用户**。
4.  输入**登录名称**和**显示名称**。
5.  在**访问方式**区域下，选择**编程访问**，然后单击**确定**。
6.  单击**复制**，保存访问密钥（AccessKey ID 和 AccessKey Secret）。

## 步骤二：为RAM用户授予请求AssumeRole的权限

1.  单击已创建RAM用户右侧对应的**添加权限**。
2.  在添加权限页面，选择**AliyunSTSAssumeRoleAccess**权限。

    ![policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4647559951/p35411.jpg)

3.  单击**确定**。

## 步骤三：创建用于获取临时访问凭证的角色

1.  在左侧导航栏，单击**RAM角色管理**。
2.  单击**新建RAM角色**，选择可信实体类型为**阿里云账号**，单击**下一步**。
3.  在新建RAM角色页面，**RAM角色名称**填写为RamOssTest，**选择云账号**为**当前云账号**。
4.  单击**完成**。
5.  单击**复制**，保存角色的ARN。

    ![arn](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5003790261/p273738.jpg)


## 步骤四：为角色授予上传文件的权限

1.  在左侧导航栏的**权限管理**菜单下，单击**权限策略管理**。
2.  单击**创建权限策略**。
3.  在新建自定义权限策略页面，填写**策略名称**，配置模式选择**脚本配置**，并在**策略内容**中赋予角色向目标存储空间examplebucket下的目录exampledir上传文件的权限。

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
                 "acs:oss:*:*:examplebucket/exampledir",
                 "acs:oss:*:*:examplebucket/exampledir/*"
               ]
         }
        ]
    }
    ```

    **警告：** 以上仅为参考示例，您需要根据实际需求配置更细粒度的授权策略，防止出现权限过大的风险。关于更细粒度的授权策略配置详情，请参见[通过RAM或STS服务向其他用户授权](/intl.zh-CN/开发指南/数据安全/访问控制/RAM Policy/RAM Policy常见示例.md)。

4.  单击**确定**。

## 步骤五：获取临时访问凭证

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
        // STS接入地址，例如sts.cn-hangzhou.aliyuncs.com。       
        String endpoint = "<sts-endpoint>";
        // 填写步骤1生成的访问密钥AccessKey ID和AccessKey Secret。
        String AccessKeyId = "<access-key-id>";
        String accessKeySecret = "<access-key-secret>";
        // 填写步骤3获取的角色ARN。
        String roleArn = "<role-arn>";
        // 标识临时访问凭证的名称.
        String roleSessionName = "<session-name>";
        // 以下Policy用于限制仅允许使用临时访问凭证向目标存储空间examplebucket上传文件。
        // 临时访问凭证最后获得的权限是步骤4设置的角色权限和该Policy设置权限的交集，即仅允许将文件上传至目标存储空间examplebucket下的exampledir目录。
        String policy = "{\n" +
                "    \"Version\": \"1\", \n" +
                "    \"Statement\": [\n" +
                "        {\n" +
                "            \"Action\": [\n" +
                "                \"oss:PutObject\"\n" +
                "            ], \n" +
                "            \"Resource\": [\n" +
                "                \"acs:oss:*:*:examplebucket/*\" \n" +
                "            ], \n" +
                "            \"Effect\": \"Allow\"\n" +
                "        }\n" +
                "    ]\n" +
                "}";
        try {
            // 添加endpoint。
            DefaultProfile.addEndpoint("", "", "Sts", endpoint);
            // 构造default profile。
            IClientProfile profile = DefaultProfile.getProfile("", AccessKeyId, accessKeySecret);
            // 构造client。
            DefaultAcsClient client = new DefaultAcsClient(profile);
            final AssumeRoleRequest request = new AssumeRoleRequest();
            request.setMethod(MethodType.POST);
            request.setRoleArn(roleArn);
            request.setRoleSessionName(roleSessionName);
            request.setPolicy(policy); // 如果policy为空，则用户将获得该角色下所有权限。
            request.setDurationSeconds(3600L); // 设置临时访问凭证的有效时间为3600秒。
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

**说明：** 临时访问凭证有效时间单位为秒，最小值为900，最大值以当前角色设定的最大会话时间为准。详情请参见[设置角色最大会话时间](/intl.zh-CN/角色管理/设置角色最大会话时间.md)。

## 步骤六：使用临时访问凭证上传文件至OSS

以Java SDK为例，将本地`D:\\localpath`路径下的exampletest.txt文件上传至存储空间examplebucket下的exampledir目录的示例代码如下：

```
import com.aliyun.oss.OSSClient;
import com.aliyun.oss.model.PutObjectRequest;

import java.io.File;

public class upload {
    public static void main(String[] args) {

// 填写步骤五获取的临时访问密钥（AccessKey ID和AccessKey Secret）。
String AccessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
// 填写步骤五获取的安全令牌SecurityToken。
String securityToken = "<yourSecurityToken>";

// 创建OSSClient实例。
OSSClient ossClient = new OSSClient(endpoint, AccessKeyId, accessKeySecret, securityToken);
// 将本地文件exampletest.txt上传至目标存储空间examplebucket下的目录exampledir。
PutObjectRequest putObjectRequest = new PutObjectRequest("examplebucket", "exampledir/exampletest.txt", new File("D:\\localpath\\exampletest.txt"));

// 如果需要上传时设置存储类型与访问权限，请参考以下示例代码。
// ObjectMetadata metadata = new ObjectMetadata();
// metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
// metadata.setObjectAcl(CannedAccessControlList.Private);
// putObjectRequest.setMetadata(metadata);

// 上传文件。
ossClient.putObject(putObjectRequest);

// 关闭OSSClient。
ossClient.shutdown();
   }
}                    
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

-   报错The security token you provided is invalid.如何处理？

    请确保完整填写[步骤五](#section_5xa_zdn_s0q)获取到的SecurityToken。

-   报错The OSS Access Key Id you provided does not exist in our records.如何处理？

    临时访问凭证已过期，过期后自动失效。请使用临时访问密钥（AccessKeyId和AccessKeySecret）向App服务器申请新的临时访问凭证。具体操作，请参见[步骤五：获取临时访问凭证](#section_5xa_zdn_s0q)。

-   获取STS时报错NoSuchBucket如何处理？

    出现这种报错通常是STS的`endpoint`填写错误。请根据您的地域，填写正确的STS接入地址。各地域的STS接入地址请参见[接入地址](/intl.zh-CN/API参考/API 参考（STS）/接入地址.md)。


