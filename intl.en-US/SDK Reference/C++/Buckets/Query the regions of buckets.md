# Query the regions of buckets

A bucket is a container for objects stored in OSS. Every object is contained in a bucket. This topic describes how to query the region of a bucket.

The following code provides an example on how to query the region or location of a bucket:

```
#include <alibabacloud/oss/OssClient.h>
using namespace AlibabaCloud::OSS;

int main(void)
{
    /*Initialize the OSS account information.*/
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";

    /*Initialize network resources.*/
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);

    /*Query the region of the bucket.*/
    GetBucketLocationRequest request(BucketName);
    auto outcome = client.GetBucketLocation(request);

    if (outcome.isSuccess()) {    
        std::cout << "getBucketLocation success, location: " << outcome.result().Location() << std::endl;
    }
    else {
        /*Handle exceptions.*/
        std::cout << "getBucketLocation fail" <<
        ",code:" << outcome.error().Code() <<
        ",message:" << outcome.error().Message() <<
        ",requestId:" << outcome.error().RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /*Release network resources.*/
    ShutdownSdk();
    return 0;
}
```

For more information about regions, see [GetBucketLocation](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucketLocation.md) and the "region" section in [Terms](/intl.en-US/Developer Guide/Terms.md) in OSS Developer Guide.

