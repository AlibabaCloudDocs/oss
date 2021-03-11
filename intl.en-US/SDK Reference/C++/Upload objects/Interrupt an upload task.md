# Interrupt an upload task

You can interrupt an upload task at time when the object is being uploaded.

Use the following code to interrupt an upload task:

```
#include <alibabacloud/oss/OssClient.h>
#include <fstream>
using namespace AlibabaCloud::OSS;

int main(void)
{
     /* Initialize the OSS account information. */
    std::string AccessKeyId = "yourAccessKeyId";
    std::string AccessKeySecret = "yourAccessKeySecret";
    std::string Endpoint = "yourEndpoint";
    std::string BucketName = "yourBucketName";
    std::string ObjectName = "yourObjectName ";

     /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
    std::vector<PutObjectOutcomeCallable> Callables;
    for (int i=0; i < 4; i++)  {  
        std::shared_ptr<std::iostream> content = std::make_shared<std::fstream>("yourLocalFilename", std::ios::in|std::ios::binary);
        PutObjectRequest request(BucketName, ObjectName, content);
        auto outcomeCallable = client.PutObjectCallable(request);
        Callables.emplace_back(std::move(outcomeCallable));
    }
    std::this_thread::sleep_for(std::chrono::milliseconds(1000));

    /* Interrupt the upload task. */
    client.DisableRequest();

    for (size_t i = 0; i < Callables.size(); i++) {
        auto outcome = Callables[i].get();
        if (outcome.error().Code() == "ClientError:100002" ||
        outcome.error().Code() == "ClientError:200042") {
            std::cout << "disable putobject success" << std::endl;
        }
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

**Note:** An interrupted upload and a canceled multipart upload have the following differences:

-   When you call the upload API to remotely upload an object to OSS, you can interrupt the upload task. In other words, you stopped the upload task. After you cancel the upload task, OSS no longer processes other multipart upload tasks that use the same upload ID as that of the canceled task.
-   If you interrupt the upload task, you can still call the upload ID to continue uploading the content although the current upload task is interrupted. If you cancel a multipart upload task, the upload ID is unavailable for future uploads.
-   Interrupted upload applies to all API operations. However, canceled multipart upload applies only to multipart upload.

