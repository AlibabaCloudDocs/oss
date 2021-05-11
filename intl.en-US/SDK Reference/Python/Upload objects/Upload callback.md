# Upload callback

This topic describes how to use upload callback. You can perform upload callback when you use put\_object and put\_object\_from\_file to perform simple upload or use complete\_multipart\_upload to perform multipart upload.

**Note:** For the complete code used to implement upload callback, visit [GitHub](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_callback.py). For more information about upload callback, see [Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md) in Object Storage Service \(OSS\) Developer Guide and [Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md) in API Reference.

The following code provides an example on how to use upload callback when you upload a string to the exampleobject.txt object in the examplebucket bucket:

```
# -*- coding: utf-8 -*-
import json
import base64
import oss2

# Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
auth = oss2.Auth('yourAccessKeyId', 'yourAccessKeySecret')
# Set yourEndpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
# Specify the bucket name. 
bucket = oss2.Bucket(auth, 'yourEndpoint', 'examplebucket')

# Define the function used to encode the callback parameter in Base64. 
def encode_callback(callback_params):
    cb_str = json.dumps(callback_params).strip()
    return oss2.compat.to_string(base64.b64encode(oss2.compat.to_bytes(cb_str)))

# Set the upload callback parameter. 
callback_params = {}
# Set the address of the server to which the callback request is sent. Example: http://oss-demo.aliyuncs.com:23450. 
callback_params['callbackUrl'] = 'http://oss-demo.aliyuncs.com:23450'
# Optional. Set the value of the Host field included in the callback request header. 
#callback_params['callbackHost'] = 'yourCallbackHost'
# Set the value of the body field included in the callback request. 
callback_params['callbackBody'] = 'bucket=${bucket}&object=${object}'
# Set Content-Type for the callback request. 
callback_params['callbackBodyType'] = 'application/x-www-form-urlencoded'
encoded_callback = encode_callback(callback_params)
# Configure custom parameters for the callback request. Each custom parameter consists of a key and a value. The key must start with x:. 
callback_var_params = {'x:my_var1': 'my_val1', 'x:my_var2': 'my_val2'}
encoded_callback_var = encode_callback(callback_var_params)

# Upload the object and perform upload callback. 
params = {'callback': encoded_callback, 'callback-var': encoded_callback_var}
# Specify the string and the full path of the object. The full path of the object cannot contain bucket names. 
result = bucket.put_object('exampleobject.txt', 'a'*1024*1024, params)
```

