# Architectures and configurations

ossimport is a tool used to migrate data to OSS. You can deploy ossimport on local servers or ECS instances in the cloud to migrate data stored locally or in other cloud storage systems to OSS.

ossimport has the following features:

-   Supports a wide range of data sources including local data sources, Qiniu Cloud Object Storage \(KODO\), Baidu Object Storage \(BOS\), Amazon Simple Storage Service \(Amazon S3\), Azure Blob, UPYUN Storage Service \(USS\), Tencent Cloud Object Service \(COS\), Kingsoft Standard Storage Service \(KS3\), HTTP, and OSS. These sources can be expanded as needed.
-   Supports standalone and distributed modes. The standalone mode is easy to deploy and use. The distributed mode is suitable for large-scale data migration.
-   Supports resumable data transfer.
-   Supports throttling.
-   Supports migration of objects whose last modified time is later than a specified time or objects whose names contain a specified prefix.
-   Supports the upload and download of data in parallel.

## Runtime environment

ossimport can be deployed to Linux or Windows that meets the following requirements:

-   Windows 7 or later versions
-   Latest Linux version
-   JDK 1.7 or later versions

**Note:** ossimport cannot be deployed in distributed mode in Windows.

## Deployment modes

ossimport supports the standalone and distributed modes.

-   The standalone mode is sufficient for small-scale migration of data smaller than 30 TB in size. You can deploy ossimport on any machine locally or in the cloud. [Download link](http://gosspublic.alicdn.com/ossimport/standalone/ossimport-2.3.4.zip)
-   The distributed mode is suitable for migration of data larger than 30 TB. You can deploy ossimport on multiple machines locally or in the cloud. [Download link](http://gosspublic.alicdn.com/ossimport/distributed/ossimport-2.3.4.tar.gz)

    **Note:** To save time for migration of huge amounts of data, deploy ossimport on an ECS instance in the same region as your OSS bucket. Use a leased line to attach the server that stores source data to Alibaba Cloud VPC. Migration efficiency is greatly improved when the internal network is used to migrate data from ECS instances to OSS.


## Standalone mode

Master, Worker, Tracker, and Console are packaged as `ossimport2.jar` and run on a machine. The system has only one Worker.

The following figure describes the file structure in standalone mode:

```
ossimport
├── bin
│ └── ossimport2.jar  # The JAR package that contains the Master, Worker, Tracker, and Console modules.
├── conf
│ ├── local_job.cfg   # The Job configuration file for the standalone mode.
│ └── sys.properties  # The configuration file that contains system parameters.
├── console.bat         # The command-line tool in Windows used to run tasks step by step.
├── console.sh          # The command-line tool in Linux used to run tasks step by step.
├── import.bat          # The script that automatically imports files based on the conf/local_job.cfg configuration file in Windows. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
├── import.sh           # The script that automatically imports files based on the conf/local_job.cfg configuration file in Linux. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
├── logs                # The directory that contains logs.
└── README.md           # The file that introduces or explains ossimport. We recommend that you read this file before you use ossimport.
```

-   import.bat and import.sh are scripts that automatically import files based on the configuration file. You can run these tools after you modify the `local_job.cfg` configuration file.
-   console.bat and console.sh are command-line tools used to run commands step by step.
-   Run scripts or commands in the `ossimport` directory. These scripts and the `*.bat/*.sh` file are at the same directory level.

## Distributed mode

The ossimport architecture in distributed mode consists of Master and Worker. The following code describes the structure:

```
Master --------- Job --------- Console
    |
    |
   TaskTracker
    |_____________________
    |Task     | Task      | Task
    |         |           |
Worker      Worker      Worker
```

|Parameter|Description|
|:--------|:----------|
|Master|Splits a job into multiple tasks by data size and number of files. The data size and number of files can be configured in the sys.properties file. Master splits a job into multiple tasks by performing the following steps: 1.  Master traverses the full list of files to be migrated from the local source or cloud storage system.
2.  Master splits a job into multiple tasks by data size and number of files. Each task is responsible for the migration or verification of a portion of files. |
|Worker|-   Migrates files and verifies data for tasks. It pulls the specific file from the data source and uploads the file to the specified directory in OSS. You can specify the data source to be migrated and the OSS configuration in the job.cfg or local\_job.cfg configuration file.
-   Supports throttling and specifies the number of concurrent tasks for data migration. You can configure the settings in the sys.properties configuration file. |
|TaskTracker|Distributes tasks and tracks task statuses. It is abbreviated to Tracker.|
|Console|Interacts with users and receives and displays command results. It supports system management commands such as deploy, start, and stop, and job management commands such as submit, retry, and clean.|
|Job|Serves as the data migration jobs submitted by users. One job corresponds to one configuration file `job.cfg`.|
|Task|Migrates a portion of files. A job can be divided into multiple tasks by data size and number of files. The minimal unit for dividing a job into tasks is a file. One file cannot be split into multiple tasks.|

In distributed mode, multiple Workers can be started to migrate data. Tasks are evenly allocated to Workers. One Worker can run multiple tasks. Only one Worker can be started on each machine. Master and Tracker are started on the machine where the first Worker specified by `workers` resides. Console must also run on this machine.

The following code describes the file structure in distributed mode:

```
ossimport
├── bin
│ ├── console.jar     # The JAR package for the Console module.
│ ├── master.jar      # The JAR package for the Master module.
│ ├── tracker.jar     # The JAR package for the Tracker module.
│ └── worker.jar      # The JAR package for the Worker module.
├── conf
│ ├── job.cfg         # The Job configuration file template.
│ ├── sys.properties  # The configuration file that contains system parameters.
│ └── workers         # The list of Workers.
├── console.sh          # The command-line tool. Currently, only Linux is supported.
├── logs                # The directory that contains logs.
└── README.md           # The file that introduces or explains ossimport. We recommend that you read this file before you use ossimport.
```

## Configuration files

Two configuration files sys.properties and local\_job.cfg are provided in standalone mode. Three configuration files sys.properties, job.cfg, and workers are provided in distributed mode. Configurations items in the local\_job.cfg and job.cfg configuration files are the same except for their file names. The workers configuration file is exclusive to the distributed mode.

-   sys.properties: the system parameters

    |Parameter|Purpose|Description|
    |:--------|:------|:----------|
    |workingDir|The working directory.|The directory to which the tool package is decompressed. Do not modify this parameter in standalone mode. Working directories of each machine in distributed mode must be the same.|
    |workerUser|The SSH username used to log on to the machine where Worker resides.|    -   If privateKeyFile is configured, privateKeyFile is prioritized.
    -   If privateKeyFile is not configured, use workerUser and workerPassword.
    -   Do not modify this parameter in standalone mode. |
    |workerPassword|The password of the SSH username used to log on to the machine where Worker resides.|Do not modify this parameter in standalone mode.|
    |privateKeyFile|The path of the private key file.|    -   If you have established an SSH channel, you can specify this parameter. Otherwise, leave this parameter unspecified.
    -   If privateKeyFile is configured, privateKeyFile is prioritized.
    -   If privateKeyFile is not configured, use workerUser and workerPassword.
    -   Do not modify this parameter in standalone mode. |
    |sshPort|The SSH port.|The default value is 22. We recommend that you retain the default value. Do not modify this parameter in standalone mode.|
    |workerTaskThreadNum|The maximum number of threads for Worker to run tasks.|    -   This parameter is related to the machine memory and network. We recommend that you set this parameter to 60.
    -   The value can be increased. For example, you can set this parameter to a greater value such as 150 for physical machines. If the network bandwidth is already full, do not further increase the value.
    -   If the network quality is poor, lower the value, such as 30. This way, you can avoid request competition for bandwidth and the timeout of a large number of requests. |
    |workerMaxThroughput\(KB/s\)|The throttling of data migration for Worker.|This value can be used for throttling. The default value is 0, which indicates that no throttling is imposed.|
    |dispatcherThreadNum|The number of threads for task distribution and status confirmation of Tracker.|If you do not have special requirements, retain the default value.|
    |workerAbortWhenUncatchedException|Indicates whether to skip or terminate a task if an unknown error occurs.|By default, unknown errors are skipped.|
    |workerRecordMd5|Indicates whether to use metadata x-oss-meta-md5 to record the MD5 hash values of files to be migrated. By default, MD5 hash values are not recorded.|This parameter value is used to verify data integrity.|

-   job.cfg: the configurations for data migration jobs. Configuration items in the `local_job.cfg` and `job.cfg` configuration files are the same except for the file names.

    |Parameter|Purpose|Description|
    |:--------|:------|:----------|
    |jobName|The name of the job. The value is of the String type.|    -   The unique identifier of the job. A job name follows the following naming conventions: The name can contain letters, digits, underscores \(\_\), and hyphens \(-\). The name must be 4 to 128 characters in length. You can submit multiple jobs of different names.
    -   If you submit a job with the same name as an existing job, the system prompts that the job already exists. You are not allowed to submit a job of the same name before you clean the existing job with the same name. |
    |jobType|The type of the job. The value is of the String type.|Valid values: import and audit. Default value: import.     -   import: runs the data migration job and verifies the migration data for consistency.
    -   audit: only verifies data consistency. |
    |isIncremental|Indicates whether to enable the incremental migration mode. The value is of the Boolean type.|    -   Default value: false
    -   If this parameter is set to true, incremental data is rescanned at the interval specified by incrementalModeInterval in seconds and synchronized to the OSS. |
    |incrementalModeInterval|The synchronization interval in seconds in incremental mode. The value is of the Integer type.|This parameter is valid when isIncremental is set to true. The minimum configurable interval is 900s. We recommend that you do not set it to a value smaller than 3600s. If you set this parameter to a smaller value, a large number of requests are wasted, which results in extra system overheads.|
    |importSince|The time in seconds based on which to migrate data. Data whose last modified time is greater than the value of this parameter is migrated. The value is of the Integer type.|    -   The timestamp follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, 1 January 1970. You can run the date +%s command to obtain the seconds.
    -   The default value is 0, indicating that all data is migrated. |
    |srcType|The source type for synchronization. The value is of the String type. Be aware that the value is case-sensitive.|The following source types are supported:     -   local: migrates data from a local file to OSS. To specify this option, specify srcPrefix and ignore srcAccessKey, srcSecretKey, srcDomain, and srcBucket.
    -   oss: migrates data from one OSS bucket to another bucket.
    -   qiniu: migrates data from KODO to OSS.
    -   bos: migrates data from BOS to OSS.
    -   ks3: migrates data from KS3 to OSS.
    -   s3: migrates data from Amazon S3 to OSS.
    -   youpai: migrates data from USS to OSS.
    -   http: migrates data from HTTP sources to OSS.
    -   cos: migrates data from COS to OSS.
    -   azure: migrates data from Azure Blob to OSS. |
    |srcAccessKey|The AccessKey ID used to access the source. The value is of the String type.|    -   If srcType is set to oss, qiniu, baidu, ks3, or s3, specify the AccessKey ID used to access the source.
    -   If srcType is set to local or http, ignore this parameter.
    -   If srcType is set to youpai or azure, specify the username used to access the source. |
    |srcSecretKey|The AccessKey secret used to access the source. The value is of the String type.|    -   If srcType is set to oss, qiniu, baidu, ks3, or s3, specify the AccessKey secret used to access the source.
    -   If srcType is set to local or http, ignore this parameter.
    -   If srcType is set to youpai, specify the operator password used to access the source.
    -   If srcType is set to azure, specify the account key used to access the source. |
    |srcDomain|The source endpoint.|    -   If srcType is set to local or http, ignore this parameter.
    -   If srcType is set to oss, enter the endpoint obtained from the OSS console. The endpoint is a second-level domain without the bucket name as the prefix.
    -   If srcType is set to qiniu, enter the domain name corresponding to the bucket obtained from the KODO console.
    -   If srcType is set to bos, enter the BOS domain name. Example: `http://bj.bcebos.com` or `http://gz.bcebos.com`.
    -   If srcType is set to ks3, enter the KS3 domain name. Example: `http://kss.ksyun.com`, `http://ks3-cn-beijing.ksyun.com`, or `http://ks3-us-west-1.ksyun.coms`.
    -   If srcType is set to S3, enter the domain name to access the region where Amazon S3 resides.
    -   If srcType is set to youpai, enter the USS domain name such as automatic identification of the optimal path of `http://v0.api.upyun.com`, telecommunication line `http://v1.api.upyun.com`, China Unicom or China Netcom line `http://v2.api.upyun.com`, or China Mobile or China Railcom line `http://v3.api.upyun.com`.
    -   If srcType is set to cos, enter the region where the COS bucket resides. Example: ap-guangzhou.
    -   If srcType is set to azure, enter the endpoint suffix in the Azure Blob connection string. Example: core.chinacloudapi.cn. |
    |srcBucket|The name of the source bucket or container.|    -   If srcType is set to local or http, ignore this parameter.
    -   If srcType is set to azure, enter the name of the source container.
    -   In other cases, enter the name of the source bucket. |
    |srcPrefix|The source prefix. The value is of the String type. The default value is null.|    -   If srcType is set to local, enter the full path that ends with a forward slash \(/\). If the path contains two or more directory levels, separate each directory with one forward slash \(/\). Example: c:/example/ or /data/example/.

**Note:** Paths such as c:/example//, /data//example/, and /data/example// are invalid.

    -   If srcType is set to oss, qiniu, bos, ks3, youpai, or s3, enter the prefix for objects to be synchronized. The prefix excludes bucket names. Example of the prefix: data/to/oss/.
    -   To synchronize all files, leave srcPrefix unspecified. |
    |destAccessKey|The AccessKey ID used to access the destination. The value is of the String type.|To obtain the AccssKey ID to access the OSS bucket, log on to [Alibaba Cloud Management Console](https://oss.console.aliyun.com/index#/). |
    |destSecretKey|The AccessKey secret used to access the destination. The value is of the String type.|To obtain the AccssKey secret used to access the OSS bucket, log on to [Alibaba Cloud Management Console](https://oss.console.aliyun.com/index#/). |
    |destDomain|The destination endpoint. The value is of the String type.|To obtain the second-level domain without the bucket name as the prefix, log on to [Alibaba Cloud Management Console](https://oss.console.aliyun.com/index#/). |
    |destBucket|The destination bucket. The value is of the String type.|The name of the OSS bucket. The name cannot end with a forward slash \(/\).|
    |destPrefix|The destination prefix. The value is of the String type. The default value is null.|    -   The destination prefix. If you retain the default value, the migrated objects are stored in the destination bucket.
    -   To synchronize data to a specified directory in OSS, end the prefix with a forward slash \(/\). Example of the prefix: data/in/oss/.
    -   Be aware that OSS object names cannot start with a forward slash \(/\). Do not start the destination prefix with a forward slash \(/\).
    -   A local file stored in the path with the format of srcPrefix+relativePath is migrated to destDomain/destBucket/destPrefix+relativePath in OSS.
    -   An object stored in the cloud in the path with the format of srcDomain/srcBucket/srcPrefix+relativePath is migrated to destDomain/destBucket/destPrefix+relativePath in OSS. |
    |taskObjectCountLimit|The maximum number of files in each Task. The value is of the Integer type. The default value is 10000.|This configuration option affects the concurrency of jobs to run. In most cases, this parameter is set based on the formula: Value = Total number of files/Total number of Workers/Number of migration threads \(workerTaskThreadNum\). The maximum value is 50000. If the total number of files is unknown, use the default value.|
    |taskObjectSizeLimit|The maximum data size in bytes for each task. The value is of the Integer type. The default value is 1 GB.|This configuration option affects the concurrency of jobs to run. In most cases, this parameter is set based on the formula: Value = Total data size/Total number of Workers/Number of migration threads \(workerTaskThreadNum\). If the total data size is unknown, use the default value.|
    |isSkipExistFile|Indicates whether to skip the existing objects during data migration. The value is of the Boolean type.|If this parameter is set to true, the objects are skipped based on the size and LastModifiedTime. If this parameter is set to false, the existing objects are overwritten. The default value is false. This option is invalid when jobType is set to audit.|
    |scanThreadCount|The number of threads that scan files in parallel. The value is of the Integer type.     -   Default value: 1.
    -   Valid values: 1 to 32
|This configuration option is related to file scanning efficiency. If you do not have special requirements, retain the default value.|
    |maxMultiThreadScanDepth|The maximum allowable depth of directories for parallel scanning. The value is of the Integer type.     -   Default value: 1.
    -   Valid values: 1 to 16
|    -   Default value 1 indicates parallel scanning on top-level directories.
    -   If you do not have special requirements, retain the default value. A large value may cause task failures. |
    |appId|The application ID \(account number\) of COS. The value is of the Integer type.|This parameter is valid when srcType is set to cos.|
    |httpListFilePath|The absolute path of the HTTP list file. The value is of the String type.|    -   This parameter is valid when srcType is set to http. When the source is accessed through an HTTP link, you must provide the absolute path of the file that contains the HTTP link. Example: c:/example/http.list.
    -   The HTTP link in the file must be divided into two columns separated with spaces, which indicates the prefix and the relative path in OSS after the upload. For example, the c:/example/http.list file can contain two rows: `http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/aa/ bb.jpg` and `http://mingdi-hz.oss-cn-hangzhou.aliyuncs.com/cc/dd.jpg`. The object names in OSS after the objects are migrated are in destPrefix+ bb.jpg and destPrefix+cc/dd.jpg formats. |

-   workers: exclusive to the distributed mode. Each IP address is separated with a line break. Examples:

    ```
    192.168.1.6
    192.168.1.7
    192.168.1.8
    ```

    -   In the preceding configuration, `192.168.1.6` is in the first row, which must be master. `192.168.1.6` is the IP address of the machine where Master, Worker, and TaskTracker are started. Console also runs on this machine.
    -   Make sure that the username, logon mode, and working directory of each Worker machine in multiple-Worker mode are the same.

## Configuration file examples

The following table describes the configuration file of a data migration job in distributed mode. The name of the configuration file in standalone mode is `local_job.cfg`, which contains the same configuration items as those in distributed mode.

|Migration type|Configuration file|Description|
|:-------------|:-----------------|:----------|
|Migrate local data to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/local/job.cfg)|srcPrefix specifies an absolute path that ends with a forward slash \(/\). Example: `D:/work/oss/data/` or `/home/user/work/oss/data/`.|
|Migrate data from KODO to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/qiniu/job.cfg)|You can leave srcPrefix and destPrefix unspecified. To specify these parameters, end the prefixes with a forward slash \(/\). Example: `destPrefix=docs/`.|
|Migrate data from BOS to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/bos/job.cfg)|You can leave srcPrefix and destPrefix unspecified. To specify these parameters, end the prefixes with a forward slash \(/\). Example: `destPrefix=docs/`.|
|Migrate data from AWS S3 to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/s3/job.cfg)|For more information, see [AWS Service Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region).|
|Migrate data from USS to OSS|[job.cfg](https://github.com/aliyun/ossimport/tree/master/conf/youpai)|Set srcAccessKey and srcSecretKey to the operator account and password.|
|Migrate data from COS to OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/cos/job.cfg)|Set srcDomain based on V4. Example: `srcDomain=sh`. You can leave srcPrefix unspecified. To specify this parameter, start and end the prefix with a forward slash \(`/`\). Example: `srcPrefix=/docs/`.|
|Migrate data from Azure Blob to OSS|[job.cfg](https://github.com/aliyun/ossimport/tree/master/conf/blob)|Set srcAccessKey and srcSecretKey to the storage account and access key. Set srcDomain to the endpoint suffix in the Azure Blob connection string. Example: `core.chinacloudapi.cn`.|
|Migrate data between buckets in OSS|[job.cfg](https://github.com/aliyun/ossimport/blob/master/conf/oss/job.cfg)|This method is suitable for data migration between different regions, different storage classes, and objects whose names have different prefixes. We recommend that you deploy your service on ECS and use the domain name for access over the internal network to minimize the traffic cost.|

## Advanced settings

-   Time-specific throttling

    In the sys.properties configuration file, workerMaxThroughput\(KB/s\) specifies the upper throttling limit for Worker. To configure throttling for business such as throttling for the source and network limits, set this parameter to a value smaller than the maximum available bandwidth for the machine based on business requirements. After the modification is complete, restart the service for the modification to take effect.

    In distributed mode, modify the sys.properties configuration file in the $OSS\_IMPORT\_WORK\_DIR/conf directory for each Worker. Restart the service.

    To implement time-specific throttling, modify the sys.properties configuration file as scheduled by using crontab and restart the service for the modification to take effect.

-   Modify the number of concurrent tasks
    -   In the sys.properties configuration file, workerTaskThreadNum specifies the number of concurrent tasks run by Worker. If the network quality is poor and Worker has to process a large number of tasks, timeout errors are returned. To resolve this issue, modify the configuration by reducing the number of concurrent tasks and restart the service.
    -   In the sys.properties configuration file, workerMaxThroughput\(KB/s\) specifies the upper throttling limit for Worker. To configure throttling for business such as throttling for the source and network limits, set this parameter to a value smaller than the maximum available bandwidth for the machine based on business requirements.
    -   In the job.cfg configuration file, taskObjectCountLimit specifies the maximum number of files in each task. The default value is 10000. This parameter configuration affects the number of tasks. The efficiency of implementing concurrent tasks may degrade if you set this parameter to a small value.
    -   In the job.cfg configuration file, taskObjectSizeLimit specifies the maximum data size for each task. The default value is 1 GB. This parameter configuration affects the number of tasks. The efficiency of implementing concurrent tasks may degrade if you set this parameter to a small value.

        **Note:**

        -   We recommend that you specify the configuration file parameters before you start the migration.
        -   After you modify parameters in the sys.properties configuration file, restart the server for migration to make the modification take effect.
        -   After job.cfg is submitted, parameters in the job.cfg configuration file cannot be modified.
-   Data verification without migration

    To specify that ossimport only verifies data without migrating data, set the job.cfg or local\_job.cfg configuration file. Set jobType to audit instead of import. Configurations of other parameters are the same as those for data migration.

-   Incremental data migration mode

    The incremental data migration mode allows you to migrate existing data after the migration task is started and migrate incremental data at intervals. The first data migration task to migrate existing data is started after you submit the job. Incremental data is migrated at intervals. The incremental data migration mode is suitable for data backup and synchronization.

    The following configuration items are available for the incremental data migration mode:

    -   In the job.cfg configuration file, isIncremental specifies whether to enable the incremental data migration mode. true indicates that the incremental data migration mode is enabled. false indicates that the incremental data migration mode is disabled. The default value is false.
    -   In the job.cfg configuration file, incrementalModeInterval indicates the interval in seconds at which incremental data migration is implemented. This configuration takes effect if you specify `isIncremental=true`. The minimum configurable value is `900`. We recommend that you do not set this parameter to a value smaller than `3600`. If you set this parameter to a smaller value, a large number of requests are wasted, resulting in extra system overheads.
-   Filtering conditions for source objects

    You can set filtering conditions to migrate objects that meet specified conditions. ossimport allows you to specify prefixes and last modified time.

    -   In the job.cfg configuration file, srcPrefix specifies the prefix of the source objects. The default value is null.
        -   If you specify `srcType=local`, enter the local directory path. Enter the full path that ends with a forward slash \(/\). If the path contains two or more directory levels, separate each directory with one forward slash \(/\). Example: `c:/example/` or `/data/example/`.
        -   If you set `srcType` to `oss`, `qiniu`, `bos`, `ks3`, `youpai`, or `s3`, enter the name prefix of source objects. Example: `data/to/oss/`. To migrate all objects, leave `srcPrefix` unspecified.
    -   In the job.cfg configuration file, importSince specifies the last modified time in seconds for source objects, importSince specifies the timestamp that follows the UNIX time format. It is the number of seconds that have elapsed since 00:00:00 Thursday, 1 January 1970. You can run the date +%s command to obtain the seconds. The default value is 0, indicating that all data is migrated. In incremental data migration mode, this parameter is valid only for the first full migration. In non-incremental mode, this parameter is valid for the entire migration job.
        -   If the `LastModified Time` values of objects are smaller than the `importSince` value, objects are not migrated.
        -   If the `LastModified Time` values of objects are greater than the `importSince` value, objects are migrated.

