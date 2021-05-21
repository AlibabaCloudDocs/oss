# probe

The probe command is used to probe access to OSS. You can use this command to detect network exceptions that occurred between the local client and OSS server and check upload and download bandwidths and the status of local symbolic links.

**Note:** Sample command lines in this topic are based on the 64-bit Linux system. For other systems, replace ./ossutil64 in the commands with the corresponding binary name. For more information, see [ossutil](/intl.en-US/Quick Start/ossutil.md).

## Usage notes

You can use the probe command to detect network exceptions that occurred between the local client and OSS server and check the upload and download bandwidths. You can also use this command to check the status of a large number of symbolic links before you upload them.

-   Detect network exceptions

    ossutil checks the network between the local client and OSS server by uploading or downloading objects. To upload or download objects to the specified location when you detect network exceptions, we recommend that you specify the names of the objects. If you need only to detect network exceptions, you can use the command without specifying object names. ossutil generates temporary objects for detection and deletes the objects after the detection.

-   Check upload and download bandwidths

    When you use this command to check upload and download bandwidths, OSS provides a recommended number of concurrent tasks in uploads and downloads based on the CPU of your local devices and your network bandwidth. You can configure the number of concurrent tasks for ossutil as recommended to maximize the usage of your network bandwidth.

-   Check the status of local symbolic links

    When you upload a large number of symbolic links, the upload is interrupted when one of the links is abnormal. We recommend that you check the status of local symbolic links before you upload them. If one of the symbolic links is abnormal, fix the problem before you upload the links.


After you run the probe command, you can view the procedures and results of each upload or download task.

-   Detect network exceptions by uploading or downloading objects
    -   A check sign \(√\) displayed following a step indicates that the step succeeded. A cross sign \(×\) following a step indicates that the step failed.
    -   If the detection succeeds, the information about the upload or download task is displayed as the result, including the size of the uploaded or downloaded object, the time used to perform the upload or download task, and the location to which the object is uploaded or downloaded.
    -   If the detection fails, ossutil returns the error causes or error codes, which can be used to troubleshoot the errors.

        For more information about error codes, see [Error responses](/intl.en-US/Troubleshooting/Error responses.md).

    -   After a detection, a log file whose name is in the following format is generated in the installation directory of ossutil: `logOssProbe + Timestamp when the detection ends.log`. The log file contains the detailed information about the execution of the probe command.
-   Check a specific index

    When you use the probe command to check the upload and download bandwidths or the status of local symbolic links, ossutil provides the check results and suggestions.


## Check the network by uploading an object and generate a report

ossutil checks the network between the local client and a bucket by uploading an object to the bucket.

