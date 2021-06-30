# sign \(generate signed object URLs\)

After you upload an object to a bucket, you can generate a signed URL for the object and share the URL with third parties for downloads and previews. This topic describes how to run the sign command to generate a signed URL for an object.

## Command syntax

```
./ossutil64 sign cloud\_url
[--timeout <value>] 
[--version-id <value>] 
[--trafic-limit <value>] 
[--disable-encode-slash] 
[--payer <value>]
```

The following table describes the parameters that you can configure when you run this command.

|Parameter|Description|
|---------|-----------|
|cloud\_url|The full path of the bucket in which the object is stored.|
|--timeout|The validity period of the signed URL. Unit: second. Valid values: 0 to 9223372036854775807. Default value: 60.|
|--version-id|The version ID of the object for which you want to generate a signed URL. This parameter applies only to objects in buckets for which versioning is enabled or suspended.|
|--trafic-limit|The maximum access speed when you use the signed URL to access the object. Unit: bit/s. The default value of this parameter is 0, which indicates that the access speed is not limited. Valid values: 819200 to 838860800 \(100 KB/s to 100 MB/s\).|
|--disable-encode-slash|Specifies that forward slashes \(/\) contained in the value of cloud\_url are not encoded.|
|--payer|The payer of the traffic and request fees incurred when the command is run. If you want the requester who accesses the resources in the specified path to pay for the traffic and request fee incurred during queries, set this parameter to requester.|

## Examples

-   You can run the following command to generate a URL for an object named exampleobject.png in a bucket named examplebucket. The validity period of the URL is the default value, which is 60 seconds.

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png
    ```

-   You can run the following command to generate a URL for an object named exampleobject.png in a bucket named examplebucket, and set the validity period of the URL to 3,600 seconds:

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png --timeout 3600
    ```

-   You can run the following command to generate a URL for an object named exampleobject.png in a bucket named examplebucket, set the validity period of the URL to 7,200 seconds, and set the maximum access speed to 100 MB/s:

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.png --timeout 7200 --trafic-limit 838860800
    ```

-   You can run the following command to generate a URL for an object named exampleobject.jpg in a versioned bucket named examplebucket, and set the validity period of the URL to 1,800 seconds:

    ```
    ./ossutil64 sign oss://examplebucket/exampleobject.jpg --timeout 1800 --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
    ```

-   If the preceding commands are successful, an output similar to the following is returned to indicate the time used to generate the URL, the validity period of the URL, and the signature information in the URL:

    ```
    https://examplebucket.ss-cn-hangzhou.aliyuncs.com/exampleobject.png?Expires=1608282224&OSSAccessKeyId=LTAI4G33piUmgRN1DXx9****&Signature=jo4%2FGykfuc1A4fvyvKRpRyymYH****
    0.368676(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to generate a URL for an object named exampletest.jpg in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account, and set the validity period of the URL to 3,600 seconds:

```
./ossutil64 sign oss://testbucket/exampletest.jpg --timeout 3600 -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

