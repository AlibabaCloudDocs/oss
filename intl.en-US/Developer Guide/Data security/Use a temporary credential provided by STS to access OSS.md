# Use a temporary credential provided by STS to access OSS

You can use Security Token Service \(STS\) to grant a temporary credential to allow a user to access your Object Storage Service \(OSS\) resources within the specified period. This way, you do not need to share your AccessKey pairs and risk the security of your OSS resources.

## Scenarios

A mobile app developer plans to store the user data of an app on Alibaba Cloud OSS. The data of each user must be isolated to prevent one user from obtaining the data of another user. In this case, you can specify the permission of each STS token to allow a user access to only its own data in OSS.

The following figure shows how to use STS to authorize users to access OSS.

![sts](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5234534261/p273744.jpg)

1.  The app user logs on to the app server. An app user uses its username and password to log on to the app server without using the AccessKey pair of the Alibaba Cloud account used to access OSS. The app server must be capable of defining the minimum access permissions for each valid app user.
2.  The application server sends a request to STS to obtain a security token. Before the app server sends the request to STS, the app server must determine the minimum permission required by the app user to access OSS and the validity period of the credential to obtain, in which the minimum permission is defined by the Resource Access Management \(RAM\) policy configured for OSS. Then, the app server calls AssumeRole to obtain a security token that indicates a role from STS.
3.  STS returns a temporary access credential to the app server. The credential consists of a security token and a temporary AccessKey pair that can be used to access OSS within a validity period.
4.  The app server returns the temporary access credential to the app client. The app client can cache the credential. After the credential expires, the app client must apply for a new temporary access credential from the app server. For example, if the temporary access credential is valid for one hour, the app client can send a request to the app server to update the credential every 30 minutes.
5.  The app client uses the cached temporary access credential to initiate a request to call OSS API operations. After OSS receives the request, OSS uses STS to verify the access credential in the request and responds to the request.

## Step 1: Create a RAM user

1.  Log on to the [RAM console](https://ram.console.aliyun.com/).
2.  In the left-side navigation pane, click **Users** under **Identities**.
3.  On the Users page, click **Create User**.
4.  On the Create User page, specify **Logon Name** and **Display Name**.
5.  In the **Access Mode** section, select **Programmatic Access**, and click **OK**.
6.  Click **Copy** in the Actions column that corresponds to the created RAM user to save the AccessKey pair of the RAM user.

## Step 2: Grant the RAM user the AssumeRole permission

1.  On the Users page, click **Add Permissions** in the Actions column that corresponds to the created RAM user.
2.  In the Add Permissions panel, select the **AliyunSTSAssumeRoleAccess** policy from the policy list under System Policy.

    ![policy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7495688951/p35411.jpg)

3.  Click **OK**.

## Step 3: Create a role used to obtain temporary access credentials from STS

1.  In the left-side navigation pane, click **RAM Roles**.
2.  Click **Create RAM Role**, select **Alibaba Cloud Account** for Trusted entity type, and then click **Next**.
3.  In the Create RAM Role panel, enter RamOssTest in the **RAM Role Name** text box, and then select **Current Alibaba Cloud Account** in the **Select Trusted Alibaba Cloud Account** section.
4.  Click **OK**. After the role is created, click **Close**.
5.  On the RAM Roles page, enter RamOssTest in the search box to search for the created role.
6.  Click **Copy** to save the ARN of the role.

    ![arn](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5234534261/p273738.jpg)


## Step 4: Grant the role permissions to upload objects to OSS

1.  In the left-side navigation pane, click **Policies** under **Permissions**.
2.  In the left-side navigation pane, click Policies under Permissions. On the Policies page, click **Create Policy**.
3.  On the Create Custom Policy page, enter a policy name in the **Policy Name** text box, select **Script** for Configuration Mode, and then edit the script in **Policy Document** to grant the role permissions to upload objects to a directory named exampledir in the bucket named examplebucket.

    **Warning:** The following script is for reference only. You must configure fine-grained RAM policies based on your requirements to avoid granting excessive permissions to users. For more information about how to configure fine-grained RAM policies, see [Example 8: Use RAM or STS to grant other users permissions to access OSS resources](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Common examples of RAM policies.md).

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

4.  Click **OK**.

## Step 5: Obtain a temporary access credential

You can call the [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md) operation or use [STS SDKs for various programming languages](/intl.en-US/SDK Reference/STS SDK Reference/STS SDK overview.md) to obtain a temporary access credential from STS.

The following code provides an example on how to use STS SDK for Java to obtain a temporary access credential:

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
        // Specify the endpoint of STS. Example: sts.cn-hangzhou.aliyuncs.com.        
        String endpoint = "<sts-endpoint>";
        // Specify the AccessKey pair generated in Step 1. 
        String AccessKeyId = "<yourAccessKeyId>"";
        String accessKeySecret = "<yourAccessKeySecret>";
        // Specify the role ARN copied in Step 3. 
        String roleArn = "<yourRoleArn>";
        // Specify a custom role session name to distinguish different tokens. Example: SessionTest.         
        String roleSessionName = "<yourRoleSessionName>";
        // The following policy allows users to use only a temporary access credential to upload objects to examplebucket. 
        // The permission of the temporary access credential is the intersection of the role permission configured in Step 4 and the permission specified by the RAM policy. Users can use the temporary access credential to upload objects only to the exampledir directory in examplebucket. 
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
            // Add the endpoint. 
            DefaultProfile.addEndpoint("", "", "Sts", endpoint);
            // Construct a default profile. 
            IClientProfile profile = DefaultProfile.getProfile("", AccessKeyId, accessKeySecret);
            // Use the profile to construct a client. 
            DefaultAcsClient client = new DefaultAcsClient(profile);
            final AssumeRoleRequest request = new AssumeRoleRequest();
            request.setMethod(MethodType.POST);
            request.setRoleArn(roleArn);
            request.setRoleSessionName(roleSessionName);
            request.setPolicy(policy); // If the policy is not specified, the user obtains all permissions of the role. 
            request.setDurationSeconds(3600L); // Set the validity period of the temporary access credential to 3,600 seconds. 
            final AssumeRoleResponse response = client.getAcsResponse(request);
            System.out.println("Expiration: " + response.getCredentials().getExpiration());
            System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
            System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
            System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
            System.out.println("RequestId: " + response.getRequestId());
        } catch (ClientException e) {
            System.out.println("Failed: ");
            System.out.println("Error code: " + e.getErrCode());
            System.out.println("Error message: " + e.getErrMsg());
            System.out.println("RequestId: " + e.getRequestId());
        }
    }
}
```

**Note:**

-   The minimum validity period of a temporary access credential is 900 seconds. The maximum validity period of a temporary access credential is the maximum session duration specified by the current role. For more information, see [Set the maximum session duration for a RAM role](/intl.en-US/RAM Role Management/Set the maximum session duration for a RAM role.md).
-   For naming conventions for the role session specified by `roleSessionName`, see [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md).

## Step 6: Use the temporary access credential to upload objects to OSS

The following code provides an example on how to use OSS SDK for Java to upload a file named exampletest.txt from a local path `D:\\localpath` to a directory named exampledir in a bucket named examplebucket:

```
import com.aliyun.oss.OSSClient;
import com.aliyun.oss.model.PutObjectRequest;

