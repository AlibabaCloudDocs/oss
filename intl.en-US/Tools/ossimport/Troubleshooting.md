# Troubleshooting

This topic describes the causes and solutions for common problems you may encounter when you use ossimport.

**Note:** All ossimport commands mentioned in this topic are shortened and the complete form must be used in practice.

-   Add console.bat to Windows commands. For example, change submit to console.bat submit.
-   Add bash console.sh to Linux commands. For example, change submit to bash console.sh submit.

## Common problems about migration failures

If a migration job fails, we recommend that you view the migration failure log to identify the cause. After resolving the problems, you can run the retry command to migrate the files again. The path of the migration failure log is master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/audit.log.

-   The job state is displayed as failed when you run the stat command.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7889793161/p45607.png)

    Solution: Run the stat command to view the job state. If JobState is failed, the migration job fails. After the migration job is complete, run the retry command to migrate the files again.

-   Some files fail to be migrated and retrying the migration also fails.

    Solution:

    1.  View the list of failed files from master/jobs/$\{JobName\}/failed\_tasks/$\{TaskName\}/error.list to obtain their relative paths.
    2.  Check whether you are authorized to access these files, whether these files are deleted, whether they are symbolic links, and whether the file names are garbled.
    3.  After the preceding problems are resolved, run the retry command to migrate the files again.
