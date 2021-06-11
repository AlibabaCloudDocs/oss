# object-tagging \(add, modify, query, and delete object tags\)

Object Storage Service \(OSS\) allows you to use tags to classify objects. You can batch manage objects that have the same tag. For example, you can specify the validity period of the objects or convert the storage class of the objects that have the same tag. The object-tagging command is used to add, modify, query, and delete object tags.

## Usage notes

-   Binary

    Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

-   Tag synchronization

    When you perform cross-region replication \(CRR\), you can synchronize objects that have specified tags from the source bucket to the destination bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).


For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md) in OSS Developer Guide.

## Command syntax

```
./ossutil64 object-tagging oss://bucketname[/prefix][key#value]
--method <value>
[--encoding-type <value>]
[-r, --recursive]
[--payer <value>]
[--version-id <value>] 
```

The following table describes the parameters that you can configure when you run this command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket in which the objects for which you want to configure tagging are stored.|
|prefix|The resources in the bucket, such as directories and objects.|
|key|The key of the tag you want to configure. The object tagging feature uses a key-value pair to tag an object. You can add up to 10 tags to each object. The tags of the same object must have unique tag keys. The key of a tag must comply with the following conventions:-   The key of a tag is up to 128 characters in length and is case-sensitive.
-   The key of a tag can contain letters, digits, spaces, and the following special characters:

+=.\_:/ |
|value|The value of the tag you want to configure. The value of a tag must comply with the following conventions:-   The value of a tag is up to 256 characters in length and is case-sensitive.
-   The value of a tag can contain letters, digits, spaces, and the following special characters:

+=.\_:/ |
|--method|The type of the request. Valid values:-   put: The command to add tags to an object or modify the tags of an object.
-   get: The command to query the tags of an object.
-   delete: The command to delete the tags of an object. |
|--encoding-type|The method used to encode the prefix specified by the object name that follows `oss://bucket_name` in the full object path. Valid value: url. If this parameter is not specified, the prefix is not encoded.|
|-r, --recursive|If you specify this parameter in the command, ossutil configures tagging for all objects whose names contain the prefix specified by the prefix parameter. If you do not specify this parameter in the command, ossutil configures tagging only for the specified object.|
|--version-id|The ID of the version of the object for which you want to configure tagging. This parameter applies only to objects in buckets for which versioning is enabled or suspended.|
|--payer|The payer of the traffic and request fees incurred during queries. If you want that the requester who accesses the resources in the specified path to be charged for the traffic and request fees incurred when the command is run, set this parameter to requester.|

## Add or modify object tags

Only the owner of a bucket and RAM users that have the PutObjectTagging permission can add tags to objects in the bucket or modify the tags of objects in the bucket.

The following examples show how to add and modify tags of an object:

**Note:** In the following examples, if specified object does not have tags, the tags are added. If the specified object already has tags, the existing tags are replaced.

-   Configure a tag whose key is tagkey and whose value is tagvalue for an object named exampleobject.txt in a bucket named examplebucket.

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.txt tagkey#tagvalue
    ```

-   Configure the following two tags for an object named exampleobject.png in a bucket named examplebucket: tagkey1\#tagvalue1 and tagkey2\#tagvalue2.

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.png.txt tagkey1#tagvalue1 tagkey2#tagvalue2
    ```

-   Configure the following three tags for objects whose names contain the "test" prefix in a bucket named examplebucket: tagkey3\#tagvalue3, tagkey4\#tagvalue4, and tagkey5\#tagvalue5.

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/test -r tagkey3#tagvalue3 tagkey4#tagvalue4 tagkey5#tagvalue5
    ```

-   Configure a tag whose key is tagkey6 and whose value is tagvalue6 for the specified version of an object named exampleobject.txt in a bucket named examplebucket.

    ```
    ./ossutil64 object-tagging --method put oss://examplebucket/exampleobject.txt tagkey6#tagvalue6 --version-id CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
    ```

    For more information about how to query all versions of an object, see [ls \(list buckets, objects, or parts\)](/intl.en-US/Tools/ossutil/Common commands/ls (list buckets, objects, or parts).md).

-   After the preceding sample commands are successful, the following similar output is returned to indicate the time used to configure tagging:

    ```
    0.106852(s) elapsed
    ```


## Queries object tags

Only the owner of a bucket and RAM users that have the GetObjectTagging permission can query the tags of objects in the bucket.

The following examples show how to query the tags of objects:

-   Query the tags of an object

    Query the tags of an object named exampleobject.txt in a bucket named examplebucket.

    ```
    ./ossutil64 object-tagging --method get oss://examplebucket/exampleobject.txt
    ```

    The following output result shows that exampleobject.txt has a tag whose key is tagkey and whose value is tagvalue.

    ```
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "tagkey"  "tagvalue"      oss://examplebucket/exampleobject.txt
    
    0.068156(s) elapsed
    ```

-   Queries the tags of multiple objects

    Query the tags of all objects whose names contain the "test" prefix in a bucket named examplebucket.

    ```
    ./ossutil64 object-tagging --method get oss://examplebucket/test -r
    ```

    The following output result shows that objects whose names contain the "test" prefix have the following three tags: tagkey3\#tagvalue3, tagkey4\#tagvalue4, and tagkey5\#tagvalue5.

    ```
    object index   tag index      tag key   tag value       object
    ---------------------------------------------------------------------------
    1              0              "tagkey3" "tagvalue3"     oss://examplebucket/test
    1              1              "tagkey4" "tagvalue4"     oss://examplebucket/test
    1              2              "tagkey5" "tagvalue5"     oss://examplebucket/test
    
    0.093040(s) elapsed
    ```


## Remove object tags

Only the owner of a bucket and RAM users that have the DeleteObjectTagging permission can remove the tags of objects in the bucket.

The following examples show how to remove the tags of objects:

-   Remove the tags of an object

    You can run the following command to remove the tags of an object named exampleobject.txt in a bucket named examplebucket:

    ```
    ./ossutil64 object-tagging --method delete oss://examplebucket/exampleobject.txt
    ```

-   Remove the tags of multiple objects

    You can run the following command to remove the tags of all objects whose names contain the "test" prefix in a bucket named examplebucket:

    ```
    ./ossutil64 object-tagging --method delete oss://examplebucket/test -r
    ```

-   If the preceding commands are successful, an output similar to the following is returned to indicate the time used to remove tags:

    ```
    0.148970(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to configure a tag whose key is tagkey7 and whose value is tagvalue7 for an object named example.png in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account:

```
./ossutil64 object-tagging --method put oss://testbucket/exampletest.png tagkey7#tagvalue7 -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information the mb command, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

