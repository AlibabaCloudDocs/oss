# Use HDP 2.6-based Hadoop to read and write OSS data

Hortonworks Data Platform \(HDP\) is a big data platform released by Hortonworks and consists of open source components such as Hadoop, Hive, and HBase. Hadoop 3.1.1 is included in the HDP 3.0.1 and supports OSS. However, earlier versions of HDP do not support OSS. This topic uses HDP 2.6.1.0 as an example to describe how to configure HDP 2.6 to read and write OSS data.

An HDP 2.6.1.0 cluster has been set up. To set up an HDP 2.6.1.0 cluster, use one of the following methods:

-   Search for references to use Ambari to set up an HDP 2.6.1.0 cluster.
-   If Ambari is not available, manually set up an HDP 2.6.1.0 cluster.

## Procedure

1.  Click [here](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-hdp-2.6.1.0-129.tar.gz) to download the HDP 2.6.1.0 package that supports OSS.

    This package includes the support for OSS based on Hadoop in [HDP 2.6.1.0](https://github.com/hortonworks/hadoop-release/tree/HDP-2.6.1.0-tag). Alibaba Cloud plans to continually release support packages for minor versions of HDP 2.

2.  Decompress the downloaded package.

    ```
     [root@hdp-master ~]# tar -xvf hadoop-oss-hdp-2.6.1.0-129.tar
    hadoop-oss-hdp-2.6.1.0-129/
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-ram-3.0.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-core-3.4.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-ecs-4.2.0.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-java-sdk-sts-3.0.0.jar
    hadoop-oss-hdp-2.6.1.0-129/jdom-1.1.jar
    hadoop-oss-hdp-2.6.1.0-129/aliyun-sdk-oss-3.4.1.jar
    hadoop-oss-hdp-2.6.1.0-129/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    ```

3.  Move the Hadoop-aliyun-2.7.3.2.6.1.0-129.jar package to the $\{/usr/hdp/current\}/hadoop-client/ directory. Move other jar packages to the $\{/usr/hdp/current\}/hadoop-client/lib/ directory.

    Directory structure after the preceding adjustments:

    ```
    [root@hdp-master ~]# ls -lh /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    -rw-r--r-- 1 root root 64K Oct 28 20:56 /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar
    
    [root@hdp-master ~]# ls -ltrh /usr/hdp/current/hadoop-client/lib
    total 27M
    ......
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 ranger-hdfs-plugin-impl
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 ranger-yarn-plugin-impl
    drwxr-xr-x 2 root root 4.0K Oct 28 20:10 native
    -rw-r--r-- 1 root root 114K Oct 28 20:56 aliyun-java-sdk-core-3.4.0.jar
    -rw-r--r-- 1 root root 513K Oct 28 20:56 aliyun-sdk-oss-3.4.1.jar
    -rw-r--r-- 1 root root  13K Oct 28 20:56 aliyun-java-sdk-sts-3.0.0.jar
    -rw-r--r-- 1 root root 211K Oct 28 20:56 aliyun-java-sdk-ram-3.0.0.jar
    -rw-r--r-- 1 root root 770K Oct 28 20:56 aliyun-java-sdk-ecs-4.2.0.jar
    -rw-r--r-- 1 root root 150K Oct 28 20:56 jdom-1.1.jar
    ```

    **Note:** All content in $\{\} is environment variables. Modify these environment variables.

4.  Perform the preceding operations on all HDP nodes.

5.  Use Ambari to add configurations. If your cluster does use Ambari for management, modify core-site.xml. This example uses Ambari. The following table describes the configurations you need to add.

    |Parameter|Configuration method|
    |:--------|:-------------------|
    |**fs.oss.endpoint**|Enter the endpoint used to access the region where the bucket is located. Example: oss-cn-zhangjiakou-internal.aliyuncs.com. |
    |**fs.oss.accessKeyId**|Enter the AccessKey ID used to access OSS.|
    |**fs.oss.accessKeySecret**|Enter the AccessKey secret used to access OSS.|
    |**fs.oss.impl**|Enter the class used to implement the OSS file system through Hadoop. Set the value to org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem.|
    |**fs.oss.buffer.dir**|Enter the temporary file directory. We recommend that you set this parameter to /tmp/oss. |
    |**fs.oss.connection.secure.enabled**|Specify whether to enable HTTPS. Performance may be affected when HTTPS is enabled. We recommend that you set this parameter to false. |
    |**fs.oss.connection.maximum**|Enter the maximum number of connections to OSS. We recommend that you set this parameter to 2048. |

    For more information about parameter descriptions, visit [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md).

6.  Restart the cluster as prompted by Ambari.

7.  Test data reading from and writing to OSS.

    -   Test data reading from OSS

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   Test data writing to OSS

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        If data can be read from and written to OSS, the configurations are successful. Otherwise, check configurations.

8.  To run MadReduce jobs, modify the content in the hdfs://hdp-master:8020/hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz package. To run TEZ jobs, modify the content in the hdfs://hdp-master:8020/hdp/apps/2.6.1.0-129/tez/tez.tar.gz package. Modify the package based on job types. Add the OSS-compliant package to the preceding package. Run the following commands:

    ```
    [root@hdp-master ~]# sudo su hdfs
    [hdfs@hdp-master root]$ cd
    [hdfs@hdp-master ~]$ hadoop fs -copyToLocal /hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz .
    [hdfs@hdp-master ~]$ hadoop fs -rm /hdp/apps/2.6.1.0-129/mapreduce/mapreduce.tar.gz
    [hdfs@hdp-master ~]$ cp mapreduce.tar.gz mapreduce.tar.gz.bak
    [hdfs@hdp-master ~]$ tar zxf mapreduce.tar.gz
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/hadoop-aliyun-2.7.3.2.6.1.0-129.jar hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/lib/aliyun-* hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ cp /usr/hdp/current/hadoop-client/lib/jdom-1.1.jar hadoop/share/hadoop/tools/lib/
    [hdfs@hdp-master ~]$ tar zcf mapreduce.tar.gz hadoop
    [hdfs@hdp-master ~]$ hadoop fs -copyFromLocal mapreduce.tar.gz /hdp/apps/2.6.1.0-129/mapreduce/
    ```


## Verify the configurations

You can test TeraGen and TeraSort to check whether the configurations take effect.

-   Test TeraGen:

    ```
    [hdfs@hdp-master ~]$ hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapred.map.tasks=100 10995116 oss://{bucket-name}/1G-input
    18/10/28 21:32:38 INFO client.RMProxy: Connecting to ResourceManager at cdh-master/192.168.0.161:8050
    18/10/28 21:32:38 INFO client.AHSProxy: Connecting to Application History server at cdh-master/192.168.0.161:10200
    18/10/28 21:32:38 INFO aliyun.oss: [Server]Unable to execute HTTP request: Not Found
    [ErrorCode]: NoSuchKey
    [RequestId]: 5BD5BA7641FCE369BC1D052C
    [HostId]: null
    18/10/28 21:32:38 INFO aliyun.oss: [Server]Unable to execute HTTP request: Not Found
    [ErrorCode]: NoSuchKey
    [RequestId]: 5BD5BA7641FCE369BC1D052F
    [HostId]: null
    18/10/28 21:32:39 INFO terasort.TeraSort: Generating 10995116 using 100
    18/10/28 21:32:39 INFO mapreduce.JobSubmitter: number of splits:100
    18/10/28 21:32:39 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1540728986531_0005
    18/10/28 21:32:39 INFO impl.YarnClientImpl: Submitted application application_1540728986531_0005
    18/10/28 21:32:39 INFO mapreduce.Job: The url to track the job: http://cdh-master:8088/proxy/application_1540728986531_0005/
    18/10/28 21:32:39 INFO mapreduce.Job: Running job: job_1540728986531_0005
    18/10/28 21:32:49 INFO mapreduce.Job: Job job_1540728986531_0005 running in uber mode : false
    18/10/28 21:32:49 INFO mapreduce.Job:  map 0% reduce 0%
    18/10/28 21:32:55 INFO mapreduce.Job:  map 1% reduce 0%
    18/10/28 21:32:57 INFO mapreduce.Job:  map 2% reduce 0%
    18/10/28 21:32:58 INFO mapreduce.Job:  map 4% reduce 0%
    ...
    18/10/28 21:34:40 INFO mapreduce.Job:  map 99% reduce 0%
    18/10/28 21:34:42 INFO mapreduce.Job:  map 100% reduce 0%
    18/10/28 21:35:15 INFO mapreduce.Job: Job job_1540728986531_0005 completed successfully
    18/10/28 21:35:15 INFO mapreduce.Job: Counters: 36
    ...
    ```

-   Test TeraSort:

    ```
    [hdfs@hdp-master ~]$ hadoop jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapred.map.tasks=100 oss://{bucket-name}/1G-input oss://{bucket-name}/1G-output
    18/10/28 21:39:00 INFO terasort.TeraSort: starting
    ...
    18/10/28 21:39:02 INFO mapreduce.JobSubmitter: number of splits:100
    18/10/28 21:39:02 INFO mapreduce.JobSubmitter: Submitting tokens for job: job_1540728986531_0006
    18/10/28 21:39:02 INFO impl.YarnClientImpl: Submitted application application_1540728986531_0006
    18/10/28 21:39:02 INFO mapreduce.Job: The url to track the job: http://cdh-master:8088/proxy/application_1540728986531_0006/
    18/10/28 21:39:02 INFO mapreduce.Job: Running job: job_1540728986531_0006
    18/10/28 21:39:09 INFO mapreduce.Job: Job job_1540728986531_0006 running in uber mode : false
    18/10/28 21:39:09 INFO mapreduce.Job:  map 0% reduce 0%
    18/10/28 21:39:17 INFO mapreduce.Job:  map 1% reduce 0%
    18/10/28 21:39:19 INFO mapreduce.Job:  map 2% reduce 0%
    18/10/28 21:39:20 INFO mapreduce.Job:  map 3% reduce 0%
    ...
    18/10/28 21:42:50 INFO mapreduce.Job:  map 100% reduce 75%
    18/10/28 21:42:53 INFO mapreduce.Job:  map 100% reduce 80%
    18/10/28 21:42:56 INFO mapreduce.Job:  map 100% reduce 86%
    18/10/28 21:42:59 INFO mapreduce.Job:  map 100% reduce 92%
    18/10/28 21:43:02 INFO mapreduce.Job:  map 100% reduce 98%
    18/10/28 21:43:05 INFO mapreduce.Job:  map 100% reduce 100%
    ^@18/10/28 21:43:56 INFO mapreduce.Job: Job job_1540728986531_0006 completed successfully
    18/10/28 21:43:56 INFO mapreduce.Job: Counters: 54
    ...
    ```


If the tests are successful, the configurations take effect.

