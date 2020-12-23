# Append upload

You can use AppendObject to append content to appendable objects that are uploaded.

## Usage notes

When you call AppendObject to upload an object, take note of the following items:

-   If the object to append does not exist, an append object is created.
-   If the object to append is an existing append object and the specified position from which the append operation starts is not equal to the current object size, the PositionNotEqualToLength error is returned. If the object to append is a non-append object, the ObjectNotAppendable error is returned.
-   The CopyObject operation cannot be performed on append objects.

For more information about how to perform append upload, see [AppendObject](/intl.en-US/API Reference/Object operations/Basic operations/AppendObject.md).

## Sample codes

The following code provides an example on how to perform append upload:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /* Initialize the OSS account information. */
    /* Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    /* The endpoint of the China (Hangzhou) region is used in this example. Specify the actual endpoint.
    std::string Endpoint = "yourEndpoint";
    /* Configure the bucket name. */
    std::string BucketName = "yourBucketName";
    /* Configure the object name. The object name indicates the complete path of the object excluding the bucket name. Example: example/folder/abc.jpg. */
    std::string ObjectName = "yourObjectName";

    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    auto meta = ObjectMetaData();
    meta.setContentType("text/plain");

    /* If the object is appended for the first time, the append position is 0. The position for the next append operation is included in the response. The position from which the next append operation starts is the current length of the object. */
    std::shared_ptr<std::iostream> content1 = std::make_shared<std::stringstream>();
    *content1 <<"Thank you for using Aliyun Object Storage Service!" ;
    AppendObjectRequest request(BucketName, ObjectName, content1, meta);
    request.setPosition(0L);

    /* Perform the first append operation. */
    auto result = client.AppendObject(request);

    if (! result.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "AppendObject fail" <<
        ",code:" << result.error().Code() <<
        ",message:" << result.error().Message() <<
        ",requestId:" << result.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    std::shared_ptr<std::iostream> content2 = std::make_shared<std::stringstream>();
    *content2 <<"Thank you for using Aliyun Object Storage Service!" ;
    auto position = result.result().Length();
    AppendObjectRequest appendObjectRequest(BucketName, ObjectName, content2);
    appendObjectRequest.setPosition(position);

    /* Perform the second append operation. */
    auto outcome = client.AppendObject(appendObjectRequest);

    if (! outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "AppendObject fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

