# Use CDH 5 to read and write OSS data

Cloudera's Distribution Including Apache Hadoop \(CDH\) is one of the Hadoop distributions. Hadoop 3.0.0 in CDH 6.0.1 supports OSS. Hadoop 2.6 in CDH 5 does not support OSS. This topic describes how to configure CDH 5 to read and write OSS data.

A CDH 5 cluster has been deployed. For more information about how to set up a CDH 5 cluster, see [Cloudera Installation](https://www.cloudera.com/documentation/enterprise/5-14-x/topics/installation.html). This topic uses CDH 5.14.4 as an example.

CDH 5 uses an earlier version \(4.2.5\) of the httpclient and httpcore components. This is because Resource Manager requires earlier versions of these components. However, OSS SDKs require later versions of these components. To address these needs, perform the following steps:

## Step 1: Add OSS configurations

Perform the following operations on all CDH nodes:

1.  View the structure of the $\{CDH\_HOME\} CDH 5 installation directory:

    ```
    [root@cdh-master CDH-5.14.4-1.cdh5.14.4.p0.3]# ls -lh
    The total size of the files are 100 KB.
    drwxr-xr-x  2 root root 4.0K June  12 21:03 bin
    drwxr-xr-x 27 root root 4.0K June  12 20:57 etc
    drwxr-xr-x  5 root root 4.0K June  12 20:57 include
    drwxr-xr-x  2 root root  68K June  12 21:09 jars
    drwxr-xr-x 38 root root 4.0K June  12 21:03 lib
    drwxr-xr-x  3 root root 4.0K June  12 20:57 lib64
    drwxr-xr-x  3 root root 4.0K June  12 20:51 libexec
    drwxr-xr-x  2 root root 4.0K June  12 21:02 meta
    drwxr-xr-x  4 root root 4.0K June  12 21:03 share                    
    ```

    **Note:** All content in $\{\} is environment variables. Modify these environment variables.

2.  Click [here](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.14.4.tar.gz) to download the CDH 5.14.4 package that is compatible with OSS to the jars folder.

    This package adds the support for OSS based on the Hadoop version in [CDH 5.14.4](https://github.com/cloudera/hadoop-common/tree/cdh5.14.4-release). Support for OSS will be available in minor versions of CDH 5.

3.  Decompress the downloaded package.

    ```
    [root@cdh-master CDH-5.14.4-1.cdh5.14.4.p0.3]# tar -tvf hadoop-oss-cdh-5.14.4.tar.gz 
    drwxr-xr-x root/root 0 2018-10-08 18:16 hadoop-oss-cdh-5.14.4/
    -rw-r--r-- root/root 13277 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/aliyun-java-sdk-sts-3.0.0.jar
    -rw-r--r-- root/root 326724 2018-10-08 18:16 hadoop-oss-cdh-5.14.4/httpcore-4.4.4.jar
    -rw-r--r-- root/root 524927 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/aliyun-sdk-oss-3.4.1.jar
    -rw-r--r-- root/root 116337 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/aliyun-java-sdk-core-3.4.0.jar
    -rw-r--r-- root/root 215492 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/aliyun-java-sdk-ram-3.0.0.jar
    -rw-r--r-- root/root 788137 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/aliyun-java-sdk-ecs-4.2.0.jar
    -rw-r--r-- root/root 70017 2018-10-08 17:36 hadoop-oss-cdh-5.14.4/hadoop-aliyun-2.6.0-cdh5.14.4.jar
    -rw-r--r-- root/root 736658 2018-10-08 18:16 hadoop-oss-cdh-5.14.4/httpclient-4.5.2.jar
                            
    ```

4.  Go to the $\{CDH\_HOME\}/lib/hadoop directory. Run the following commands:

    ```
    [root@cdh-master hadoop]# rm -f lib/httpclient-4.2.5.jar
    [root@cdh-master hadoop]# rm -f lib/httpcore-4.2.5.jar
    [root@cdh-master hadoop]# ln -s .. /../jars/hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun-2.6.0-cdh5.14.4.jar
    [root@cdh-master hadoop]# ln -s hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun.jar
    [root@cdh-master hadoop]# cd lib
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-core-3.4.0.jar aliyun-java-sdk-core-3.4.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-ecs-4.2.0.jar aliyun-java-sdk-ecs-4.2.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-ram-3.0.0.jar aliyun-java-sdk-ram-3.0.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-sts-3.0.0.jar aliyun-java-sdk-sts-3.0.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-sdk-oss-3.4.1.jar aliyun-sdk-oss-3.4.1.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpclient-4.5.2.jar httpclient-4.5.2.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpcore-4.4.4.jar httpcore-4.4.4.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/jdom-1.1.jar jdom-1.1.jar
    ```

5.  Go to the $\{CDH\_HOME\}/lib/hadoop-yarn/bin/ directory of the Resource Manager deployment node. Replace `CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_DIR}/* CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*` in yarn with `CLASSPATH=$HADOOP_YARN_HOME/${YARN_DIR}/*:${CLASSPATH}CLASSPATH=$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*:${CLASSPATH}`.

6.  Go to the $\{CDH\_HOME\}/lib/hadoop-yarn/lib directory of the Resource Manager deployment node. Run the following commands:

    ```
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpclient-4.2.5.jar httpclient-4.2.5.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpcore-4.2.5.jar httpcore-4.2.5.jar
    ```

7.  Use cluster management tool CM to add configurations.

    If no clusters are managed by CM, modify the core-site.xml file. The following table uses CM as an example to describe the configurations to be added.

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

8.  Restart the cluster as prompted by CM.

9.  Test data reading from and writing to OSS.

    -   Test data reading from OSS:

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   Test data writing to OSS:

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        If data can be read from and written to OSS, the configurations are successful. Otherwise, check the configurations.


## Step 2: Configure OSS support for Impala

Impala can be used to query data stored in HDFS. After you configure CDH 5 to support OSS, you can use Impala to query data stored in OSS. OSS SDKs require later versions of the two components. Therefore, you must perform the following operations on all nodes where Impala is deployed:

1.  Go to the $\{CDH\_HOME\}/lib/impala/lib directory. Run the following commands:

    ```
    [root@cdh-master lib]# rm -f httpclient-4.2.5.jar httpcore-4.2.5.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpclient-4.5.2.jar httpclient-4.5.2.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpcore-4.4.4.jar httpcore-4.4.4.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-core-3.4.0.jar aliyun-java-sdk-core-3.4.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-ecs-4.2.0.jar aliyun-java-sdk-ecs-4.2.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-ram-3.0.0.jar aliyun-java-sdk-ram-3.0.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-java-sdk-sts-3.0.0.jar aliyun-java-sdk-sts-3.0.0.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-sdk-oss-3.4.1.jar aliyun-sdk-oss-3.4.1.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/jdom-1.1.jar jdom-1.1.jar
    ```

2.  Go to the $\{CDH\_HOME\}/bin directory, modify the impalad, statestored, and catalogd files. In the last line of each file, add the following content to the front of the exec command:

    ```
    export CLASSPATH=$CLASSPATH:${IMPALA_HOME}/lib/httpclient-4.5.2.jar:${IMPALA_HOME}/lib/httpcore-4.4.4.jar:${IMPALA_HOME}/lib/hadoop-aliyun.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-core-3.4.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ecs-4.2.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ram-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-sts-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-sdk-oss-3.4.1.jar:${IMPALA_HOME}/lib/jdom-1.1.jar
    ```

3.  Restart Impala-related processes on all nodes. You can then use Impala to query OSS data.


## Verify the configurations

Benchmark in TPC-DS has a table named call\_center. Assume that this table is stored in OSS. You can create an external table that maps to call\_center and query the number of records based on cc\_country.

```
[root@cdh-master ~]# impala-shell -i cdh-slave01:21000
Starting Impala Shell without Kerberos authentication
Connected to cdh-slave01:21000
Server version: impalad version 2.11.0-cdh5.14.4 RELEASE (build20e635646a13347800fad36a7d0b1da25ab32404)
***********************************************************************************
Welcome to the Impala shell.
(Impala Shell v2.11.0-cdh5.14.4 (20e6356) built on Tue Jun 1203:43:08 PDT 2018)

The HISTORY command lists all shell commands in chronological order.
***********************************************************************************
[cdh-slave01:21000] > droptableifexists call_center;
Query: droptableifexists call_center
[cdh-slave01:21000] >
[cdh-slave01:21000] > createexternaltable call_center(
                    >       cc_call_center_sk         bigint
                    > ,     cc_call_center_id         string
                    > ,     cc_rec_start_date        string
                    > ,     cc_rec_end_date          string
                    > ,     cc_closed_date_sk         bigint
                    > ,     cc_open_date_sk           bigint
                    > ,     cc_name                   string
                    > ,     cc_class                  string
                    > ,     cc_employees              int
                    > ,     cc_sq_ft                  int
                    > ,     cc_hours                  string
                    > ,     cc_manager                string
                    > ,     cc_mkt_id                 int
                    > ,     cc_mkt_class              string
                    > ,     cc_mkt_desc               string
                    > ,     cc_market_manager         string
                    > ,     cc_division               int
                    > ,     cc_division_name          string
                    > ,     cc_company                int
                    > ,     cc_company_name           string
                    > ,     cc_street_number          string
                    > ,     cc_street_name            string
                    > ,     cc_street_type            string
                    > ,     cc_suite_number           string
                    > ,     cc_city                   string
                    > ,     cc_county                 string
                    > ,     cc_state                  string
                    > ,     cc_zip                    string
                    > ,     cc_country                string
                    > ,     cc_gmt_offset             double
                    > ,     cc_tax_percentage         double
                    > )
                    > rowformatdelimitedfieldsterminatedby'|'
                    > location 'oss://${your-bucket-name}/call_center';
Query: createexternaltable call_center(
      cc_call_center_sk         bigint
,     cc_call_center_id         string
,     cc_rec_start_date        string
,     cc_rec_end_date          string
,     cc_closed_date_sk         bigint
,     cc_open_date_sk           bigint
,     cc_name                   string
,     cc_class                  string
,     cc_employees              int
,     cc_sq_ft                  int
,     cc_hours                  string
,     cc_manager                string
,     cc_mkt_id                 int
,     cc_mkt_class              string
,     cc_mkt_desc               string
,     cc_market_manager         string
,     cc_division               int
,     cc_division_name          string
,     cc_company                int
,     cc_company_name           string
,     cc_street_number          string
,     cc_street_name            string
,     cc_street_type            string
,     cc_suite_number           string
,     cc_city                   string
,     cc_county                 string
,     cc_state                  string
,     cc_zip                    string
,     cc_country                string
,     cc_gmt_offset             double
,     cc_tax_percentage         double
)
rowformatdelimitedfieldsterminatedby'|'
location 'oss://${your-bucket-name}/call_center'
Fetched 0row(s) in0.07s

[cdh-slave01:21000] > select cc_country, count(*) from call_center groupby cc_country;
Query: select cc_country, count(*) from call_center groupby cc_country
Query submitted at: 2018-10-2816:21:13 (Coordinator: http://cdh-slave01:25000)
Query progress can be monitored at: http://cdh-slave01:25000/query_plan?query_id=fb4e09977145f367:3bdfe4d600000000
+---------------+----------+
| cc_country    | count(*) |
+---------------+----------+
| United States | 30       |
+---------------+----------+
Fetched 1 row(s) in 4.71s
```

## Reference

For more information about Hadoop, visit [Hadoop supports OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK).

You can also access OSS through Alibaba Cloud E-MapReduce. EMR is an all-in-one enterprise-ready big data platform that provides cluster, job, and data management services based on the open source ecosystem. The ecosystem includes Hadoop, Spark, Kafka, Flink, and Storm. EMR provides seamless support for OSS. EMR is combined with OSS to provide access to OSS through the open source ecosystem and optimized technologies. For more information, see [What is E-MapReduce?](/intl.en-US/Product Introduction/What is E-MapReduce?.md).

