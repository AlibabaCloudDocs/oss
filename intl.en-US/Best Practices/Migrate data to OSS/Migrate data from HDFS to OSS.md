# Migrate data from HDFS to OSS

This topic describes how to use the Jindo DistCp tool provided by Alibaba Cloud to migrate data from HDFS to OSS.

Hadoop Distributed File System \(HDFS\) is used as the underlying storage for a large amount of data in traditional big data architectures. The DistCp tool provided by Hadoop is used to migrate and copy data in HDFS. However, this tool cannot take advantage of the features of OSS, which results in low efficiency and poor data consistency. In addition, DistCp provides only simple functions, which cannot meet the requirements of users.

Jindo DistCp is a tool provided by Alibaba Cloud to copy files in HDFS. It allows you to copy files within or between large-scale clusters. Jindo DistCp uses MapReduce to distribute files, handle errors, and recover data. It uses lists of files and directories as the input of map/reduce tasks. Each map/reduce task copies the files and directories in the input list. Jindo DistCp allows you to copy data between HDFS systems, between HDFS and OSS, and between OSS buckets. It also provides various custom parameters and policies for data copying.

Compared with Hadoop DistCp, Jindo DistCp provides the following benefits when you migrate data from HDFS to OSS:

-   Provide high efficiency. The data migration speed of Jindo DistCp is 1.59 times faster than that of Hadoop DistCp.
-   Provide rich basic functions, multiple copy methods, and various scenario-based optimization policies.
-   Take advantages of OSS features to perform various operations on files, including compressing files and converting the storage class of the files to IA or Archive.
-   Copy files without changing files names to ensure data consistency.
-   Supports various scenarios and can be used to replace Hadoop DistCp. Jindo DistCp supports Hadoop 2.7.x and Hadoop 3.x.

## Prerequisites