-   Command syntax

    ```
    ./ossutil probe {--upload [file_name]} {--bucketname bucket_name} [--object object_name] [--addr domain_name] [--upmode]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |--upload|Yes|Specifies that the detection is performed by uploading an object.|
    |file\_name|No|The complete path of the local file that you want to upload to the bucket. If you leave this parameter unspecified, ossutil generates a temporary file and detect the network by uploading the file.|
    |--bucketname|No|The name of the bucket to which you want to upload the object.|
    |--object|No|The name of the object that you want to upload. If you specify this parameter in the command, ossutil stores the uploaded object in the specified bucket. If you do not specify this parameter in the command, ossutil deletes the uploaded object after the detection.|
    |--addr|No|The address of the network that you want to check. ossutil uses the ping command to check the network connectivity between the local client and the specified address.Default value: `www.aliyun.com`. |
    |--upmode|No|The method used to upload the object. Default value: normal.Valid values:

    -   normal: The object is uploaded by using simple upload.
    -   append: The object is uploaded by using append upload.
    -   multipart: The object is uploaded by using multipart upload. |

-   Examples
    -   Check the network by uploading a random object and specifying the address of the network.

        In this example, the bucket to which the object is uploaded is named examplebucket and the address of the network to check is `aliyun.com`. You can run the following command to check the network:

        ```
        ./ossutil probe --upload --bucketname examplebucket --add aliyun.com
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(normal)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:122880(byte)
        upload time consuming:245(ms)
        (only the time consumed by probe command)
        
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201173031.log
        ```

    -   Check the network by uploading the specified object in default simple upload mode and delete the uploaded object after the detection.

        In this example, the file named example.txt in the local root directory is uploaded to the bucket named examplebucket. You can run the following command to check the network:

        ```
        ./ossutil probe --upload example.txt --bucketname examplebucket 
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(normal)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:7(byte)
        upload time consuming:224(ms)
        (only the time consumed by probe command)
        upload object is example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201173841.log
        ```

    -   Check the network by uploading the specified object in append upload mode and store the uploaded object in the bucket after detection.

        In this example, the file named example.txt in the local root directory is uploaded to the bucket named examplebucket. You can run the following command to check the network:

        ```
        ./ossutil probe --upload example.txt --bucketname examplebucket --object example.txt --upmode append
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare file...[√]
        begin network detection...[√]
        begin upload file(append)...[√]
        
        *************************  upload result  *************************
        upload file:success
        upload file size:7(byte)
        upload time consuming:171(ms)
        (only the time consumed by probe command)
        
        upload object is example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201201174126.log
        ```


## Check the network by downloading an object with its URL and generate a report

ossutil checks the network between the local client and a bucket by downloading an object with its URL from the bucket.

-   Command syntax

    ```
    ./ossutil probe {--download} {--url http_url} [--addr=domain_name] [file_name]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |--download|Yes|Specifies that the detection is performed by downloading an object.|
    |--url|Yes|The URL of the object that you want to download. ossutil uses this URL to download the object to a local directory.    -   If the ACL of the object to download is public read, set this parameter to the URL of the object. Example: `https://examplebucket.oss-cn-beijing.aliyuncs.com/example.jpg`.
    -   If the ACL of the object to download is private, set this parameter to the signed URL of the object. The URL must be start and ends with a pair of double quotation marks \(""\). Example: `"https://examplebucket.oss-cn-beijing.aliyuncs.com/example.jpg?Expires=1552015472&OSSAccessKeyId=TMP.******5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3A******oCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D"`. |
    |--addr|No|The address of the network that you want to check. ossutil uses the ping command to check the network connectivity between the local client and the specified address.Default value: `www.aliyun.com` |
    |file\_name|No|The local path to which the object is downloaded.    -   If you specify only the object name but do not specify a directory, the object is stored with the specified name in the installation directory of ossutil.
    -   If you specify only a directory but do not specify the object name, the object is stored with its original name in the specified directory.
    -   If you leave this parameter unspecified, the object is stored with its original name in the installation directory of ossutil. |

-   Examples
    -   Check the network by downloading the specified object with its URL and renaming the object.

        In this example, the public-read object named `example.txt` in the bucket named examplebucket is downloaded using its URL: `https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt`. The object is renamed to `/localfile/test.txt` after it is downloaded. You can run the following command to check the network:

        ```
        ./ossutil probe --download --url https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt  /localfile/test.txt
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1246(ms)
        (only the time consumed by probe command)
        
        download file is /localfile/test.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202171639.log
                                        
        ```

    -   Check the network by downloading the specified object with its URL and specifying the address of the network.

        In this example, the public-read object named `example.txt` in the bucket named examplebucket is downloaded using its URL: `https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt`. The address of the network to check is `aliyun.com`. You can run the following command to check the network:

        ```
        ./ossutil probe --download --url https://examplebucket.oss-cn-beijing.aliyuncs.com/example.txt --add aliyun.com
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1344(ms)
        (only the time consumed by probe command)
        
        download file is /root/example.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202172232.log                                
        ```


## Check the network by downloading the specified object and generate a report

ossutil checks the network between the local client and a bucket by downloading an object from the bucket.

