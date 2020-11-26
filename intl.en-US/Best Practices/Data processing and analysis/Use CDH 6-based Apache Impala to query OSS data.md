# Use CDH 6-based Apache Impala to query OSS data

Cloudera's Distribution Including Apache Hadoop \(CDH\) is one of the Hadoop distributions. Hadoop 3.0.0 in CDH 6.0.1 supports OSS. This topic describes how to enable CDH 6 components such as Hadoop, Hive, Spark, and Impala to query OSS data.

A CDH 6 cluster is set up. For more information about how to set up a CDH 6 cluster, visit [Cloudera Installation Guide](https://www.cloudera.com/documentation/enterprise/6/6.0/topics/installation.html). This topic uses CDH 6.0.1 as an example.

## Step 1: Add OSS configurations

1.  Use cluster management tool CM to add configurations.

    If no clusters are managed by CM, modify the core-site.xml file. The following table uses CM as an example to describe the configurations to be added.

    |Parameter|Configuration method|
    |:--------|:-------------------|
    |**fs.oss.endpoint**|Enter the endpoint used to access the region where the bucket is located. Example: oss-cn-zhangjiakou-internal.aliyuncs.com. |
    |**fs.oss.accessKeyId**|Enter the AccessKey ID used to access OSS.|
    |**fs.oss.accessKeySecret**|Enter the AccessKey secret used to access OSS.|
    |**fs.oss.impl**|Enter the class used to implement the OSS file system by using Hadoop. Set the value to org.apache.hadoop.fs.aliyun.oss.AliyunOSSFileSystem.|
    |**fs.oss.buffer.dir**|Enter the temporary file directory. We recommend that you set this parameter to /tmp/oss. |
    |**fs.oss.connection.secure.enabled**|Specify whether to enable HTTPS. Performance may be affected when HTTPS is enabled. We recommend that you set this parameter to false. |
    |**fs.oss.connection.maximum**|Enter the maximum number of connections to OSS. We recommend that you set this parameter to 2048. |

    For more information about parameter descriptions, visit [Hadoop-Aliyun module](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md).

2.  Restart the cluster as prompted by CM.

3.  Test reads and writes on OSS data

    -   Test data reading from OSS:

        ```
        hadoop fs -ls oss://${your-bucket-name}/
        ```

    -   Test data writing to OSS:

        ```
        hadoop fs -mkdir oss://${your-bucket-name}/hadoop-test
        ```

        If data can be read from and written to OSS, the configurations are successful. Otherwise, check configurations.

        **Note:** All content in $\{\} is environment variables. You can modify these environment variables.


## Step 2: Configure OSS support for Impala

By default, CDH 6 supports OSS. However the OSS-compliant package is not included in CLASSPATH of the Impala component. To add this package to CLASSPATH, perform the following operations on all Impala nodes:

1.  Go to the $\{CDH\_HOME\}/lib/impala directory. Create a symbolic link:

    ```
    [root@cdh-master impala]# cd lib/
    [root@cdh-master lib]# ln -s .. /.. /../jars/hadoop-aliyun-3.0.0-cdh6.0.1.jar hadoop-aliyun.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/aliyun-sdk-oss-2.8.3.jar aliyun-sdk-oss-2.8.3.jar
    [root@cdh-master lib]# ln -s .. /.. /../jars/jdom-1.1.jar jdom-1.1.jar
    ```

2.  Go to the $\{CDH\_HOME\}/bin directory, modify the impalad, statestored, and catalogd files. In the last line of each file, add the following content to the front of the exec command:

    ```
    export CLASSPATH=${CLASSPATH}:${IMPALA_HOME}/lib/hadoop-aliyun.jar:${IMPALA_HOME}/lib/aliyun-sdk-oss-2.8.3.jar:${IMPALA_HOME}/lib/jdom-1.1.jar
    ```

3.  Restart Impala-related processes on all nodes.


## Verify the configurations

Use Impala to query OSS data. Run query statements derived from TPC-DS. For more information, visit [Apache Impala](http://impala.apache.org/) and TPC-DS queries in [hive-testbench](https://github.com/hortonworks/hive-testbench/tree/hive14).

TPC-DS is based on HiveQL, which is compatible with Impala SQL. For compatibility reasons, this topic uses HiveQL as an example.

1.  Generate sample data and store it in OSS.

    ```
    [root@cdh-master ~]# git clone https://github.com/hortonworks/hive-testbench.git
    [root@cdh-master ~]# cd hive-testbench
    [root@cdh-master hive-testbench]# git checkout hive14
    [root@cdh-master hive-testbench]# ./tpcds-build.sh
    [root@cdh-master hive-testbench]# FORMAT=textfile ./tpcds-setup.sh 50 oss://{your-bucket-name}/
    ```

2.  Create a table.

    The statement used to create a table by using the TPC-DS benchmark is compatible with that by using Impala. Copy the statement in the ddl-tpcds/text/alltables.sql file. Modify $\{LOCATION\}. Example:

    ```
    [root@cdh-master hive-testbench]# impala-shell -i cdh-slave01 -d default
    Starting Impala Shell without Kerberos authentication
    Connected to cdh-slave01:21000
    Server version: impalad version 3.0.0-cdh6.0.1 RELEASE(build9a74a5053de5f7b8dd983802e6d75e58d31472db)
    ***********************************************************************************
    Welcome to the Impala shell.
    (Impala Shell v3.0.0-cdh6.0.1(9a74a50) built on Wed Sep 1911:27:37 PDT 2018)
    
    Want to know what version of Impala you're connected to? Run the VERSION command to
    find out!
    ***********************************************************************************
    Query: use `default`
    [cdh-slave01:21000] default> create external table call_center(
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
                               > row format delimited fields terminated by '|'
                               > location 'oss://{your-bucket-name}/50/call_center';
    Query: create external table call_center(
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
    row format delimited fields terminated by '|'
    location 'oss://{your-bucket-name}/50/call_center'
    +-------------------------+
    | summary                 |
    +-------------------------+
    | Table has been created. |
    +-------------------------+
    Fetched 1 row(s) in 4.10s
    ```

3.  Check the table content.

    ```
    [cdh-slave01:21000] default> show tables;
    Query: show tables
    +------------------------+
    | name                   |
    +------------------------+
    | call_center            |
    | catalog_page           |
    | catalog_returns        |
    | catalog_sales          |
    | customer               |
    | customer_address       |
    | customer_demographics  |
    | date_dim               |
    | household_demographics |
    | income_band            |
    | inventory              |
    | item                   |
    | promotion              |
    | reason                 |
    | ship_mode              |
    | store                  |
    | store_returns          |
    | store_sales            |
    | time_dim               |
    | warehouse              |
    | web_page               |
    | web_returns            |
    | web_sales              |
    | web_site               |
    +------------------------+
    Fetched 24 row(s) in 0.03s
    ```

4.  Run a TPC-DS query on Impala. You can query OSS data.

    1.  Go to the sample-queries-tpcds directory that contains SQL files.

        ```
        [root@cdh-master hive-testbench]# ls sample-queries-tpcds
        query12.sql  query20.sql  query27.sql  query39.sql  query46.sql  query54.sql  query64.sql  query71.sql  query7.sql   query87.sql  query93.sql  README.md
        query13.sql  query21.sql  query28.sql  query3.sql   query48.sql  query55.sql  query65.sql  query72.sql  query80.sql  query88.sql  query94.sql  testbench.settings
        query15.sql  query22.sql  query29.sql  query40.sql  query49.sql  query56.sql  query66.sql  query73.sql  query82.sql  query89.sql  query95.sql  testbench-withATS.settings
        query17.sql  query24.sql  query31.sql  query42.sql  query50.sql  query58.sql  query67.sql  query75.sql  query83.sql  query90.sql  query96.sql
        query18.sql  query25.sql  query32.sql  query43.sql  query51.sql  query60.sql  query68.sql  query76.sql  query84.sql  query91.sql  query97.sql
        query19.sql  query26.sql  query34.sql  query45.sql  query52.sql  query63.sql  query70.sql  query79.sql  query85.sql  query92.sql  query98.sql
        ```

    2.  Run query13.sql.

        ```
        [root@cdh-master hive-testbench]# impala-shell -i cdh-slave01 -d default -f sample-queries-tpcds/query13.sql
        Starting Impala Shell without Kerberos authentication
        Connected to cdh-slave01:21000
        Server version: impalad version 3.0.0-cdh6.0.1 RELEASE(build9a74a5053de5f7b8dd983802e6d75e58d31472db)
        Query: use`default`Query: selectavg(ss_quantity)
               ,avg(ss_ext_sales_price)
               ,avg(ss_ext_wholesale_cost)
               ,sum(ss_ext_wholesale_cost)
         from store_sales
             ,store
             ,customer_demographics
             ,household_demographics
             ,customer_address
             ,date_dim
         where store.s_store_sk = store_sales.ss_store_sk
         and  store_sales.ss_sold_date_sk = date_dim.d_date_sk and date_dim.d_year = 2001and((store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = store_sales.ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'M'and customer_demographics.cd_education_status = '4 yr Degree'and store_sales.ss_sales_price between100.00and150.00and household_demographics.hd_dep_count = 3
             )or
            (store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = store_sales.ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'D'and customer_demographics.cd_education_status = 'Primary'and store_sales.ss_sales_price between50.00and100.00and household_demographics.hd_dep_count = 1
             ) or
            (store_sales.ss_hdemo_sk=household_demographics.hd_demo_sk
          and customer_demographics.cd_demo_sk = ss_cdemo_sk
          and customer_demographics.cd_marital_status = 'U'and customer_demographics.cd_education_status = 'Advanced Degree'and store_sales.ss_sales_price between150.00and200.00and household_demographics.hd_dep_count = 1
             ))
         and((store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('KY', 'GA', 'NM')
          and store_sales.ss_net_profit between100and200
             ) or
            (store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('MT', 'OR', 'IN')
          and store_sales.ss_net_profit between150and300
             ) or
            (store_sales.ss_addr_sk = customer_address.ca_address_sk
          and customer_address.ca_country = 'United States'and customer_address.ca_state in('WI', 'MO', 'WV')
          and store_sales.ss_net_profit between50and250
             ))
        Query submitted at: 2018-10-3011:44:47(Coordinator: http://cdh-slave01:25000)
        Query progress can be monitored at: http://cdh-slave01:25000/query_plan?query_id=ff4b3157eddfc3c4:8a31c6500000000
        +-------------------+-------------------------+----------------------------+----------------------------+
        | avg(ss_quantity)  | avg(ss_ext_sales_price) | avg(ss_ext_wholesale_cost) | sum(ss_ext_wholesale_cost) |
        +-------------------+-------------------------+----------------------------+----------------------------+
        | 30.87106918238994 | 2352.642327044025       | 2162.600911949685          | 687707.09                  |
        +-------------------+-------------------------+----------------------------+----------------------------+
        Fetched 1 row(s) in 353.16s
        ```

    The following tables are involved in this query: store, store\_sales, customer\_demographics, household\_demographics, customer\_address, and date\_dim.

    ```
    [cdh-slave01:21000] default> showtable STATS store;
    Query: showtable STATS store
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                  |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    | -1    | 1      | 37.56KB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/store |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS store_sales;
    Query: showtable STATS store_sales
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                        |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    | -1    | 50     | 18.75GB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/store_sales |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS customer_demographics;
    Query: showtable STATS customer_demographics
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                                  |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    | -1    | 50     | 76.92MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/customer_demographics |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+-----------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS household_demographics;
    Query: showtable STATS household_demographics
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    | #Rows | #Files | Size     | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                                   |
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    | -1    | 1      | 148.10KB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/household_demographics |
    +-------+--------+----------+--------------+-------------------+--------+-------------------+------------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS customer_address;
    Query: showtable STATS customer_address
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    | #Rows | #Files | Size    | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                             |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    | -1    | 1      | 40.54MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/customer_address |
    +-------+--------+---------+--------------+-------------------+--------+-------------------+------------------------------------------------------+
    Fetched 1 row(s) in 0.01s
    [cdh-slave01:21000] default> showtable STATS date_dim;
    Query: showtable STATS date_dim
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    | #Rows | #Files | Size   | Bytes Cached | CacheReplication | Format | Incremental stats | Location                                     |
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    | -1    | 1      | 9.84MB | NOT CACHED   | NOT CACHED        | TEXT   | false             | oss://{your-bucket-name}/50/date_dim |
    +-------+--------+--------+--------------+-------------------+--------+-------------------+----------------------------------------------+
    Fetched 1 row(s) in 0.01s
    ```


## References

For more information, visit the following websites and see the following topics:

-   [Hadoop supports OSS](https://yq.aliyun.com/articles/292792?spm=a2c4e.11155435.0.0.7ccba82fbDwfhK)
-   [Hadoop](https://github.com/apache/hadoop/blob/trunk/hadoop-tools/hadoop-aliyun/src/site/markdown/tools/hadoop-aliyun/index.md)
-   [Use CDH 5 to read and write OSS data](/intl.en-US/Best Practices/Data processing and analysis/Use CDH 5 to read and write OSS data.md)
-   [Use HDP 2.6-based Hadoop to read and write OSS data](/intl.en-US/Best Practices/Data processing and analysis/Use HDP 2.6-based Hadoop to read and write OSS data.md)

You can also access OSS by using Alibaba Cloud E-MapReduce \(EMR\). EMR is an all-in-one enterprise-ready big data platform that provides cluster, job, and data management services based on the open source ecosystem. The ecosystem includes Hadoop, Spark, Kafka, Flink, and Storm. EMR provides seamless support for OSS. EMR is combined with OSS to provide access to OSS by using the open source ecosystem and optimized technologies. For more information, see [What is E-MapReduce?](/intl.en-US/Product Introduction/What is E-MapReduce?.md).

