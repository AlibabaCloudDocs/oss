# Configure Spark to use OSS Select to accelerate data queries

This topic describes how to configure Spark to use OSS Select to accelerate data queries and benefits of data queries.

This topic assumes that you have built and configured a CDH 6 cluster based on [Use CDH 6-based Apache Impala to query OSS data](/intl.en-US/Best Practices/Data processing and analysis/Use CDH 6-based Apache Impala to query OSS data.md).

**Note:** All content in `${}` is environment variables. Modify these environment variables.

## Step 1: Configure Spark to read and write OSS data

By default, the OSS-compliant package is excluded in CLASSPATH of Spark. To add this package to CLASSPATH and configure Spark to read and write OSS data, perform the following operations on all CDH nodes:

1.  Go to the $\{CDH\_HOME\}/lib/spark directory. Run the following commands:

    ```
    [root@cdh-master spark]# cd jars/
    [root@cdh-master jars]# ln -s .. /.. /../jars/hadoop-aliyun-3.0.0-cdh6.0.1.jar hadoop-aliyun.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-sdk-oss-2.8.3.jar aliyun-sdk-oss-2.8.3.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/jdom-1.1.jar jdom-1.1.jar
    ```

2.  Go to the $\{CDH\_HOME\}/lib/spark directory. Run a query.

    ```
    [root@cdh-master spark]# ./bin/spark-shell
    WARNING: User-defined SPARK_HOME (/opt/cloudera/parcels/CDH-6.0.1-1.cdh6.0.1.p0.590678/lib/spark) overrides detected (/opt/cloudera/parcels/CDH/lib/spark).
    WARNING: Running spark-class from user-defined location.
    Setting default log level to "WARN".
    To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
    Spark context Web UI available at http://x.x.x.x:4040
    Spark context available as 'sc' (master = yarn, app id = application_1540878848110_0004).
    Spark session available as'spark'.
    Welcome to
          ____              __
         / __/__  ___ _____/ /__
        _\ \/ _ \/ _ `/ __/  '_/
       /___/ .__/\_,_/_/ /_/\_\   version 2.2.0-cdh6.0.1
          /_/
    
    Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)
    Type in expressions to have them evaluated.
    Type :help for more information.
    
    scala> val myfile = sc.textFile("oss://{your-bucket-name}/50/store_sales")
    myfile: org.apache.spark.rdd.RDD[String] = oss://{your-bucket-name}/50/store_sales MapPartitionsRDD[1] at textFile at <console>:24
    
    scala> myfile.count()
    res0: Long = 144004764
    
    scala> myfile.map(line => line.split('|')).filter(_(0).toInt >= 2451262).take(3)
    res15: Array[Array[String]] = Array(Array(2451262, 71079, 20359, 154660, 284233, 6206, 150579, 46, 512, 2160001, 84, 6.94, 11.38, 9.33, 681.83, 783.72, 582.96, 955.92, 5.09, 681.83, 101.89, 106.98, -481.07), Array(2451262, 71079, 26863, 154660, 284233, 6206, 150579, 46, 345, 2160001, 12, 67.82, 115.29, 25.36, 0.00, 304.32, 813.84, 1383.48, 21.30, 0.00, 304.32, 325.62, -509.52), Array(2451262, 71079, 55852, 154660, 284233, 6206, 150579, 46, 243, 2160001, 74, 32.41, 34.67, 1.38, 0.00, 102.12, 2398.34, 2565.58, 4.08, 0.00, 102.12, 106.20, -2296.22))
    
    scala> myfile.map(line => line.split('|')).filter(_(0) >= "2451262").saveAsTextFile("oss://{your-bucket-name}/spark-oss-test.1")
    ```

    If the query is run properly, Spark is deployed.


## Step 2: Configure Spark to support OSS Select

For more information about OSS Select, see [OSS Select](https://yq.aliyun.com/articles/593910). The following section uses oss-cn-shenzhen.aliyuncs.com as the OSS endpoint. Perform the following operations on all CDH nodes:

1.  Click [here](http://gosspublic.alicdn.com/hadoop-spark/spark-2.2.0-oss-select-0.1.0-SNAPSHOT.tar.gz) to download the spark-2.2.0-oss-select-0.1.0-SNAPSHOT.tar.gz package to the $\{CDH\_HOME\}/jars directory. This package is in preview.

2.  Decompress the downloaded package.

    ```
    [root@cdh-master jars]# tar -tvf spark-2.2.0-oss-select-0.1.0-SNAPSHOT.tar.gz 
    drwxr-xr-x root/root 0 2018-10-30 17:59 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/
    -rw-r--r-- root/root 26514 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/stax-api-1.0.1.jar
    -rw-r--r-- root/root 547584 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-sdk-oss-3.3.0.jar
    -rw-r--r-- root/root 13277 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-java-sdk-sts-3.0.0.jar
    -rw-r--r-- root/root 116337 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-java-sdk-core-3.4.0.jar
    -rw-r--r-- root/root 215492 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-java-sdk-ram-3.0.0.jar
    -rw-r--r-- root/root 67758 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/jettison-1.1.jar
    -rw-r--r-- root/root 57264 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/json-20170516.jar
    -rw-r--r-- root/root 890168 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/jaxb-impl-2.2.3-1.jar
    -rw-r--r-- root/root 458739 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/jersey-core-1.9.jar
    -rw-r--r-- root/root 147952 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/jersey-json-1.9.jar
    -rw-r--r-- root/root 788137 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-java-sdk-ecs-4.2.0.jar
    -rw-r--r-- root/root 153115 2018-10-30 16:11 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/jdom-1.1.jar
    -rw-r--r-- root/root 65437 2018-10-31 14:41 spark-2.2.0-oss-select-0.1.0-SNAPSHOT/aliyun-oss-select-spark_2.11-0.1.0-SNAPSHOT.jar
    						
    ```

3.  Go to the $\{CDH\_HOME\}/lib/spark/jars directory. Run the following commands:

    ```
    [root@cdh-master jars]# pwd
    /opt/cloudera/parcels/CDH/lib/spark/jars
    [root@cdh-master jars]# rm -f aliyun-sdk-oss-2.8.3.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-oss-select-spark_2.11-0.1.0-SNAPSHOT.jar aliyun-oss-select-spark_2.11-0.1.0-SNAPSHOT.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-java-sdk-core-3.4.0.jar aliyun-java-sdk-core-3.4.0.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-java-sdk-ecs-4.2.0.jar aliyun-java-sdk-ecs-4.2.0.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-java-sdk-ram-3.0.0.jar aliyun-java-sdk-ram-3.0.0.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-java-sdk-sts-3.0.0.jar aliyun-java-sdk-sts-3.0.0.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/aliyun-sdk-oss-3.3.0.jar aliyun-sdk-oss-3.3.0.jar
    [root@cdh-master jars]# ln -s .. /.. /../jars/jdom-1.1.jar jdom-1.1.jar
    ```


## Comparison test

Test environment: Spark on YARN is used for the comparison test. Up to four containers can be run on each of the four Node Manager nodes. Each container is configured with CPU of one core and memory of 2 GB.

Test data: a total of 630 MB in size, including three columns: Name, Company, and Age.

```
ot@cdh-master jars]# hadoop fs -ls oss://select-test-sz/people/
Found 10 items
-rw-rw-rw-   1   63079930 2018-10-30 17:03 oss://select-test-sz/people/part-00000
-rw-rw-rw-   1   63079930 2018-10-30 17:03 oss://select-test-sz/people/part-00001
-rw-rw-rw-   1   63079930 2018-10-30 17:05 oss://select-test-sz/people/part-00002
-rw-rw-rw-   1   63079930 2018-10-30 17:05 oss://select-test-sz/people/part-00003
-rw-rw-rw-   1   63079930 2018-10-30 17:06 oss://select-test-sz/people/part-00004
-rw-rw-rw-   1   63079930 2018-10-30 17:12 oss://select-test-sz/people/part-00005
-rw-rw-rw-   1   63079930 2018-10-30 17:14 oss://select-test-sz/people/part-00006
-rw-rw-rw-   1   63079930 2018-10-30 17:14 oss://select-test-sz/people/part-00007
-rw-rw-rw-   1   63079930 2018-10-30 17:15 oss://select-test-sz/people/part-00008
-rw-rw-rw-   1   63079930 2018-10-30 17:16 oss://select-test-sz/people/part-00009
```

Go to the $\{CDH\_HOME\}/lib/spark/ directory. Run spark-shell to perform two tests. One uses OSS Select to query data. The other does not use OSS Select to query data.

```
[root@cdh-master spark]# ./bin/spark-shell
WARNING: User-defined SPARK_HOME (/opt/cloudera/parcels/CDH-6.0.1-1.cdh6.0.1.p0.590678/lib/spark) overrides detected (/opt/cloudera/parcels/CDH/lib/spark).
WARNING: Running spark-class from user-defined location.
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
Spark context Web UI available at http://x.x.x.x:4040
Spark context available as 'sc' (master = yarn, app id = application_1540887123331_0008).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___/ .__/\_,_/_/ /_/\_\   version 2.2.0-cdh6.0.1
      /_/

Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_152)
Type in expressions to have them evaluated.
Type :help for more information.

