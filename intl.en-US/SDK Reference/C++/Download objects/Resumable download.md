# Resumable download

Large-sized objects may fail to be downloaded because the network is unstable or the program exits abnormally. Objects may still fail to be downloaded after multiple attempts. To address this problem, OSS provides resumable download.

To enable resumable download, you must split the object you want to download into multiple parts and download them separately. After you download all parts, these parts are combined into a complete object.

You can use OSSClient. ResumableDownloadObject to achieve resumable download. The following table describes the parameters contained in DownloadObjectRequest.

|Name|Description|Required|Default value|Configuration method|
|:---|:----------|:-------|:------------|:-------------------|
|bucket|The name of the bucket.|Yes|None|Constructor|
|key|The name of the object.|Yes|None|Constructor|
|filePath|The path to the local file. The file located in this path is downloaded to OSS.|No|Name of the object|Constructor|
|partSize|The size of each part. Valid values: \[100 KB, 5 GB\].|No|8 MB|setPartSize|
|threadNum|The number of parts you need to download simultaneously.|No|3|Constructor or setThreadNum|
|checkpointDir|The file to record the results of multipart download. This parameter must be configured to enable resumable upload. This file stores information about download progress. If you fail to download a part, the part download continues based on the recorded progress. After the object is downloaded to your local device, the checkpoint file is deleted.|No|The same directory as that of DownloadFile|Constructor or setCheckpointDir|

Use the following code for resumable download:

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
    std::string DownloadFilePath = "yourDownloadFilePath";
    std::string CheckpointFilePath = "yourCheckpointFilepath";
 
    /* Initialize network resources. */
    InitializeSdk();

    ClientConfiguration conf;
    OssClient client(Endpoint, AccessKeyId, AccessKeySecret, conf);
 
    /* Start resumable download. */
    DownloadObjectRequest request(BucketName, ObjectName, DownloadFilePath, CheckpointFilePath);
    auto outcome = client.ResumableDownloadObject(request);
  
    if (!outcome.isSuccess()) {
        /* Handle exceptions. */
        std::cout << "ResumableDownloadObject fail" <<
        ",code:" << outcome.error(). Code() <<
        ",message:" << outcome.error(). Message() <<
        ",requestId:" << outcome.error(). RequestId() << std::endl;
        ShutdownSdk();
        return -1;
    }

    /* Release network resources. */
    ShutdownSdk();
    return 0;
}
```