-   "The bucket you are attempting to access must be addressed using the specified endpoint" is displayed in the migration failure log.

    ```
    Exception:com.aliyun.oss.OSSException: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.
    <Error>
      <Code>AccessDenied</Code>
      <Message>The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint. </Message>
      <RequestId>56EA98DE815804**21B23EE6</RequestId>
      <HostId>my-oss-bucket.oss-cn-qingdao.aliyuncs.com</HostId>
      <Bucket>my-oss-bucket</Bucket>
      <Endpoint>oss-cn-hangzhou.aliyuncs.com</Endpoint>
    </Error>
    ```

    Analysis: The value of srcDomain or destDomain is invalid. Enter valid endpoints based on the endpoints listed in [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

-   "The request signature we calculated does not match the signature you provided" is displayed in the migration failure log.

    ```
    Exception:com.aliyun.oss.OSSException: The request signature we calculated does not match the signature you provided. Check your key and signing method.
    [ErrorCode]: SignatureDoesNotMatch
    [RequestId]: xxxxxxx
    [HostId]: xxx.oss-cn-shanghai.aliyuncs.com
    ```

    Analysis: The values of destAccessKey and destSecretKey are invalid. Enter a valid AccessKey pair.

-   "The bucket name "xxx/xx" is invalid" is displayed in the migration failure log.

    ```
    java.lang.IllegalArgumentException: The bucket name "xxx/xx" is invalid. A bucket name must: 1) be comprised of lower-case characters, numbers or dash(-); 2) start with lower case or numbers; 3) be between 3-63 characters long.
    ```

    Analysis: Check whether the value of destBucket is valid. The bucket name cannot contain forward slashes \(/\) and paths.

-   "Connect to xxx.oss-cn-beijing-internal.aliyuncs.com:80 timed out" is displayed in the migration failure log.

    ```
    Unable to execute HTTP request: Connect to xxx.oss-cn-beijing-internal.aliyuncs.com:80 timed out
    [ErrorCode]: ConnectionTimeout
    [RequestId]: Unknown
    ```

    Analysis: The connection timeout error is returned because the configuration file uses the internal endpoint of OSS, but the device that was used to migrate data is not an ECS instance or is not an ECS instance that is in the same region as OSS instances. The internal endpoint supports access only from ECS instances that are located in the same region as OSS instances.

    Solution:

    -   Set the domain name to a public endpoint in the configuration file. Delete the migration job and submit the job again.
    -   Use an ECS instance that is located in the same region as OSS instances to implement the migration job.
-   "The specified bucket is not valid" is displayed in the migration failure log.

    ```
    com.aliyun.oss.OSSException: The specified bucket is not valid.
    [ErrorCode]: InvalidBucketName
    [RequestId]: 57906B4DD0EBAB0FF553D661
    [HostId]: you-bucket.you-bucketoss-cn-hangzhou-internal.aliyuncs.com
    ```

    Analysis: The value of destDomain must be the endpoint of the region where the bucket is located, rather than the second-level domain name that contains the bucket name. For example, if the bucket is located in China \(Beijing\), enter oss-cn-beijing.aliyuncs.com. For more information, see [Configuration file examples](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

-   "Unable to execute HTTP request: The Difference between ... is too large" is displayed in the migration failure log.

    ```
    Unable to execute HTTP request: The Difference between the request time and the current time is too large.
    [ErrorCode]: RequestTimeTooSkewed
    [RequestId]: xxxxxxx
    ```

    Analysis: This error may result from either of the following reasons.

    -   The difference between the system time of the machine or device sending the request and the system time adopted by OSS exceeds 15 minutes. This is the most common situation.
    -   A large number of requests are sent at the same time, resulting in a high CPU utilization and slow concurrent uploads.
    Solution:

    -   Set the system time of the machine or device that is sending the request to the same time adopted by OSS.
    -   If the error is caused by high concurrency, you can reduce the number of requests that are sent at a time. You can set workerTaskThreadNum of the sys.properties file to a smaller value.
-   "No route to host" is displayed in the migration failure log.

    Analysis: This error is returned when the network connection fails because of local firewalls or iptables.

    Solution: Run the ping command to check whether the network connection between the source and destination instances is normal.

    -   If the network connection is normal, you can check if there are restrictions configured by the computer firewalls and local firewalls. You can also try disabling the firewall.
    -   If the network connection is abnormal, troubleshoot the problem and try again.
-   "Unknown http list file format" is displayed in the migration failure log when you migrate files over HTTP.

    Analysis: This error is returned when the format of the specified HTTP list file or its content is invalid.

    Solution:

    -   You can convert the format of files that are copied from a different operating system by using commands such as mac2unix and doc2unix in Linux and tools such as notepad in Windows.
    -   If the format of the HTTP list file content is invalid, change the content to the valid format. For more information about the format of the HTTP list file content, see the "Configuration files" section in [Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md).
-   "The object key "/xxxxx.jpg" is invalid" is displayed in the migration failure log.

    ```
    Exception:java.lang.IllegalArgumentException: The object key "/xxxxx.jpg" is invalid. An object name should be between 1 - 1023 bytes long when encoded as UTF-8 and cannot contain LF or CR os unsupported chars in XML1.0, and cannot begin with "/" or "\".
    ```

    Solution:

    -   Check whether srcPrefix is set to a folder but its value does not end with a forward slash \(/\). If you set the value to a folder, it must end with a forward slash \(/\).
    -   Check whether the value of destPrefix starts with a forward slash \(/\) or a backslash \(\\\). The value of destPrefix cannot start with a forward slash \(/\) or a backslash \(\\\).

## Common problems when running a migration job

If a problem occurs during the migration, you can first view the running log. When ossimport is deployed in standalone mode, the running log file path is logs/ossimport2.log. When ossimport is deployed in distributed mode, the running log file path is logs/import.log.

-   "UnsupportedClassVersionError" is returned when a command is run.

    ```
    Exception in thread "main" java.lang.UnsupportedClassVersionError: com/aliyun/ossimport2/OSSImport2 : Unsupported major.minor version 51.0
            at java.lang.ClassLoader.defineClass1(Native Method)
            at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631)
            at java.lang.ClassLoader.defineClass(ClassLoader.java:615)
            at com.simontuffs.onejar.JarClassLoader.defineClass(JarClassLoader.java:693)
            at com.simontuffs.onejar.JarClassLoader.findClass(JarClassLoader.java:599)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:306)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:247)
            at com.simontuffs.onejar.Boot.run(Boot.java:300)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    ```

    Analysis: The Java version is earlier than the required version. Update it to version 1.7 or 1.8.

-   "InvocationTargetException" is returned when you use the submit command to submit a job.

    ```
    Exception in thread "main" java.lang.reflect.InvocationTargetException
            at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
            at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.lang.reflect.Method.invoke(Method.java:497)
            at com.simontuffs.onejar.Boot.run(Boot.java:306)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    Caused by: java.lang.NullPointerException
            at com.aliyun.ossimport2.config.JobConfig.load(JobConfig.java:44)
            at com.aliyun.ossimport2.OSSImport2.doSubmitJob(OSSImport2.java:289)
            at com.aliyun.ossimport2.OSSImport2.main(OSSImport2.java:120)
            ... 6 more
    ```

    Analysis: This error is returned because the system identifies that some configuration items are deleted or commented out from the configuration file.

    Solution: Restore the configuration items that are deleted or commented out from the configuration file. Leave configurations items unspecified after the equal sign \(=\) instead of deleting them. For configuration file examples, see [Configuration file examples](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

-   "com.aliyun.oss.ClientException: Unknown" is displayed in the running log.

    ```
    com.aliyun.oss.ClientException: Unknown
    [ErrorCode]: NonRepeatableRequest
    [RequestId]: Cannot retry request with a non-repeatable request entity.  The cause lists the reason the original request failed.
    ```

    Analysis: "com.aliyun.oss.ClientException: Unknown" or "SocketTimeoutException" error is returned when the server is at full bandwidth. In this case, ossimport automatically retries. If the job still fails, run the retry command to migrate the files again.

-   "Too many open files" is displayed in the running log in Linux.

    Solution: Run the ulimit-n command to view the limit of handles in Linux.

    -   If the handle limit is greater than 10,000, use the ulimit -n 65536 command to increase the value and restart the process.
    -   If handle values are greater than 100,000, use the sudo losf -n command to check which processes have enabled the handles. You can evaluate these processes and kill the unnecessary processes to release the handles.
-   The migration job exits in seconds after starting in Windows

    Analysis:

    -   Java is not installed or a Java version earlier than 1.7 is used.
    -   Configuration file error.
    Solution:

    -   Install the supported Java version.
    -   Follow examples to edit configuration files. For more information about configuration file examples, see [Configuration file examples](/intl.en-US/Tools/ossimport/Architectures and configurations.md).
-   After a job is submitted by using the submit command, "no jobs is running or finished" is displayed when you use the stat command to view the job status.

    ```
    bash console.sh stat
    [WARN]   List files dir not exist : /home/<user>/ossimport/workdir/master/jobs/
    no jobs is running or finished.
    ```

    Analysis: A job starts running when the service is started and the job is submitted. In this case, you can use the stat command to view the job status.

    -   If you only submitted the job by using the submit command, but did not start the service by using the start command, "no jobs is running or finished" is displayed. You must use the start command to start the service.
    -   If the service is started and the job has just been submitted, Master must scan the file list first. Because the job has not been generated and distributed at this time, this error is displayed in the logs.
    -   If the error still occurs after the service has been started and the task has been submitted for a period of time, check whether the process exits unexpectedly after it starts. When ossimport is deployed in standalone mode, view the log file from logs/ossimport2.log. When ossimport is deployed in distributed mode, view the log file from logs/ossimport.log. Find the cause of the exception, resolve it, and then start the service process.
-   "scanFinished: false " is displayed when you use the stat command to check the job status.

    Solution: Check whether the total number of tasks is increasing.

    -   If the total number of tasks is increasing, the file list of the migration job is listing new files. It is normal that this error is returned in this case.
    -   If the incremental data migration mode is enabled, scanFinished is not true, and the number of tasks does not change, ossimport scans the file list at specified intervals to check for new or modified files.
    -   If the incremental data migration mode is disabled and the number of tasks does not increase, check the running log for exceptions. When ossimport is deployed in standalone mode, view the log file from logs/ossimport2.log. When ossimport is deployed in distributed mode, view the log file from logs/ossimport.log. Find the cause of the exception, resolve it, and then start the service process.
-   The process in Linux does not respond, but no exception is output in logs

    Analysis: If the system has less than 2 GB of available memory, the process may be killed due to insufficient memory. You can check whether the dmesg log contains records about the processes that were killed because of insufficient memory.

-   How do I restart a service after the process stops responding or is killed?

    All submitted jobs have breakpoint records. If you do not clear the original job by using the clean command, you can directly use the start command to start the service without submitting the job again.

-   How do I upload files whose names are garbled to OSS in Linux?

    Solution:

    1.  Check the encoding format of the garbled names.
    2.  Use the export LANG="<your file name encode\>" command to parse these encoded names.
    3.  Use the clean command to clear the original job, and then use the submit command to submit the job again.
-   "java.nio.file.AccessDeniedException" is returned when the service is started.

    Analysis: You are not authorized to access the configuration file.

    Solution: Set the access permission of the configuration file to public read, or log on to Linux as an administrator to start the service.

-   Task Counts are displayed as 0, but JobState is displayed as succeed.

    Description: Pending Task Count and Diamond Task Count are both 0, but JobState is SUCCEED.

    ```
    [2015-12-28 16:12:35]   [INFO]  JobName:dir_data
    [2015-12-28 16:12:35]   [INFO]  Pending Task Count:0
    [2015-12-28 16:12:35]   [INFO]  Dispatched Task Count:0
    [2015-12-28 16:12:35]   [INFO]  Succeed Task Count:0
    [2015-12-28 16:12:35]   [INFO]  Failed Task Count:0
    [2015-12-28 16:12:35]   [INFO]  Is Scan Finished:true
    [2015-12-28 16:12:35]   [INFO]  JobState:SUCCEED
    ```

    Analysis:

    -   Files cannot be listed because the value of srcPrefix is invalid.
    -   Folders specified by srcPrefix do not have files. Folders cannot be uploaded to OSS because the concept of folders is simulated by OSS.
    Solution: Set a valid value for the srcPrefix parameter and make sure there are available files in the folder specified by srcPrefix.

-   "InvocationTargetException" is returned when you submit the job.

    ```
    submit job:/disk2/ossimport2/local_job.cfg
    Exception in thread "main" java.lang.reflect.InvocationTargetException
            at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
            at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.lang.reflect.Method.invoke(Method.java:606)
            at com.simontuffs.onejar.Boot.run(Boot.java:306)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    Caused by: java.lang.NullPointerException
            at com.aliyun.ossimport2.OSSImport2.doSubmitJob(OSSImport2.java:289)
            at com.aliyun.ossimport2.OSSImport2.main(OSSImport2.java:120)
            ... 6 more
    ```

    Analysis: This error is returned because the configuration file or configuration file path is invalid.

    Solution:

    1.  Configure the workingDir configuration item in the conf/sys.properties file properly.
    2.  If the configuration file is valid, check the configuration file path.
-   Source files for synchronization do not exist.

    Analysis: Master first lists the files and then migrates data based on the list. If files are deleted from the source after listing, they cannot be migrated. The deleted files are skipped during migration, and the names are listed in the error list.

-   The running status of the migration job is inconsistent with the configuration file.

    Description: The job configuration file is valid, but the job running status is inconsistent with the job configuration file during the migration.

    Analysis: If a migration job has been submitted, modifications to the job configuration file do not take effect after the job is suspended.

    Solution: Clear the previous job by using the clean command. After the job configuration file is modified, submit the job again.


## Common problems about migrating data from UPYUN Storage Service \(USS\) to OSS

-   The number of jobs is always 0.

    Analysis: View the job running log first.

    ```
    [2016-07-21 10:21:46] [INFO] [name=YoupaiList, totalRequest=1729925, avgLatency=38,
              recentLatency=300000]
    ```

    -   If the value of `recentLatency` in the running log is smaller than or equal to 30000, the files are listed properly. It can take more than 30 seconds \(timeout\) for files to be listed on USS in most cases. Files listed within 30 seconds are returned. This problem disappears when all files are listed.
    -   If the value of `recentLatency` is small, this problem occurs because the account password is invalid. USS only returns null instead of error results when SDK errors occur. You can obtain the error code returned by USS for troubleshooting only by capturing packets.
-   How to specify `srcAccessKey` and `srcSecretKey`.

    Enter the operator account and password of USS.

-   HTTP status code 429 keeps appearing.

    USS has specified the SDK access interval. If too many requests are sent over a short period of time, the system limits the number of requests to be handled. We recommend that you contact the customer service of USS to remove the restrictions. ossimport retries in this case.


## Common problems

-   After the migration job is complete, the data volume displayed in the OSS console is smaller than the source data volume.

    Description: The bucket size does not change in the OSS console after all the migration job is complete, or the data sizes calculated by using the du command vary greatly in Linux.

    Analysis:

    -   Bucket sizes in OSS console are updated with a delay of one to two hours.
    -   The du command in Linux counts the block size, which is larger than the file size. You can use the ls -lR <absolute folder path\> \| grep "\\-rw" \| awk '\{sum+=$5\}END\{print sum\}' command to count the actual size of the local folder.
-   The error message such as unknown command "java" or unknown command "nohup" is returned when you run commands in Linux.

    Analysis: The command that you want to run is not installed. Use the yum, apt-get, or `zypper` command to install the corresponding command based on operating systems.

-   Can I use `srcPrefix` in the configuration file to specify files?

    You can only use `srcPrefix` to specify folders or prefixes.

-   Can I configure proxies for ossimport?

    No. ossimport does not support proxies.

-   Why am I charged for migrating data within OSS?

    If you migrate data by using an internal endpoint, you are charged based on the number of requests. No traffic fee is charged.

-   Will files that are deleted from the local folder be deleted from OSS if the incremental data migration mode is enabled?

    No. If the incremental data migration mode is enabled, files that are deleted from the local folder will not be deleted from OSS.

-   Why are there new files that are not synchronized when the incremental data migration mode is enabled?

    In incremental data migration mode, the last modification time of a file is used to determine whether the file is new. Linux mv command, Windows commands such as cp, mv, and rsync with `-t` or `-a` parameters do not modify the last modification time of files. Files modified by using these commands are not scanned for and therefore are ignored during migration.

-   Can file permissions be migrated when files are migrated to OSS?

    No. After the migration is complete, you can use the set-meta command of ossutil to modify the permissions. For more information, see [set-meta](/intl.en-US/Tools/ossutil/Common commands/set-meta.md).