scala> val sqlContext = spark.sqlContext
sqlContext: org.apache.spark.sql.SQLContext = org.apache.spark.sql.SQLContext@4bdef487

scala> sqlContext.sql("CREATE TEMPORARY VIEW people USING com.aliyun.oss " +
     |   "OPTIONS (" +
     |   "oss.bucket 'select-test-sz', " +
     |   "oss.prefix 'people', " + // objects with this prefix belong to this table
     |   "oss.schema 'name string, company string, age long'," + // like 'column_a long, column_b string'
     |   "oss.data.format 'csv'," + // we only support csv now
     |   "oss.input.csv.header 'None'," +
     |   "oss.input.csv.recordDelimiter '\r\n'," +
     |   "oss.input.csv.fieldDelimiter ','," +
     |   "oss.input.csv.commentChar '#'," +
     |   "oss.input.csv.quoteChar '\"'," +
     |   "oss.output.csv.recordDelimiter '\n'," +
     |   "oss.output.csv.fieldDelimiter ','," +
     |   "oss.output.csv.quoteChar '\"'," +
     |   "oss.endpoint 'oss-cn-shenzhen.aliyuncs.com', " +
     |   "oss.accessKeyId 'Your Access Key Id', " +
     |   "oss.accessKeySecret 'Your Access Key Secret')")
