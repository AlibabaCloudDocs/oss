# Back up an HDFS to OSS for disaster tolerance {#concept_l42_mnw_5db .concept}

## Background {#section_e1j_xnw_5db .section}

Currently, many data centers are constructed using Hadoop, and in turn an increasing number of enterprises want to smoothly migrate their services to the cloud.

Object Storage Service \(OSS\) is the most widely-used storage service on Alibaba Cloud. The OSS data migration tool, ossimport2, allows you to sync files from your local devices or a third-party cloud storage service to OSS. However, ossimport2 cannot read data from Hadoop file systems. As a result, it becomes impossible to make full use of the distributed structure of Hadoop. In addition, this tool only supports local files. Therefore, you must first download files from your Hadoop file system \(HDFS\) to your local device and then upload them using the tool. This process consumes a great deal of time and energy.

To solve this problem, Alibaba Cloud’s E-MapReduce team developed a Hadoop data migration tool **emr-tools**. This tool allows you to migrate data from Hadoop directly to OSS.

This chapter introduces how to quickly migrate data from HDFS to OSS.

## Prerequisites {#section_mds_ynw_5db .section}

Make sure your current machine can access your Hadoop cluster. That is, you must be able to use Hadoop commands to access HDFS.

```
hadoop fs -ls /
```

## Migrate Hadoop data to OSS {#section_ghz_j4w_5db .section}

1.  Download [emr-tools](https://yq.aliyun.com/attachment/download/?spm=5176.100239.blogcont78093.18.BfNz7d&id=1956).

    **Note:** emr-tools is compatible with Hadoop versions 2.4.x, 2.5.x, 2.6.x, and 2.7.x. If you require compatibility with other Hadoop versions, [open a ticket](https://selfservice.console.aliyun.com/ticket/createIndex).

2.  Extract the compressed tool to a local directory.

    ```
    tar jxf emr-tools.tar.bz2
    ```

3.  Copy HDFS data to OSS.

    ```
    cd emr-tools
    ./hdfs2oss4emr.sh /path/on/hdfs oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss
    ```

    The relevant parameters are described as follow.

    |Parameters|Description|
    |:---------|:----------|
    |accessKeyId|The key used to access OSS APIs.For more information, see [How to obtain AccessKeyId and AccessKeySecret](https://www.alibabacloud.com/help/doc-detail/48699.htm).

|
    |accessKeySecret|
    |bucket-name.oss-cn-hangzhou.aliyuncs.com|The OSS access domain name, including the bucket name and endpoint address.|

    The system enables a Hadoop MapReduce task \(DistCp\).

4.  After the task is completed, local data migration information is displayed. This information is similar to the following sample.

    ```
    17/05/04 22:35:08 INFO mapreduce.Job: Job job_1493800598643_0009 completed successfully
    17/05/04 22:35:08 INFO mapreduce.Job: Counters: 38
     File System Counters
             FILE: Number of bytes read=0
             FILE: Number of bytes written=859530
             FILE: Number of read operations=0
             FILE: Number of large read operations=0
             FILE: Number of write operations=0
             HDFS: Number of bytes read=263114
             HDFS: Number of bytes written=0
             HDFS: Number of read operations=70
             HDFS: Number of large read operations=0
             HDFS: Number of write operations=14
             OSS: Number of bytes read=0
             OSS: Number of bytes written=258660
             OSS: Number of read operations=0
             OSS: Number of large read operations=0
             OSS: Number of write operations=0
     Job Counters
             Launched map tasks=7
             Other local map tasks=7
             Total time spent by all maps in occupied slots (ms)=60020
             Total time spent by all reduces in occupied slots (ms)=0
             Total time spent by all map tasks (ms)=30010
             Total vcore-milliseconds taken by all map tasks=30010
             Total megabyte-milliseconds taken by all map tasks=45015000
     Map-Reduce Framework
             Map input records=10
             Map output records=0
             Input split bytes=952
             Spilled Records=0
             Failed Shuffles=0
             Merged Map outputs=0
             GC time elapsed (ms)=542
             CPU time spent (ms)=14290
             Physical memory (bytes) snapshot=1562365952
             Virtual memory (bytes) snapshot=17317421056
             Total committed heap usage (bytes)=1167589376
     File Input Format Counters
             Bytes Read=3502
     File Output Format Counters
             Bytes Written=0
     org.apache.hadoop.tools.mapred.CopyMapper$Counter
             BYTESCOPIED=258660
             BYTESEXPECTED=258660
             COPY=10
    copy from /path/on/hdfs to oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss does succeed !!!
    ```

5.  You can use osscmd to view information about OSS data.

    ```
    osscmd ls oss://bucket-name/path/on/oss
    ```


## Migrate OSS data to Hadoop {#section_kzs_sqw_5db .section}

If you have already created a Hadoop cluster on Alibaba Cloud, you can use the following command to migrate data from OSS to the new Hadoop cluster.

```
./hdfs2oss4emr.sh oss://accessKeyId:accessKeySecret@bucket-name.oss-cn-hangzhou.aliyuncs.com/path/on/oss /path/on/new-hdfs
```

## More scenarios {#section_swh_wqw_5db .section}

In addition to offline clusters, you can also use emr-tools for Hadoop clusters constructed on ECS. This allows you to quickly migrate a self-built cluster to the [E-MapReduce](https://www.aliyun.com/product/emapreduce?) service.

If your cluster is already on ECS, but in a classic network, it will not provide good interoperability with services in Virtual Private Cloud \(VPC\). In this case, migrate the cluster to a VPC instance. Follow these steps to migrate the cluster:

1.  Use emr-tools to migrate data to OSS.
2.  Create a new cluster \(create it yourself or use E-MapReduce\) in the VPC environment.
3.  Migrate data from OSS to the new HDFS cluster.

If you use E-MapReduce, on the Hadoop cluster, you can directly access OSS using [Spark](https://www.alibabacloud.com/help/zh/doc-detail/28118.htm), [MapReduce](https://www.alibabacloud.com/help/doc-detail/28128.htm) and [Hive](https://www.alibabacloud.com/help/doc-detail/28129.htm). This not only avoids one data copy operation \(from OSS to HDFS\), but also greatly reduces storage costs. For more information about cost reduction, see [EMR+OSS: Separated storage and computing](intl.en-US/Best Practices/Data processing and analysis/EMR+OSS: Separated storage and computing for offline computing.md#).

