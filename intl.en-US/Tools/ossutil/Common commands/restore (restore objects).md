# restore \(restore objects\)

Before you access Archive or Cold Archive objects, you must run the restore command to restore the objects.

**Note:**

-   Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).
-   For more information about the status of Archive or Cold Archive objects during restoration and how you are charged for object restoration, see [Restore objects](/intl.en-US/Developer Guide/Objects/Manage files/Restore objects.md).

## Command syntax

```
./ossutil64 restore oss://bucketname[/prefix][local\_xml\_file]
[--encoding-type <value>]
[--payer <value>]
[--version-id <value>]
[-r, --recursive]
[-f, --force] 
[--retry-times <value>]
[-j, --job <value>]
```

The following table describes the parameters that you can configure in this command.

|Parameter|Description|
|---------|-----------|
|bucketname|The name of the bucket in which the objects you want to restore is stored.|
|prefix|The resources in the bucket, such as directories and objects.|
|local\_xml\_file|The local XML file used to store parameters that are configured to restore Cold Archive objects.|
|--encoding-type|The method used to encode the value of the prefix parameter. Valid value: url. If this parameter is not specified, the prefix parameter is not encoded.|
|--payer|The payer of the traffic and request fees incurred when the command is run. If you want the requester who accesses the resources in the specified path to pay for the traffic and request fee incurred during queries, set this parameter to requester.|
|--version-id|The ID of the version of the object you want to restore. This parameter applies only to objects in buckets for which versioning is enabled or suspended.|
|-r, --recursive|If you specify this parameter, ossutil restores all objects whose names contain the specified prefix in the bucket. If you do not specify this parameter, ossutil restores only the specified object.|
|-f, --force|Specifies the command to forcibly run without prompting the user for confirmation.|
|--retry-times|The number of retries after the command fails to be run. Default value: 10. Valid value: 1 to 500.|
|-j, --job|The number of concurrent tasks performed across multiple objects. Valid values: 1 to 10000. Default value: 3.|

## Examples

-   Restore an Archive object

    OSS takes 1 minute to restore an Archive object. An object cannot be read while it is being restored.

    By default, a restored object remains in the restored state for one day. If you run the restore command for an object that is already in the restored state, the restored state is prolonged by one day. This way, you can prolong the restored state of an object to up to seven days. After the period, the object becomes the frozen state.

    -   You can run the following command to restore an Archive object named exampleobject.txt in a bucket named examplebucket:

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.txt
        ```

    -   You can run the following command with the -r parameter specified to restore all Archive objects whose names contain the dest prefix in a bucket named examplebucket:

        ```
        ./ossutil64 restore oss://examplebucket/dest -r                         
        ```

    -   You can run the following command to restore the specified version of an object named exampleobject.txt in a bucket named examplebucket:

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.txt --version-id  CAEQARiBgID8rumR2hYiIGUyOTAyZGY2MzU5MjQ5ZjlhYzQzZjNlYTAyZDE3****
        ```

        For more information about how to query all versions of an object, see [ls \(list buckets, objects, or parts\)](/intl.en-US/Tools/ossutil/Common commands/ls (list buckets, objects, or parts).md).

-   Restore a Cold Archive object

    The following example shows how to restore a Cold Archive object named exampleobject.jpg in a bucket named examplebucket within 1 hour and keep the object in the restored state for three days:

    1.  Create a local XML file named config.xml and configure the following parameters in the file:

        ```
        <RestoreRequest>
            <Days>3</Days>
            <JobParameters>
                <Tier>Expedited</Tier>
            </JobParameters>
        </RestoreRequest>
        ```

        The following table describes the parameters you can configure.

        |Parameter|Description|
        |---------|-----------|
        |Days|The duration for which you want to keep the restored Cold Archive object in the restored state. Unit: days. Valid values: 1 to 7. |
        |Tier|The restoration priority of the Cold Archive object. Valid values:

        -   Expedited: The object is restored within 1 hour.
        -   Standard: The object is restored in 2 to 5 hours.
        -   Bulk: The object is restored in 5 to 12 hours. |

    2.  Run the following command to restore exampleobject.jpg:

        ```
        ./ossutil64 restore oss://examplebucket/exampleobject.jpg config.xml
        ```

    **Note:** The time required to restore an object vary with the object size.

-   Output

    If the preceding command is successful, an output similar to the following is returned to indicate the time used to restore the object:

    ```
    0.106852(s) elapsed
    ```


## Common options

To use ossutil to manage buckets that are located in different regions, you can use the -e option to use the endpoint of the specified bucket. To use ossutil to manage buckets that are owned by different Alibaba Cloud accounts, you can use the -i option to use the AccessKey ID of the specified account, and use the -k option to use the AccessKey secret of the specified account.

For example, you can run the following command to restore an object named exampletest.png in a bucket named testbucket, which is located in the China \(Shanghai\) region and owned by another Alibaba Cloud account:

```
./ossutil64 restore oss://testbucket/exampletest.png -e oss-cn-shanghai.aliyuncs.com -i LTAI4Fw2NbDUCV8zYUzA****  -k 67DLVBkH7EamOjy2W5RVAHUY9H****
```

For more information about other common options, see [Common options](/intl.en-US/Tools/ossutil/View options.md).

