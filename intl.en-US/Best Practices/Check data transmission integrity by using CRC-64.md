# Check data transmission integrity by using CRC-64

An error may occur when data is transferred between the client and server. OSS can return the CRC-64 value of objects uploaded through any of the methods provided. The client can compare the CRC-64 value with the value calculated on the local machine to verify data integrity.

OSS calculates the CRC-64 value for newly uploaded objects and stores the result as metadata of the object. OSS then adds the `x-oss-hash-crc64ecma` header to the returned response header, which indicates its CRC-64 value. This CRC-64 value is calculated based on [Standard ECMA-182](http://www.ecma-international.org/publications/standards/Ecma-182.htm).

If the object exists in OSS before CRC-64 goes online, OSS does not calculate its CRC-64 value. Therefore, its CRC-64 value is not returned when the object is obtained.

## Operation instructions

-   The PutObject, AppendObject, PostObject, and MultipartUploadPart operations return the corresponding CRC-64 value. The client can obtain the CRC-64 value returned by the server after the upload is complete and can check it against the value calculated on the local machine.
-   When the MultipartComplete operation is called, the CRC-64 value of the entire object is returned if each part has a CRC-64 value. However, if a part is uploaded before the CRC-64 goes online, the CRC-64 value is returned.
-   The GetObject, HeadObject, and GetObjectMeta operations return the corresponding CRC-64 value \(if any\). After the GetObject operation is complete, the client can obtain the CRC-64 value returned by the server and check it against the value calculated on the local machine.

    **Note:** The GET request that includes the Range header returns the CRC-64 value of the entire object.

-   The newly generated object or part may not have the CRC-64 value after copy related operations such as CopyObject and UploadPartCopy are complete.

## Examples

The following code provides an example on how to use Python to use CRC-64 values to verify data transmission integrity:

1.  Calculate the CRC-64 value.

    ```
    import oss2
    from oss2.models import PartInfo
    import os
    import crcmod
    import random
    import string
    do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)
    def check_crc64(local_crc64, oss_crc64, msg="check crc64"):
    if local_crc64 ! = oss_crc64:
    print "{0} check crc64 failed. local:{1}, oss:{2}.".format(msg, local_crc64, oss_crc64)
    return False
    else:
    print "{0} check crc64 ok.".format(msg)
    return True
    def random_string(length):
    return ''.join(random.choice(string.lowercase) for i in range(length))
    bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    ```

2.  Verify CRC-64 values for PutObject.

    ```
    content = random_string(1024)
     key = 'normal-key'
     result = bucket.put_object(key, content)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(content))
     check_crc64(local_crc64, oss_crc64, "put object")
    ```

3.  Verify CRC-64 values for GetObject.

    ```
    result = bucket.get_object(key)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(result.resp.read()))
     check_crc64(local_crc64, oss_crc64, "get object")
    ```

4.  Verify CRC-64 values for UploadPart and MultipartComplete.

    ```
    part_info_list = []
     key = "multipart-key"
     result = bucket.init_multipart_upload(key)
     upload_id = result.upload_id
     part_1 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 1, part_1)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_1))
     # Check whether the uploaded part_1 data is complete
     check_crc64(local_crc64, oss_crc64, "upload_part object 1")
     part_info_list.append(PartInfo(1, result.etag, len(part_1)))
     part_2 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 2, part_2)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2))
     # Check whether the uploaded part_2 data is complete
     check_crc64(local_crc64, oss_crc64, "upload_part object 2")
     part_info_list.append(PartInfo(2, result.etag, len(part_2)))
     result = bucket.complete_multipart_upload(key, upload_id, part_info_list)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2, do_crc64(part_1)))
     # Check whether the final object in OSS is consistent with the local file
     check_crc64(local_crc64, oss_crc64, "complete object")
    ```


## OSS SDK support

The following table describes OSS SDKs, some of which support CRC-64 for downloads and uploads.

|SDK|Support for CRC|Example|
|:--|:--------------|:------|
|Java SDK|Yes|[CRCSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CRCSample.java)|
|Python SDK|Yes|[object\_check.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_check.py)|
|PHP SDK|No|None|
|C\# SDK|No|None|
|C SDK|Yes|[oss\_crc\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_crc_sample.c)|
|JavaScript SDK|No|None|
|Go SDK|Yes|[crc\_test.go](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/oss/crc_test.go)|
|Ruby SDK|No|None|
|iOS SDK|Yes|[OSSCrc64Tests.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSiOSTests/OSSCrc64Tests.m)|
|Android SDK|Yes|[CRC64Test.java](https://github.com/aliyun/aliyun-oss-android-sdk/blob/master/oss-android-sdk/src/androidTest/java/com/alibaba/sdk/android/CRC64Test.java)|