-   If you use a user-created ECS cluster, you must deploy a Hadoop 2.7.x or Hadoop 3.x environment and be able to run MapReduce jobs.
-   If you use Alibaba Cloud E-MapReduce \(EMR\):
    -   For EMR-3.28.0 or Bigfoot 2.7.0 and later, you can run Shell commands to use Jindo DistCp. For more information, see [t1903713.dita\#task\_2526637](/intl.en-US/SmartData/Use SmartData in EMR V3.28.X/Use Jindo DistCp.md).
    -   For versions earlier than EMR-3.28.0, compatibility issues may occur when you use Jindo DistCp. You can submit a ticket to solve the issues.

## Step 1: Download the JAR package of Jindo DistCp

-   [JAR package of Jindo DistCp for Hadoop 2.7.x](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp/Hadoop2.7%2BS3/jindo-distcp-2.7.3.jar)
-   [JAR package of Jindo DistCp for Hadoop 3.x](https://smartdata-binary.oss-cn-shanghai.aliyuncs.com/Jindo-distcp/Hadoop3.x%2BS3/jindo-distcp-2.7.3.jar)

## Step 2: Configure the AccessKey pair used to access OSS

You can configure the AccessKey pair by using one of the following methods:

-   Set the --key, --secret, and --endPoint parameters in the command to configure the AccessKey pair. The following command provides an example on how to configure the AccessKey pair:

    ```
    hadoop jar jindo-distcp-2.7.3.jar --src /data/incoming/example_file --dest oss://example_folder/example_file --key yourAccessKeyId --secret yourAccessKeySecret --endPoint oss-cn-hangzhou.aliyuncs.com
    ```

    -   --src: specifies the path of the source file, which is `/data/incoming/example_file` in the sample command.
    -   --dest: specifies the path of the destination file, which is `oss://example_folder/example_file` in the sample command.
    -   --key: specifies your AccessKey ID. For more information about AccessKey IDs, see [Create an AccessKey pair]().
    -   --secret: specifies your AccessKey secret. For more information about AccessKey secret, see [Create an AccessKey pair]().
    -   --endPoint: specifies the endpoint of the region where the bucket that stores the destination file is located, which is `oss-cn-hangzhou.aliyuncs.com` in the sample command. For more information about the regions and endpoints of OSS, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).
-   Set the --key, --secret, and --endPoint parameters in the core-site.xml configuration file to avoid setting these parameters each time you copy files to OSS. The following code provides an example on how to set parameters related to AccessKey pairs in the configuration file:

    ```
    <configuration>
        <property>
            <name>fs.jfs.cache.oss-accessKeyId</name>
            <value>xxx</value>
        </property>
        <property>
            <name>fs.jfs.cache.oss-accessKeySecret</name>
            <value>xxx</value>
        </property>
        <property>
            <name>fs.jfs.cache.oss-endpoint</name>
            <value>oss-cn-xxx.aliyuncs.com</value>
        </property>
    </configuration>
    ```

-   Configure the password-free feature of Jindo DistCp to avoid storing your AccessKey pairs in plaintext, which makes data more secure. For more information, see [t1872858.dita\#task\_2435437](/intl.en-US/SmartData/Use SmartData in EMR versions earlier than V3.27.0/Use the password-free feature of JindoFS SDK.md).

## Step 3: Configure parameters

You can configure multiple parameters to use the various functions provided by Jindo DistCp. The following table lists the parameters that you can configure and configuration examples. For the detailed usage and sample commands of Jindo DistCp, visit [How to use Jindo DistCp](https://github.com/aliyun/aliyun-emapreduce-sdk/blob/master-2.x/docs/jindo_distcp_how_to.md?spm=a2c6h.12873639.0.0.700f97aeCrmwQP&file=jindo_distcp_how_to.md).

|Parameter|Required|Description|Example|
|---------|--------|-----------|-------|
|--src|Yes|The path of the source file.|--src oss://exampleBucket/sourceDir|
|--dest|Yes|The path of the destination file.|--dest oss://exampleBucket/destDir|
|--parallelism|No|The number of copy tasks that can be concurrently run. You can set this parameter based on your cluster resources.|--parallelism 10|
|--policy|No|The storage class of the destination file. Valid values:-   archive \(Archive\)
-   ia \(Infrequently Access\)

|--policy archive|
|--srcPattern|No|Specifies whether to use regular expressions to select or filter the files to copy. You can use custom regular expressions to filter the files to copy. The paths specified in regular expressions must be full paths.|Configure the parameter as follows to copy only files whose names end with `.log`:--srcPattern . \*\\.log |
|--deleteOnSuccess|No|Specifies whether to delete the source file after it is copied to OSS. If you set this parameter, the copy operation is similar to the operation performed by the mv command.|--deleteOnSuccess|
|--outputCodec|No|The compression method for the files to copy. Valid values: gzip, gz, lzo, and snappy. Jindo GistCp also supports the following two keywords:-   none: saves the copied files uncompressed. If the files are compressed, Jindo DistCp decompress the files.
-   keep: keeps the compression status of the source files.

|--outputCodec gzip|
|srcPrefixesFile|No|The list that includes files to copy at a time. The files in the lists are prefixed with the path specified by the src parameter.|--srcPrefixesFile file:///opt/folders.txt|
|--outputManifest|No|Specifies that a gzip file is generated in the directory specified by the dest parameter to record the information about the copied files.|--outputManifest=manifest-2020-04-17.gz|
|--requirePreviousManifest|No|Specifies whether to read the information about copied files to perform incremental update. Valid values:-   false: The information about copied files is not read.
-   true: The information about copied files is read.

|--requirePreviousManifest=false|
|--previousManifest|No|Specifies that the information about copied files is read in this copy to perform incremental update.|--previousManifest=oss://exampleBucket/manifest-2020-04-16.gz|
|--copyFromManifest|No|Specifies that the files recorded in the manifest file are copied. In general, this parameter is configured with --previousManifest at the same time.|--previousManifest oss://exampleBucket/manifest-2020-04-16.gz --copyFromManifest|
|--groupBy|No|The regular expression used to filter the files to aggregate. This parameter must be configured with targetSize at the same time.|--groupBy='.\*/\(\[a-z\]+\). \*.txt'|
|--targetSize|No|The maximum size of the aggregated file. Unit: MB.|--targetSize=10|
|--enableBalancePlan|No|The copy strategy for scenarios where the size of the files to copy differs slightly.|--enableBalancePlan|
|--enableDynamicPlan|No|The copy strategy for scenarios where the size of the files to copy differs greatly such as scenarios where large files and small files are copied at the same time.|--enableDynamicPlan|
|--enableTransaction|No|The copy strategy for scenarios where the data consistency between jobs is required. By default, only the data consistency between tasks is guaranteed.|--enableTransaction|
|--diff|No|The comparison strategy used to check whether all files are copied in the current task. If not all files are copied, a list that includes the copied files is generated.|--diff|
|--key|No|The AccessKey ID used to access OSS.|--key yourAccessKeyId|
|--secret|No|The AccessKey secret used to access OSS.|--secret yourAccessKeySecret|
|--endPoint|No|The endpoint of the region where the bucket that stores the destination files is located.|--endPoint oss-cn-hangzhou.aliyuncs.com|
|--cleanUpPending|No|Specifies that residual files in OSS are cleaned up. If you specify this parameter, Jindo DistCp takes some time to clean up the residual files.|--cleanUpPending|

