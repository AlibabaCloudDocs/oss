# 通过crc64校验数据传输的完整性

数据在客户端和服务器之间传输时有可能会出错。OSS现在支持对各种方式上传的Object返回其crc64值，客户端可以和本地计算的crc64值做对比，从而完成数据完整性的验证。

OSS对新上传的Object进行crc64的计算，并将结果作为Object的元信息存储，随后在返回的response header中增加`x-oss-hash-crc64ecma`头部，表示其crc64值，该64位CRC根据[ECMA-182标准](http://www.ecma-international.org/publications/standards/Ecma-182.htm)计算得出。

对于crc64上线之前就已经存在于OSS上的Object，OSS不会对其计算crc64值，所以获取此类Object时不会返回其crc64值。

## 操作说明

-   PutObject、AppendObject、PostObject、MultipartUploadPart均会返回对应的crc64值，客户端可以在上传完成后拿到服务器返回的crc64值和本地计算的数值进行校验。
-   MultipartComplete时，如果所有的Part都有crc64值，则会返回整个Object的crc64值；若某些Part没有crc64值，则不返回整个Object的crc64值。例如某个Part在crc64上线之前就已经上传，则不返回crc64值。
-   GetObject、HeadObject、GetObjectMeta都会返回对应的crc64值（如有）。客户端可以在GetObject完成后，拿到服务器返回的crc64值和本地计算的数值进行校验。

    **说明：** range get请求返回的将会是整个Object的crc64值。

-   Copy相关的操作，如CopyObject、UploadPartCopy，新生成的Object/Part不保证具有crc64值。

## 应用示例

以下为完整的Python示例代码，演示如何基于crc64值验证数据传输的完整性。

1.  计算crc64。

    ```
    import oss2
    from oss2.models import PartInfo
    import os
    import crcmod
    import random
    import string
    do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)
    def check_crc64(local_crc64, oss_crc64, msg="check crc64"):
    if local_crc64 != oss_crc64:
    print "{0} check crc64 failed. local:{1}, oss:{2}.".format(msg, local_crc64, oss_crc64)
    return False
    else:
    print "{0} check crc64 ok.".format(msg)
    return True
    def random_string(length):
    return ''.join(random.choice(string.lowercase) for i in range(length))
    bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    ```

2.  验证PutObject。

    ```
    content = random_string(1024)
     key = 'normal-key'
     result = bucket.put_object(key, content)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(content))
     check_crc64(local_crc64, oss_crc64, "put object")
    ```

3.  验证GetObject。

    ```
    result = bucket.get_object(key)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(result.resp.read()))
     check_crc64(local_crc64, oss_crc64, "get object")
    ```

4.  验证UploadPart和Complete。

    ```
    part_info_list = []
     key = "multipart-key"
     result = bucket.init_multipart_upload(key)
     upload_id = result.upload_id
     part_1 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 1, part_1)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_1))
     #check 上传的 part 1数据是否完整
     check_crc64(local_crc64, oss_crc64, "upload_part object 1")
     part_info_list.append(PartInfo(1, result.etag, len(part_1)))
     part_2 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 2, part_2)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2))
     #check 上传的 part 2数据是否完整
     check_crc64(local_crc64, oss_crc64, "upload_part object 2")
     part_info_list.append(PartInfo(2, result.etag, len(part_2)))
     result = bucket.complete_multipart_upload(key, upload_id, part_info_list)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2, do_crc64(part_1)))
     #check 最终oss上的object和本地文件是否一致
     check_crc64(local_crc64, oss_crc64, "complete object")
    ```


## OSS SDK支持

部分OSS SDK已经支持上传、下载使用crc64进行数据校验，用法见下表中的示例。

|SDK|是否支持CRC|示例|
|:--|:------|:-|
|Java SDK|是|[CRCSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CRCSample.java)|
|Python SDK|是|[object\_check.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_check.py)|
|PHP SDK|否|无|
|C\# SDK|否|无|
|C SDK|是|[oss\_crc\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_crc_sample.c)|
|JavaScript SDK|否|无|
|Go SDK|是|[crc\_test.go](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/oss/crc_test.go)|
|Ruby SDK|否|无|
|iOS SDK|是|[OSSCrc64Tests.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSiOSTests/OSSCrc64Tests.m)|
|Android SDK|是|[CRC64Test.java](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/oss-android-sdk/src/androidTest/java/com/alibaba/sdk/android/CRC64Test.java)|