res0: org.apache.spark.sql.DataFrame = []

scala>   val sql: String = "select count(*) from people where name like 'Lora%'"
sql: String = select count(*) from people where name like 'Lora%'

scala>   sqlContext.sql(sql).show()
+--------+
|count(1)|
+--------+
|   31770|
+--------+

scala> val textFile = sc.textFile("oss://select-test-sz/people/")
textFile: org.apache.spark.rdd.RDD[String] = oss://select-test-sz/people/ MapPartitionsRDD[8] at textFile at <console>:24

scala> textFile.map(line => line.split(',')).filter(_(0).startsWith("Lora")).count()
res3: Long = 31770
				
```

The following figure shows the time difference between queries when OSS Select is used. The query time when OSS Select is used is 15s, and the query time when OSS Select is not used is 54s.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6454449951/p33120.png)

## Implementation of the Spark-compliant OSS Select package \(in preview\)

You can make Spark compatible with OSS by extending the Spark Data Source API. For more information, visit [Spark Data Source API](https://mapr.com/blog/spark-data-source-api-extending-our-spark-sql-query-engine/). PrunedFilteredScan can be used to push down required columns and filters to OSS Select. The support for the package is under development. Definition specifications and supported filter conditions:

-   Definition specifications:

    ```
    scala> sqlContext.sql("CREATE TEMPORARY VIEW people USING com.aliyun.oss " +
         |   "OPTIONS (" +
         |   "oss.bucket 'select-test-sz', " +
         |   "oss.prefix 'people', " + // objects with this prefix belong to this table
         |   "oss.schema 'name string, company string, age long'," + // like 'column_a long, column_b string'
         |   "oss.data.format 'csv'," + // we only support csv now
         |   "oss.input.csv.header 'None'," +
         |   "oss.input.csv.recordDelimiter '\r\n'," +
         |   "oss.input.csv.fieldDelimiter ','," +
         |   "oss.input.csv.commentChar '#'," +
         |   "oss.input.csv.quoteChar '\"'," +
         |   "oss.output.csv.recordDelimiter '\n'," +
         |   "oss.output.csv.fieldDelimiter ','," +
         |   "oss.output.csv.quoteChar '\"'," +
         |   "oss.endpoint 'oss-cn-shenzhen.aliyuncs.com', " +
         |   "oss.accessKeyId 'Your Access Key Id', " +
         |   "oss.accessKeySecret 'Your Access Key Secret')")
    ```

    |Field|Description|
    |:----|:----------|
    |oss.bucket|Enter the bucket that contains the data.|
    |oss.prefix|All objects whose names contain this prefix are mapped to the defined TEMPORARY VIEW table.|
    |oss.schema|Specify the schema of the defined TEMPORARY VIEW table. The schema is of the String type. A file will be used to specify the schema in the future.|
    |oss.data.format|Specify the format of the data content. The CSV format is supported. Other formats will be supported.|
    |oss.input.csv. \*|Specify the input parameters when the CSV object is queried.|
    |oss.output.csv. \*|Specify the output parameters when the CSV object is queried.|
    |oss.endpoint|Enter the endpoint used to access the region where the bucket is located.|
    |oss.accessKeyId|Enter the AccessKey ID used to access OSS.|
    |oss.accessKeySecret|Enter the AccessKey secret used to access OSS.|

    **Note:** Only basic parameters are defined. For more information, see [SelectObject](/intl.en-US/API Reference/Object operations/Basic operations/SelectObject.md).

-   The following operators are supported for filter conditions: `=,<,>,<=, >=,||,or,not,and,in,like(StringStartsWith,StringEndsWith,StringContains)`. Only required columns are pushed down to OSS Select when conditions such as arithmetic operations and string concatenation that are not supported by PrunedFilteredScan cannot be used to push down data.

    **Note:**

    OSS Select supports other filter conditions. For more information, see [SelectObject](/intl.en-US/API Reference/Object operations/Basic operations/SelectObject.md).


## Compare queries by using TPC-H

query1.sql in TPC-H is used to query the lineitem table, test the query performance, and verify the configuration effect. To enable OSS Select to filter more data, change the where condition from l\_shipdate <= '1998-09-16' to where l\_shipdate \> '1997-09-16'. The size of data for testing is 2.27 GB. Query methods:

-   Use only the Spark SQL statement to query data:

    ```
    [root@cdh-master ~]# hadoop fs -ls oss://select-test-sz/data/lineitem.csv
    -rw-rw-rw-   1 2441079322 2018-10-31 11:18 oss://select-test-sz/data/lineitem.csv
    ```

-   Use OSS Select in the Spark SQL statement to query data:

    ```
    scala> import org.apache.spark.sql.types.{ IntegerType, LongType, StringType, StructField, StructType, DoubleType}
    import org.apache.spark.sql.types.{ IntegerType, LongType, StringType, StructField, StructType, DoubleType}
    
    scala> import org.apache.spark.sql.{ Row, SQLContext}
    import org.apache.spark.sql.{ Row, SQLContext}
    
    scala> val sqlContext = spark.sqlContext
    sqlContext: org.apache.spark.sql.SQLContext = org.apache.spark.sql.SQLContext@74e2cfc5
    
    scala> val textFile = sc.textFile("oss://select-test-sz/data/lineitem.csv")
    textFile: org.apache.spark.rdd.RDD[String] = oss://select-test-sz/data/lineitem.csv MapPartitionsRDD[1] at textFile at <console>:26
    
    scala> val dataRdd = textFile.map(_.split('|'))
    dataRdd: org.apache.spark.rdd.RDD[Array[String]] = MapPartitionsRDD[2] at map at <console>:28
    
    scala> val schema = StructType(
         |     List(
         |         StructField("L_ORDERKEY",LongType,true),
         |         StructField("L_PARTKEY",LongType,true),
         |         StructField("L_SUPPKEY",LongType,true),
         |         StructField("L_LINENUMBER",IntegerType,true),
         |         StructField("L_QUANTITY",DoubleType,true),
         |         StructField("L_EXTENDEDPRICE",DoubleType,true),
         |         StructField("L_DISCOUNT",DoubleType,true),
         |         StructField("L_TAX",DoubleType,true),
         |         StructField("L_RETURNFLAG",StringType,true),
         |         StructField("L_LINESTATUS",StringType,true),
         |         StructField("L_SHIPDATE",StringType,true),
         |         StructField("L_COMMITDATE",StringType,true),
         |         StructField("L_RECEIPTDATE",StringType,true),
         |         StructField("L_SHIPINSTRUCT",StringType,true),
         |         StructField("L_SHIPMODE",StringType,true),
         |         StructField("L_COMMENT",StringType,true)
         |     )
         | )
    schema: org.apache.spark.sql.types.StructType = StructType(StructField(L_ORDERKEY,LongType,true), StructField(L_PARTKEY,LongType,true), StructField(L_SUPPKEY,LongType,true), StructField(L_LINENUMBER,IntegerType,true), StructField(L_QUANTITY,DoubleType,true), StructField(L_EXTENDEDPRICE,DoubleType,true), StructField(L_DISCOUNT,DoubleType,true), StructField(L_TAX,DoubleType,true), StructField(L_RETURNFLAG,StringType,true), StructField(L_LINESTATUS,StringType,true), StructField(L_SHIPDATE,StringType,true), StructField(L_COMMITDATE,StringType,true), StructField(L_RECEIPTDATE,StringType,true), StructField(L_SHIPINSTRUCT,StringType,true), StructField(L_SHIPMODE,StringType,true), StructField(L_COMMENT,StringType,true))
    
    scala> val dataRowRdd = dataRdd.map(p => Row(p(0).toLong, p(1).toLong, p(2).toLong, p(3).toInt, p(4).toDouble, p(5).toDouble, p(6).toDouble, p(7).toDouble, p(8), p(9), p(10), p(11), p(12), p(13), p(14), p(15)))
    dataRowRdd: org.apache.spark.rdd.RDD[org.apache.spark.sql.Row] = MapPartitionsRDD[3] at map at <console>:30
    
    scala> val dataFrame = sqlContext.createDataFrame(dataRowRdd, schema)
    dataFrame: org.apache.spark.sql.DataFrame = [L_ORDERKEY: bigint, L_PARTKEY: bigint ... 14 more fields]
    
    scala> dataFrame.createOrReplaceTempView("lineitem")
    
    scala> spark.sql("select l_returnflag, l_linestatus, sum(l_quantity) as sum_qty, sum(l_extendedprice) as sum_base_price, sum(l_extendedprice * (1 - l_discount)) as sum_disc_price, sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge, avg(l_quantity) as avg_qty, avg(l_extendedprice) as avg_price, avg(l_discount) as avg_disc, count(*) as count_order from lineitem where l_shipdate > '1997-09-16' group by l_returnflag, l_linestatus order by l_returnflag, l_linestatus").show()
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+------------------+-------------------+-----------+
    |l_returnflag|l_linestatus|    sum_qty|      sum_base_price|      sum_disc_price|          sum_charge|           avg_qty|         avg_price|           avg_disc|count_order|
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+------------------+-------------------+-----------+
    |           N|           O|7.5697385E7|1.135107538838699... |1.078345555027154... |1.121504616321447... |25.501957856643052|38241.036487881756|0.04999335309103123|    2968297|
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+------------------+-------------------+-----------+
    
    scala> sqlContext.sql("CREATE TEMPORARY VIEW item USING com.aliyun.oss " +
         |   "OPTIONS (" +
         |   "oss.bucket 'select-test-sz', " +
         |   "oss.prefix 'data', " +
         |   "oss.schema 'L_ORDERKEY long, L_PARTKEY long, L_SUPPKEY long, L_LINENUMBER int, L_QUANTITY double, L_EXTENDEDPRICE double, L_DISCOUNT double, L_TAX double, L_RETURNFLAG string, L_LINESTATUS string, L_SHIPDATE string, L_COMMITDATE string, L_RECEIPTDATE string, L_SHIPINSTRUCT string, L_SHIPMODE string, L_COMMENT string'," +
         |   "oss.data.format 'csv'," + // we only support csv now
         |   "oss.input.csv.header 'None'," +
         |   "oss.input.csv.recordDelimiter '\n'," +
         |   "oss.input.csv.fieldDelimiter '|'," +
         |   "oss.input.csv.commentChar '#'," +
         |   "oss.input.csv.quoteChar '\"'," +
         |   "oss.output.csv.recordDelimiter '\n'," +
         |   "oss.output.csv.fieldDelimiter ','," +
         |   "oss.output.csv.commentChar '#'," +
         |   "oss.output.csv.quoteChar '\"'," +
         |   "oss.endpoint 'oss-cn-shenzhen.aliyuncs.com', " +
         |   "oss.accessKeyId 'Your Access Key Id', " +
         |   "oss.accessKeySecret 'Your Access Key Secret')")
    res2: org.apache.spark.sql.DataFrame = []
    
    scala> sqlContext.sql("select l_returnflag, l_linestatus, sum(l_quantity) as sum_qty, sum(l_extendedprice) as sum_base_price, sum(l_extendedprice * (1 - l_discount)) as sum_disc_price, sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge, avg(l_quantity) as avg_qty, avg(l_extendedprice) as avg_price, avg(l_discount) as avg_disc, count(*) as count_order from item where l_shipdate > '1997-09-16' group by l_returnflag, l_linestatus order by l_returnflag, l_linestatus").show()
    
    scala> sqlContext.sql("select l_returnflag, l_linestatus, sum(l_quantity) as sum_qty, sum(l_extendedprice) as sum_base_price, sum(l_extendedprice * (1 - l_discount)) as sum_disc_price, sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge, avg(l_quantity) as avg_qty, avg(l_extendedprice) as avg_price, avg(l_discount) as avg_disc, count(*) as count_order from item where l_shipdate > '1997-09-16' group by l_returnflag, l_linestatus order by l_returnflag, l_linestatus").show()
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+-----------------+-------------------+-----------+
    |l_returnflag|l_linestatus|    sum_qty|      sum_base_price|      sum_disc_price|          sum_charge|           avg_qty|        avg_price|           avg_disc|count_order|
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+-----------------+-------------------+-----------+
    |           N|           O|7.5697385E7|1.135107538838701E11|1.078345555027154... |1.121504616321447... |25.501957856643052|38241.03648788181|0.04999335309103024|    2968297|
    +------------+------------+-----------+--------------------+--------------------+--------------------+------------------+-----------------+-------------------+-----------+
    ```


The following figure shows that one query takes 38s when Spark SQL and OSS Select are used. The other query takes 2.5 minutes when only Spark SQL is used. OSS Select accelerates queries.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6454449951/p33133.jpeg)

## References

For more information, visit the following websites and see the following topics:

-   [Use SQL statements to select data from OSS objects](https://yq.aliyun.com/articles/593910)
-   [Use CDH 6-based Apache Impala to query OSS data](/intl.en-US/Best Practices/Data processing and analysis/Use CDH 6-based Apache Impala to query OSS data.md)
-   [Use CDH 5 to read and write OSS data](/intl.en-US/Best Practices/Data processing and analysis/Use CDH 5 to read and write OSS data.md)
-   [Spark Data Source API](https://mapr.com/blog/spark-data-source-api-extending-our-spark-sql-query-engine/)

