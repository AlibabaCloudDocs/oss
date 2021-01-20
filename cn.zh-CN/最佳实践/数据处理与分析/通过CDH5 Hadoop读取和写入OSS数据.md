# 通过CDH5 Hadoop读取和写入OSS数据

CDH（Cloudera's Distribution, including Apache Hadoop）是众多Hadoop发行版本中的一种，最新版本CDH6.0.1中的Hadoop3.0.0版本已经支持OSS，但CDH5中的Hadoop2.6版本不支持OSS。本文介绍如何配置CDH5支持OSS读写。

拥有一个已搭建好的CDH5集群（本文以CDH 5.14.4版本为例）。如何搭建CDH5集群，请参见[官方文档](https://www.cloudera.com/documentation/enterprise/5-14-x/topics/installation.html)。

由于CDH5的httpclient和httpcore这两个组件版本较低（4.2.5），Resource Manager要求的httpclient和httpcore必须是低版本，而OSS SDK要求这两个组件的版本较高。因此，本文提供了一个workaround方案。

## 步骤一：增加OSS配置

您需要在所有的CDH节点执行以下操作：

1.  查看CDH5安装目录$\{CDH\_HOME\}的结构：

    ```
    [root@cdh-master CDH-5.14.4-1.cdh5.14.4.p0.3]# ls -lh
    总用量 100K
    drwxr-xr-x  2 root root 4.0K 6月  12 21:03 bin
    drwxr-xr-x 27 root root 4.0K 6月  12 20:57 etc
    drwxr-xr-x  5 root root 4.0K 6月  12 20:57 include
    drwxr-xr-x  2 root root  68K 6月  12 21:09 jars
    drwxr-xr-x 38 root root 4.0K 6月  12 21:03 lib
    drwxr-xr-x  3 root root 4.0K 6月  12 20:57 lib64
    drwxr-xr-x  3 root root 4.0K 6月  12 20:51 libexec
    drwxr-xr-x  2 root root 4.0K 6月  12 21:02 meta
    drwxr-xr-x  4 root root 4.0K 6月  12 21:03 share                    
    ```

    **说明：** 本文中所有$\{\}的内容为环境变量，请根据您实际的环境修改。

2.  下载[CDH 5.14.4版本支持OSS的支持包](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.14.4.tar.gz)至CDH5的安装目录中的jars文件夹中。

    该支持包是根据[CDH 5.14.4](https://github.com/cloudera/hadoop-common/tree/cdh5.14.4-release)中Hadoop的版本，并增加Apache Hadoop对OSS支持的补丁后编译得到的。其他版本支持包下载地址，请参见：

    -   [CDH 5.8.5](https://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.8.5.tar.gz)
    -   [CDH 5.4.4](https://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-5.4.4.tar.gz)
    -   [CDH 6.3.2](http://gosspublic.alicdn.com/hadoop-spark/hadoop-oss-cdh-6.3.2.tar.gz)

        **说明：** 对于CDH 6.3.2版本，您需要将支持包的文件复制到CDH6安装目录的jars文件夹中，然后参考以下步骤进行部署（主要更新aliyun-sdk-oss-3.4.1.jar以及将aliyun-java-sdk-\*.jar符号链接到对应的位置）。

3.  解压支持包。

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

4.  进入$\{CDH\_HOME\}/lib/hadoop目录，执行如下命令：

    ```
    [root@cdh-master hadoop]# rm -f lib/httpclient-4.2.5.jar
    [root@cdh-master hadoop]# rm -f lib/httpcore-4.2.5.jar
    [root@cdh-master hadoop]# ln -s ../../jars/hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun-2.6.0-cdh5.14.4.jar
    [root@cdh-master hadoop]# ln -s hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun.jar
    [root@cdh-master hadoop]# cd lib
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-core-3.4.0.jar aliyun-java-sdk-core-3.4.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-ecs-4.2.0.jar aliyun-java-sdk-ecs-4.2.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-ram-3.0.0.jar aliyun-java-sdk-ram-3.0.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-sts-3.0.0.jar aliyun-java-sdk-sts-3.0.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-sdk-oss-3.4.1.jar aliyun-sdk-oss-3.4.1.jar
    [root@cdh-master lib]# ln -s ../../../jars/httpclient-4.5.2.jar httpclient-4.5.2.jar
    [root@cdh-master lib]# ln -s ../../../jars/httpcore-4.4.4.jar httpcore-4.4.4.jar
    [root@cdh-master lib]# ln -s ../../../jars/jdom-1.1.jar jdom-1.1.jar
    ```

5.  进入Resurce Manager部署节点的$\{CDH\_HOME\}/lib/hadoop-yarn/bin/目录，将yarn中的`CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_DIR}/* CLASSPATH=${CLASSPATH}:$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*`替换为`CLASSPATH=$HADOOP_YARN_HOME/${YARN_DIR}/*:${CLASSPATH}CLASSPATH=$HADOOP_YARN_HOME/${YARN_LIB_JARS_DIR}/*:${CLASSPATH}`。

6.  进入到Resurce Manager部署节点的$\{CDH\_HOME\}/lib/hadoop-yarn/lib目录，执行如下命令：

    ```
    [root@cdh-master lib]# ln -s ../../../jars/httpclient-4.2.5.jar httpclient-4.2.5.jar
    [root@cdh-master lib]# ln -s ../../../jars/httpcore-4.2.5.jar httpcore-4.2.5.jar
    ```

7.  通过集群管理工具CM增加配置。

    若没有CM管理的集群，可以修改core-site.xml。以CM为例，您需要增加如下配置：

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6454449951/p33147.png)

    |配置项|值|
    |:--|:-|
    |**fs.oss.endpoint**|填写需要连接的OSS的Endpoint。 例如华东1（杭州）地址的Endpoint为`oss-cn-hangzhou.aliyuncs.com`。更多地域的Endpoint信息，请参见[访问域名和数据中心](/cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md)。 |
    |**fs.oss.accessKeyId**|填写OSS的AccessKey ID。AccessKey的获取方式，请参见[创建AccessKey]()。|
    |**fs.oss.accessKeySecret**|填写OSS的AccessKey Secret。AccessKey的获取方式，请参见[创建AccessKey]()。|
    |**fs.oss.impl**|Hadoop OSS文件系统实现类。目前固定为`org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem`。|
    |**fs.oss.buffer.dir**|填写临时文件目录。 建议值：`/tmp/oss` |
    |**fs.oss.connection.secure.enabled**|是否开启HTTPS，开启HTTPS会影响性能。 建议值：false |
    |**fs.oss.connection.maximum**|与OSS的连接数。 建议值：2048 |

    更多参数详情，请参见 [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md)。

8.  根据CM提示重启集群。

9.  测试读写OSS。

    -   测试读

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   测试写

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        若测试可以读写OSS，则配置成功；若无法读写OSS，请检查配置。


## 步骤二：配置Impala对OSS的支持

Impala可以直接查询存储在HDFS的数据，在CDH5支持OSS后，就可以直接查询存储在OSS的数据。OSS SDK要求这两个组件的版本较高，所以需要在所有部署Impala的节点执行以下操作：

1.  进入$\{CDH\_HOME\}/lib/impala/lib，执行如下命令：

    ```
    [root@cdh-master lib]# rm -f httpclient-4.2.5.jar httpcore-4.2.5.jar
    [root@cdh-master lib]# ln -s ../../../jars/httpclient-4.5.2.jar httpclient-4.5.2.jar
    [root@cdh-master lib]# ln -s ../../../jars/httpcore-4.4.4.jar httpcore-4.4.4.jar
    [root@cdh-master lib]# ln -s ../../../jars/hadoop-aliyun-2.6.0-cdh5.14.4.jar hadoop-aliyun.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-core-3.4.0.jar aliyun-java-sdk-core-3.4.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-ecs-4.2.0.jar aliyun-java-sdk-ecs-4.2.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-ram-3.0.0.jar aliyun-java-sdk-ram-3.0.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-java-sdk-sts-3.0.0.jar aliyun-java-sdk-sts-3.0.0.jar
    [root@cdh-master lib]# ln -s ../../../jars/aliyun-sdk-oss-3.4.1.jar aliyun-sdk-oss-3.4.1.jar
    [root@cdh-master lib]# ln -s ../../../jars/jdom-1.1.jar jdom-1.1.jar
    ```

2.  进入$\{CDH\_HOME\}/bin目录，修改impalad、statestored、catalogd三个文件，在文件最后一行的exec命令前，增加如下内容：

    ```
    export CLASSPATH=$CLASSPATH:${IMPALA_HOME}/lib/httpclient-4.5.2.jar:${IMPALA_HOME}/lib/httpcore-4.4.4.jar:${IMPALA_HOME}/lib/hadoop-aliyun.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-core-3.4.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ecs-4.2.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-ram-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-java-sdk-sts-3.0.0.jar:${IMPALA_HOME}/lib/aliyun-sdk-oss-3.4.1.jar:${IMPALA_HOME}/lib/jdom-1.1.jar
    ```

3.  重启所有节点的impala相关进程。

    重启完成后即可使用impala查询OSS数据。


## 验证配置

TPC-DS的benchmark有一张表为call\_center，假设该表存储在OSS中，我们可以创建一个外部表指向它，并且查询这张表根据cc\_country分组分别有多少条记录。

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

## 更多参考

关于Hadoop更多内容，请参见[Hadoop支持集成OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK)。

