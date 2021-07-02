# Obtain request IDs

Object Storage Service \(OSS\) assigns a unique ID for each request received by OSS. You can use the ID of a request to query the information about the request in various logs. When you contact technical support to help troubleshoot errors that occurred in OSS, you must provide the IDs of the failed requests. Technical support can use these request IDs to locate and resolve your problems. This topic describes how to obtain the ID of a request.

## Obtain request IDs from real-time logs

To obtain the ID of a request, you can query the real-time logs of operations performed on buckets and objects in the OSS console.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket whose logs you want to query.

3.  Choose **Logging** \> **Real-time Log Query**.

4.  Press `Ctrl + F`, enter request\_id to search for the ID of the request.

    ![log](../images/p283829.jpg)


## Obtain request IDs by using browser developer tools

The following example shows how to use the developer tools of Google Chrome to obtain the ID of a request that you sent to upload an object:

1.  Open the developer tools of your browser.

    1.  Open your browser and press `F12`.

    2.  On the developer tools page, click **Network**.

2.  Uploads an object.

    1.  Log on to the OSS console. In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload an object.

    2.  In the left-side navigation pane, click **Files**.

    3.  Uploads an object. For more information, see [Upload objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload objects.md).

3.  Use browser developer tools to obtain the ID of the upload request.

    1.  On the developer tools page of your browser, click the Name tab, and then select the object that you uploaded in Step 2.

    2.  Click the **Headers** tab, obtain the ID of the upload request in the **Response Headers** section.

        ![log](../images/p283939.jpg)


## Obtain request IDs by using OSS SDKs

The following examples show how to use OSS SDK for Python to obtain the IDs of requests that you sent to upload and download an object:

-   The following code provides an example on how to obtain the ID of a request that is sent to upload an object:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('[$AccessKey_ID]', '[$AccessKey_Secret]')
    # Set Endpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    # Set Bucket_Name to the name of the bucket to which you want to upload the object. 
    bucket = oss2.Bucket(auth, '[$Endpoint]', '[$Bucket_Name]')
    # Set Object_Name to the name of the object that you want to upload. 
    key = '[$Object_Name]'
    # Set Object_Content to the string you want to upload to the object. 
    requestid= bucket.put_object(key, '[$Object_Content]').request_id
    # Display the ID of the request. 
    print(requestid)
    ```

-   The following code provides an example on how to obtain the ID of a request that is sent to download an object:

    ```
    # -*- coding: utf-8 -*-
    import oss2
    # Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use a RAM user to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console. 
    auth = oss2.Auth('[$AccessKey_ID]', '[$AccessKey_Secret]')
    # Set Endpoint to the endpoint of the region in which the bucket is located. For example, if your bucket is located in the China (Hangzhou) region, set yourEndpoint to https://oss-cn-hangzhou.aliyuncs.com. 
    # Set Bucket_Name to the name of the bucket from which you want to download the object. 
    bucket = oss2.Bucket(auth, '[$Endpoint]', '[$Bucket_Name]')
    # Set Object_Name to the full path of the object that you want to download. The full path of the object does not contain the bucket name. For example, if you want to download an object named example.txt in the destdir directory of a bucket named examplebucket, set Object_Name to destdir/example.txt. 
    # Set Local_File to the path of the local file as which the downloaded object is stored. For example, if you store the downloaded object as a local file named local.txt in the /tmp directory, set Local_File to /tmp/local.txt. 
    requestid = bucket.get_object_to_file('[$Object_Name]', '[$Local_File]').request_id
    # Display the ID of the request. 
    print(requestid)
    ```


