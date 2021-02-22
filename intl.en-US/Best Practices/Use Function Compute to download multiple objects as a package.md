# Use Function Compute to download multiple objects as a package

This topic describes how to use Function Compute to download multiple objects as a package to a local computer.

-   Function Compute is activated.

    You can activate Function Compute on the buy page. For more information, see [Function Compute](https://www.aliyun.com/product/fc).

-   Function Compute is authorized to access OSS.

    For information about how to perform authorization, see [Permissions](https://www.alibabacloud.com/help/zh/doc-detail/52885.htm).


When you download multiple objects from OSS at a time, the download speed may be affected if the sizes of the objects are small and the number of the objects is large. To resolve this problem, use Function Compute to compress the objects into a package. Then, download the package to your local device and decompress the package. The following flowchart shows the process of how to use Function Compute to compress the objects into a package and download objects.

![Decompress objects](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1554449951/p93634.png)

Process details:

1.  A user calls a function and specifies a bucket and the objects to compress.
2.  Function Compute obtains the specified objects from OSS and randomly generates a name for the ZIP package.
3.  Function Compute uploads the ZIP package to OSS.
4.  Function Compute returns the URL to download the ZIP package to the user.
5.  The user uses the returned URL to download the ZIP package from OSS.

**Note:**

-   The disk space used to run the function is limited. Streaming download and streaming upload are used to transfer the ZIP package. Only a small amount of data is cached in the buffer.
-   To accelerate the transfer of the ZIP package, Function Compute uploads the ZIP package to OSS while Function Compute generates the ZIP package.
-   When the ZIP package is uploaded to OSS, multipart upload is used to upload parts. Multi-threading is supported.
-   It takes a maximum of 10 minutes for Function Compute to compress objects. Test data: It takes about 63s to upload 57 objects whose total size is 1.06 GB.

## Procedure

Linux is used in this example. To use Windows and macOS, modify corresponding commands.

1.  Install FunCraft.

    For more information about how to install FunCraft, visit [Install FunCraft](https://github.com/alibaba/funcraft/blob/master/docs/usage/installation-zh.md).

2.  Download the code package used to run the function.

    ```
    wget https://fc-demo-public.oss-cn-hangzhou.aliyuncs.com/demo-source-code/zip-oss-code.zip
    ```

3.  Decompress the code package.

    ```
    unzip zip-oss-code.zip
    ```

4.  Modify the event.json file. Specify the location of the objects to compress.

    ```
    vi event.json
    {
       "region": "regionid",
       "bucket": "bucketname",
       "source-dir": "path/"
     }
    ```

    Parameters:

    -   region: Enter the ID of region where your bucket is located. Example: `cn-hangzhou`.
    -   bucket: Enter the name of the bucket.
    -   source-dir: Enter the folder of the objects to compress. Example: `. /`. We recommend that you store all objects to compress in a folder.
5.  Enter fun deploy to deploy the function. Record the URL value.

    ![deploy](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1554449951/p93785.png)

6.  Use curl to trigger the function.

    Command syntax:

    ```
    curl -v -L -o /localpath -d @./event.json urlvalue
    ```

    Command used in this example:

    ```
    curl -v -L -o /test/oss.zip -d @./event.json https://174649585760****.cn-hangzhou.fc.aliyuncs.com/2016-08-15/proxy/zip-service/zip-oss/
    ```