-   Command syntax

    ```
    ./ossutil probe {--download} {--bucketname bucket_name} [--object object_name] [--add domain_name] [file_name]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |--download|Yes|Specifies that the detection is performed by downloading an object.|
    |--bucketname|Yes|The name of the bucket from which you want to download the object.|
    |--object|No|The name of the object that you want to download. If you specify this parameter in the command, ossutil downloads the specified object to a local directory. If you do not specify this parameter in the command, ossutil generates a temporary object, uploads the object to the specified bucket, and downloads the object. After the detection, ossutil deletes the temporary object from the specified bucket.|
    |--addr|No|The address of the network that you want to check. ossutil uses the ping command to check the network connectivity between the local client and the specified address.Default value: `www.aliyun.com`. |
    |file\_name|No|The local path to which the object is downloaded.    -   If you specify only the object name but do not specify a directory, the object is stored with the specified name in the installation directory of ossutil.
    -   If you specify only a directory but do not specify the object name, the object is stored with its original name in the specified directory.
    -   If you leave this parameter unspecified, the object is stored with its original name in the installation directory of ossutil. |

-   Examples
    -   Check the network by downloading and renaming the specified object.

        In this example, the object `/ossfolder/example.txt` in the bucket named examplebucket is downloaded to a local directory and is renamed to `/localfolder/test.txt` after the object is downloaded. You can run the following command to check the network:

        ```
        ./ossutil probe --download --bucketname examplebucket --object /ossfolder/example.txt /localfolder/text.txt
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:57374182(byte)
        download time consuming:1108(ms)
        (only the time consumed by probe command)
        
        download file is /localfolder/text.txt
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202173032.log
        ```

    -   Check the network by downloading a temporary object and specifying the address of the network.

        In this example, a temporary object is uploaded to and downloaded from the bucket named examplebucket. The address of the network to check is `aliyun.com`. You can run the following command to check the network:

        ```
        ./ossutil probe --download --bucketname examplebucket --add aliyun.com
        ```

        A similar output is displayed:

        ```
        begin parse parameters and prepare object...[√]
        begin network detection...[√]
        begin download file...[√]
        
        *************************  download result  *************************
        download file:success
        download file size:122880(byte)
        download time consuming:516(ms)
        (only the time consumed by probe command)
        
        download file is /root/oss-test-probe-1606902911701892000-3ifwj9t0ln
        
        ************************* report log info*************************
        report log file:/root/logOssProbe20201202173512.log
        ```


## Check a specific index

You can use the probe command to check a specific index, including the status of local symbolic links and the upload and download bandwidths. ossutil also returns the names of abnormal symbolic links or a recommended number of concurrent tasks in uploads and downloads based on the check results.

-   Command syntax

    ```
    ./ossutil probe {--probe-item item_value} {--bucketname bucket-name} [--object object_name]
    ```

    The following table describes the parameters you can configure in the command.

    |Parameter|Required|Description|
    |---------|--------|-----------|
    |--probe-item|Yes|The index that you want to check.Valid values:

    -   cycle-symlink: checks whether local abnormal symbolic links exist.
    -   upload-speed: checks the upload bandwidth.
    -   download-speed: checks the download bandwidth. |
    |--bucketname|Yes when the value of --probe-item is not `cycle-symlink`|The name of the bucket for which you want to check the upload or download bandwidth.|
    |--object|Yes when the value of --probe-item is `download-speed`|The path of the object that you want to download. The object must exist and be larger than 5 MB in size. Example: `ossfolder/example.txt`.|

-   Examples
    -   Check whether abnormal symbolic links exists in the localfolder directory of the local root directory.

        You can run the following command to check abnormal symbolic links:

        ```
        ./ossutil probe --probe-item cycle-symlink /root/localfolder
        ```

        A similar output is displayed:

        ```
        Error: stat /root/localfolder/example.jpg: no such file or directory
        ```

        The results show that the symbolic link named example.jpg is abnormal.

    -   Check the upload bandwidth.

        In this example, ossutil uploads a temporary object to the bucket named examplebucket and returns a recommended number of concurrent tasks in upload based on the hardware specification of the current device and the upload bandwidth.

        You can run the following command to check the upload bandwidth:

        ```
        ./ossutil probe --probe-item upload-speed --bucketname examplebucket
        ```

        A similar output is displayed:

        ```
        cpu core count:2
        parallel:2,average speed:679.72(KB/s),current speed:1344.00(KB/s),max speed:1440.00(KB/s))
        parallel:3,average speed:643.31(KB/s),current speed:704.00(KB/s),max speed:1632.00(KB/s))
        parallel:4,average speed:646.62(KB/s),current speed:512.00(KB/s),max speed:1600.00(KB/s))
        
        suggest parallel is 2, max average speed is 679.72(KB/s)
        ```

        The results show that the CPU of the device has two cores and the maximum average upload bandwidth is 679.72 KB/s. Based on these results, ossutil recommends that you set the number of concurrent upload tasks to 2.

    -   Check the download bandwidth.

        In this example, ossutil downloads the object named `example.txt` from the bucket named examplebucket to a local directory and returns a recommended number of concurrent tasks in download based on the hardware specification of the current device and the download bandwidth.

        You can run the following command to check the download bandwidth:

        ```
        ./ossutil probe --probe-item download-speed --bucketname examplebucket --object example.txt
        ```

        A similar output is displayed:

        ```
        cpu core count:2
        parallel:2,average speed:12524.93(KB/s),current speed:12288.63(KB/s),max speed:14302.25(KB/s)
        parallel:3,average speed:12564.45(KB/s),current speed:12144.39(KB/s),max speed:14484.24(KB/s)
        parallel:4,average speed:12545.21(KB/s),current speed:12766.58(KB/s),max speed:13534.42(KB/s)
        
        suggest parallel is 3, max average speed is 12564.45(KB/s)
        ```

        The results show that the CPU of the device has two cores and the maximum average download bandwidth is 12,564.45 KB/s. Based on these results, ossutil recommends that you set the number of concurrent download tasks to 3.


