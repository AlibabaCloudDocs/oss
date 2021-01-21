# Use MaxCompute to build a data warehouse based on OSS

This topic describes how to use MaxCompute to build a petabyte-grade data warehouse based on OSS. MaxCompute can analyze the large amounts of data stored in OSS in a fast and efficient manner, allowing you to explore data value within several minutes and at low cost.

-   You have activated OSS, and created one or more buckets.
    -   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
    -   For more information about how to create a bucket, see [Create a bucket](/intl.en-US/Developer Guide/Buckets/Create buckets.md).
-   You have activated MaxCompute and authorized MaxCompute to access OSS.
    -   For more information about how to activate MaxCompute, see [Activate MaxCompute](/intl.en-US/Prepare/Activate MaxCompute.md).
    -   You need to authorize the account used for running MaxCompute jobs to access OSS data. After you log on with an Alibaba Cloud account, click [here](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.16.761b1cdfvC1ITJ#role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunODPSDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22https:%2F%2Fram.console.aliyun.com%2F%22,%20%22Service%22:%20%22ODPS%22%7D) to authorize the account.

Internet finance applications need to store large amounts of financial data exchange objects in OSS every day and perform structured analysis of large text files. MaxCompute provides the OSS external table query function, which allows you to use external tables to load large OSS objects into MaxCompute for analysis. This method can improve the efficiency of the entire link.

## Operation example: Analyze data collected by IoT

1.  Upload data collected by IoT to OSS.

    For more information about the procedure, see [Upload an object](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload an object.md). You can use any data set to perform tests and verify the best practice described in this topic. This example prepares CSV data in OSS. The endpoint is oss-cn-beijing-internal.aliyuncs.com. The bucket name is oss-odps-test. Data is stored in /demo/vehicle.csv.

2.  Create a MaxCompute project.

    For more information about the procedure, see [Create a project](/intl.en-US/Prepare/Create a project.md).

3.  Create an external table by using MaxCompute.

    For more information about the procedure, see the "Create a table" section in [Create and view a table](/intl.en-US/Quick Start/Create and view a table.md). An example of the statement is as follows:

    ```
    CREATE EXTERNAL TABLE IF NOT EXISTS ambulance_data_csv_external
    (
        vehicleId int,
        recordId int,
        patientId int,
        calls int,
        locationLatitute double,
        locationLongtitue double,
        recordTime string,
        direction string
        )
        STORED BY 'com.aliyun.odps.CsvStorageHandler'
        LOCATION 'oss://oss-cn-beijing-internal.aliyuncs.com/oss-odps-test/Demo/';
    ```

4.  Query the external table through MaxCompute.

    After creating an external table, you can use it as a normal table. For more information, see [Obtain and analyze data](/intl.en-US/Quick Start/Run SQL statements and export data.md).

    Assume that the /demo/vehicle.csv object contains the following data:

    ```
    1,1,51,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,2,13,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,3,48,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,4,30,1,46.81006,-92.08174,9/14/2014 0:00,W
    1,5,47,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,6,9,1,46.81006,-92.08174,9/14/2014 0:00,S
    1,7,53,1,46.81006,-92.08174,9/14/2014 0:00,N
    1,8,63,1,46.81006,-92.08174,9/14/2014 0:00,SW
    1,9,4,1,46.81006,-92.08174,9/14/2014 0:00,NE
    1,10,31,1,46.81006,-92.08174,9/14/2014 0:00,N
    ```

    Run the following SQL statement:

    ```
    select recordId, patientId, direction from ambulance_data_csv_external where patientId > 25;
    ```

    A similar command output is displayed:

    ```
    +------------+------------+-----------+
    | recordId   | patientId  | direction |
    +------------+------------+-----------+
    | 1          | 51         | S         |
    | 3          | 48         | NE        |
    | 4          | 30         | W         |
    | 5          | 47         | S         |
    | 7          | 53         | N         |
    | 8          | 63         | SW        |
    | 10         | 31         | N         |
    +------------+------------+-----------+
    ```

    For more information about how to use OSS external tables, see [Overview](/intl.en-US/Development/External table/Overview.md).