import java.io.File;

public class upload {
    public static void main(String[] args) {

// Specify the temporary AccessKey pair included in the access credential obtained from STS in Step 5. 
String AccessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
// Specify the security token included in the temporary access credential obtained from STS in Step 5. 
String securityToken = "<yourSecurityToken>";

// Create an OSSClient instance. 
OSSClient ossClient = new OSSClient(endpoint, AccessKeyId, accessKeySecret, securityToken);
// Upload a local file named example.txt to a directory named exampledir in a bucket named examplebucket. 
PutObjectRequest putObjectRequest = new PutObjectRequest("examplebucket", "exampledir/exampletest.txt", new File("D:\\localpath\\exampletest.txt"));

// The following code provides an example on how to specify the storage class and access control list (ACL) of the object to upload. 
// ObjectMetadata metadata = new ObjectMetadata();
// metadata.setHeader(OSSHeaders.OSS_STORAGE_CLASS, StorageClass.Standard.toString());
// metadata.setObjectAcl(CannedAccessControlList.Private);
// putObjectRequest.setMetadata(metadata);

// Upload the object. 
ossClient.putObject(putObjectRequest);

// Shut down the OSSClient instance. 
ossClient.shutdown();
   }
}                    
```

For more examples of OSS SDKs for other programming languages, see the following topics:

-   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Authorized access.md)
-   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Authorized access.md)
-   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Authorized access.md)
-   [OSS SDK for C++](/intl.en-US/SDK Reference/C++/Authorized access.md)
-   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Authorized access.md)
-   [OSS SDK for C](/intl.en-US/SDK Reference/C/Authorized access.md)
-   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Authorized access.md)
-   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Authorized access.md)
-   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Authorized access.md)
-   [OSS SDK for Android](/intl.en-US/SDK Reference/Android/Authorized access.md)
-   [OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/Authorized access.md)

## FAQ

-   What do I do if the The security token you provided is invalid. error message is returned?

    Make sure that you specify the complete security token obtained in [Step 5](#section_5xa_zdn_s0q).

-   What do I do if the The OSS Access Key Id you provided does not exist in our records. error message is returned?

    Use the temporary AccessKey pair to apply for a new temporary access credential from the app server because the current temporary access credential expired. For more information, see [Step 5](#section_5xa_zdn_s0q).

-   What do I do if the NoSuchBucket error is returned when I try to obtain an access credential from STS?

    Enter a valid STS `endpoint` based on your region. The error message is returned because the specified STS endpoint is invalid. For more information about the endpoints used to access STS in different regions, see [Endpoints](/intl.en-US/API Reference/API Reference (STS)/Endpoints.md).


