# Use CDH 5 to read and write OSS data

Cloudera's Distribution Including Apache Hadoop \(CDH\) is one of the Hadoop distributions. OSS is supported by Hadoop 3.0.0 in CDH 6.0.1 but not supported by Hadoop 2.6 in CDH 5. This topic describes how to configure CDH 5 to read and write OSS data.

A CDH 5 cluster is deployed. In this topic, CDH 5.14.4 is used as an example. For more information about how to build a CDH 5 cluster, visit [Cloudera Installation Guide](https://www.cloudera.com/documentation/enterprise/5-14-x/topics/installation.html).

CDH 5 uses an earlier version \(4.2.5\) of the httpclient and httpcore components based on the requirements of Resource Manager. However, OSS SDKs require later versions of the two components. To meet the requirements of OSS SDKs, perform the following steps:

## Step 1: Add OSS configurations

Perform the following operations on all CDH nodes:

1.  View the structure of the $\{CDH\_HOME\} directory in which CDH 5 is installed.

    ```
    [root@cdh-master CDH-5.14.4-1.cdh5.14.4.p0.3]# ls -lh
    The total size of the files is 100 KB.
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

    **Note:** In this topic, all contents enclosed with $\{\} are environment variables. Modify the environment variables based on the actual environment.

2.  Click [here](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.14.4.tar.gz) to download the CDH 5.14.4 package that is compatible with OSS to the jars folder in the installation directory of CDH 5.

    This package is embedded with the patch provided by Apache Hadoop for OSS based on the Hadoop version of [CDH 5.14.4](https://github.com/cloudera/hadoop-common/tree/cdh5.14.4-release). To download other versions of the package, access the following links:

    -   [CDH 5.8.5](https://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.8.5.tar.gz)
    -   [CDH 5.4.4](https://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.4.4.tar.gz)
    -   [CDH 6.3.2](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-6.3.2.tar.gz)

        **Note:** For CDH 6.3.2, you must copy the files of the package to the jars folder of the installation directory of CDH and then perform the following steps to update aliyun-sdk-oss-3.4.1.jar and link the aliyun-java-sdk-\*.jar files to corresponding locations.

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

5.  Go to the $\{CDH\_HOME\}/lib/hadoop-yarn/bin/ directory on the node on which Resource Manager is deployed. Replace `CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_DIR}/* CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*` in the yarn file with `CLASSPATH=$HADOOP_YARN_HOME/${YARN_DIR}/*:${CLASSPATH}CLASSPATH=$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*:${CLASSPATH}`.

6.  Go to the $\{CDH\_HOME\}/lib/hadoop-yarn/lib directory on the node on which Resource Manager is deployed. Run the following commands:

    ```
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpclient-4.2.5.jar httpclient-4.2.5.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/httpcore-4.2.5.jar httpcore-4.2.5.jar
    ```

7.  Use the cluster management tool CM to add configurations.

    If no clusters are managed by CM, modify the core-site.xml file. The following table describes the parameters that you can configure in CM for an example.

    |Parameter|Configuration method|
    |:--------|:-------------------|
    |**fs.oss.endpoint**|Specify the endpoint used to access the region where the bucket is located. For example, if the bucket is in the China \(Hangzhou\) region, set the endpoint to `oss-cn-hangzhou.aliyuncs.com`. For more information about the endpoints of other regions, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md). |
    |**fs.oss.accessKeyId**|Specify the AccessKey ID used to access OSS. For more information about how to obtain the AccessKey ID, see [Create an AccessKey pair]()|
    |**fs.oss.accessKeySecret**|Specify the AccessKey secret used to access OSS. For more information about how to obtain the AccessKey secret, see [Create an AccessKey pair]().|
    |**fs.oss.impl**|Specify the class used to implement the OSS file system based on Hadoop. Set this parameter to `org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem`.|
    |**fs.oss.buffer.dir**|Specify the name of the directory used to store temporary files. We recommend that you set this parameter to `/tmp/oss`. |
    |**fs.oss.connection.secure.enabled**|Specify whether to enable HTTPS. If HTTPS is enabled, the performance of the system degrades. We recommend that you set this parameter to false. |
    |**fs.oss.connection.maximum**|Specify the maximum number of connections to OSS. We recommend that you set this parameter to 2048. |

    For more information about the parameters, visit [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md).

8.  Restart the cluster as prompted.

9.  Test reading data from and writing data to OSS.

    -   Run the following command to test reading data from OSS:

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   Run the following command to test writing data to OSS:

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        If test data can be read from and written to OSS, it indicates that the configurations are correct. Otherwise, check your configurations.


## Step 2: Configure OSS support for Impala

Impala can be used to query data stored in Hadoop Distributed File System \(HDFS\). After you configure CDH 5 to support OSS, you can use Impala to query data stored in OSS. OSS SDKs require later versions of the httpclient and httpcore components. Therefore, you must perform the following steps on the node on which Impala is deployed:

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

2.  Go to the $\{CDH\_HOME\}/bin directory, add the following content before the exec command in the last line of the impalad, statestored, and catalogd files.

    ```
    export CLASSPATH=$CLASSPATH:${IMPALA_HOME}/lib/httpclient-4.5.2.jar:${IMPALA_HOME}/lib/httpcore-4.4.4.jar:${IMPALA_HOME}/lib/hadoop-aliyun.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-core-3.4.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ecs-4.2.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ram-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-sts-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-sdk-oss-3.4.1.jar:${IMPALA_HOME}/lib/jdom-1.1.jar
    ```

3.  Restart Impala-related processes on all nodes.

    You can then use Impala to query OSS data.


## Verify configurations

The benchmark of TPC-DS uses a table named call\_center. If this table is stored in OSS, you can create an external table that maps to call\_center and query the number of records in the table based on cc\_country.

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

## References

For more information about Hadoop, visit [Hadoop supports OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK).

