# Authorized access

This topic describes how to authorize temporary access to Object Storage Service \(OSS\) by using Security Token Service \(STS\) or a signed URL.

## Use STS to authorize temporary access

You can use STS to authorize temporary access to OSS. STS is a web service that provides temporary access tokens for cloud computing users. You can use STS to grant an access credential with a custom validity period and custom permissions for a third-party application or a RAM user managed by you. For more information about STS, see [What is STS?](/intl.en-US/API Reference/API Reference (STS)/What is STS?.md)

STS has the following benefits:

-   You need only to generate an access token and send the access token to a third-party application, instead of exposing your AccessKey pair to the third-party application. You can customize the access permissions and validity period of this token.
-   The access token automatically expires after the validity period. Therefore, you do not need to manually revoke the permissions of an access token.

For more information about how to access OSS by using STS, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md) in OSS Developer Guide.

Run the pip install aliyun-python-sdk-sts command to install the official STS client for Python. For the complete sample code of STS usage, visit [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/sts.py).

Make sure that you use OSS SDK for Python V2.0.6 and later. The following code provides an example on how to use STS to authorize the temporary download of an object:

```
# -*- coding: utf-8 -*-

from aliyunsdkcore import client
from aliyunsdksts.request.v20150401 import AssumeRoleRequest
import json
import oss2

# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
endpoint = 'yourEndpoint'
# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
access_key_id = 'yourAccessKeyId'
access_key_secret = 'yourAccessKeySecret'
# Specify the bucket name. 
bucket_name = 'examplebucket'
# Specify the string and the full path of the object. The full path of the object cannot contain bucket names. 
object_name = 'exampleobject.txt'
# To obtain the Alibaba Cloud Resource Name (ARN) information of the RAM role, log on to the RAM console. In the left-side navigation pane, click RAM Roles. On the RAM Roles page, search for and click the created RAM role. On the Role Details page, you can view and copy the ARN information. 
# Specify the ARN information of the RAM role. Format: `acs:ram::$accountID:role/$roleName`. $accountID specifies the ID of the Alibaba Cloud account and $roleName the name of the RAM role. 
role_arn = 'acs:ram::17464958********:role/ossststest'

# Create a RAM policy. 
# The policy specifies that GetObject operations can be performed on resources only in a bucket named examplebucket. 
policy_text = '{"Version": "1", "Statement": [{"Action": ["oss:GetObject"], "Effect": "Allow", "Resource": ["acs:oss:*:*:examplebucket/*"]}]}'

clt = client.AcsClient(access_key_id, access_key_secret, 'cn-hangzhou')
req = AssumeRoleRequest.AssumeRoleRequest()

# Set the format of the returned value to JSON. 
req.set_accept_format('json')
req.set_RoleArn(role_arn)
# Set the RoleSessionName parameter. This parameter can be used to identify the RAM user who assumes the RAM role. 
req.set_RoleSessionName('session-name')
req.set_Policy(policy_text)
body = clt.do_action_with_exception(req)

# Use the AccessKey pair of the RAM user to apply for a temporary access credential from STS. 
token = json.loads(oss2.to_unicode(body))

# Initialize the StsAuth instance based on the authentication information in the temporary access credential. 
auth = oss2.StsAuth(token['Credentials']['AccessKeyId'],
                    token['Credentials']['AccessKeySecret'],
                    token['Credentials']['SecurityToken'])

# Initialize a bucket based on the StsAuth instance. 
bucket = oss2.Bucket(auth, endpoint, bucket_name)

# Download the object from the bucket. 
read_obj = bucket.get_object(object_name)
print(read_obj.read())            
```

## Use a signed URL to authorize temporary access

You can generate a signed URL and provide the URL to a visitor for temporary access. When you generate a signed URL, you can specify the validity period of the URL to limit the period of access from visitors.

You can add signature information to a URL and provide the URL to a third-party user for authorized access. For more information, see [Generate a signed URL](/intl.en-US/API Reference/Access control/Generate a signed URL.md).

**Note:** The validity period must be set for both an STS temporary account and a signed URL. When you use an STS temporary account to generate a signed URL to perform operations such as object upload and download, the minimum validity period takes precedence. For example, you can set the validity period of your STS temporary account to 1200 seconds, and that of the signed URL to 3600 seconds. After 1200 seconds, you cannot use the signed URL generated by the STS temporary account to upload objects.

-   Use a signed URL to authorize the temporary upload of an object

    The following code provides an example on how to use a signed URL to authorize the temporary upload of an object:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    # Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    # Specify the bucket name. 
    bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')
    
    # Generate a signed URL that is used to upload the object. The valid period of the URL is 60 seconds. 
    print(bucket.sign_url('PUT', 'exampleobject.txt', 60))            
    ```

-   Use a signed URL to authorize the temporary download of an object

    The following code provides an example on how to use a signed URL to authorize the temporary download of an object:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
    # Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    # Specify the bucket name. 
    bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')
    
    # Generate a signed URL that is used to download the object. The valid period of the URL is 60 seconds. 
    print(bucket.sign_url('GET', 'exampleobject.txt', 60))            
    ```


