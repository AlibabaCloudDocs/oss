# Upload callback

This topic describes how to use upload callback.

Use the following code for upload callback:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName";
    /* Specify the IP address of the server you want to send the callback request to, such as http://oss-demo.aliyunc s.com:23450. */
    std::string ServerName = "yourServerName";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    std::shared_ptr<std::iostream> content = std::make_shared<std::stringstream>();
    *content << "Thank you for using Aliyun Object Storage Service!" ;
  
    /* Set the upload callback parameters. */
    std::string callbackBody = "bucket=${bucket}&object=${object}&etag=${etag}&size=${size}&mimeType=${mimeType}&my_var1=${x:var1}";
    ObjectCallbackBuilder builder(ServerName, callbackBody, "", ObjectCallbackBuilder::Type::URL);
    std::string value = builder.build();
    ObjectCallbackVariableBuilder varBuilder;
    varBuilder.addCallbackVariable("x:var1", "value1");
    std::string varValue = varBuilder.build();
    PutObjectRequest request(BucketName, ObjectName, content);
    request.MetaData().addHeader("x-oss-callback", value);
    request.MetaData().addHeader("x-oss-callback-var", varValue);
    auto outcome = client.PutObject(request);

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about upload callback, see the "[Upload callback](/intl.en-US/Developer Guide/Objects/Upload files/Upload callback.md)" section in OSS Developer Guide. For more information about debugging methods and troubleshooting, see [Upload callback errors and troubleshooting](). For more information about the implementation of upload callback, see the "[Callback](/intl.en-US/API Reference/Object operations/Basic operations/Callback.md)" section in API reference.

