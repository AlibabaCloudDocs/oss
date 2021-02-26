# Use ossimport to migrate data

By using ossimport, you can migrate data from local storage, third-party storage, or OSS buckets in any region to OSS buckets in different regions. This topic describes how to use ossimport to migrate data from a third-party storage service to OSS.

A user has 500 TB of data stored in the Guangzhou region of Tencent Cloud Object Storage \(COS\). The user wants to use ossimport to migrate the data to an bucket in the China \(Hangzhou\) region of OSS within one week. The business runs properly during the migration process.

ossimport can be deployed in standalone mode or distributed mode:

-   Standalone mode is suitable to migrate data volumes smaller than 30 TB.
-   Distributed mode is suitable to migrate data volumes larger than 30 TB.

To migrate a large amount of data, deploy ossimport in distributed mode.

**Note:** You can also use data online migration to easily migrate data. For more information, see [Background information]() in Data Online Migration.

## Preparations

-   You have activated OSS and created a bucket in the China \(Hangzhou\) region.
    -   For more information about how to activate OSS, see [Activate OSS](/intl.en-US/Console User Guide/Sign up for OSS.md).
    -   For more information about how to create a bucket, see [Create buckets](/intl.en-US/Quick Start/OSS console/Create buckets.md).
-   You have configured a RAM user and granted OSS access permissions to the RAM user.

    Create a RAM user in the RAM console, authorize the RAM user to access OSS, and save the AccessKey ID and AccessKey secret. For more information, see [Create and authorize a RAM user]().

-   Optional. You have purchased an ECS instance.

    The ECS instance and OSS instance are located in the same region, which is China \(Hangzhou\). We recommend that you purchase a general-purpose ECS instance with 2 vCPU and memory of 4 GiB. We recommend that you purchase a pay-as-you-go instance if you need to release the ECS instance after the data is migrated.

    **Note:** If you want to deploy ossimport to a small number of machines, you can deploy them locally. If you want to deploy ossimport to a large number of machines, we recommend that you deploy them on an ECS instance. This example uses an ECS instance to show you how to perform a migration task.

    The number of required ECS instances is calculated based on the formula: Number of required ECS instances = X/Y/\(Z/100\). In the formula, X indicates the amount of data to be migrated. Y indicates the required duration in days. Z indicates the migration speed in Mbit/s \(about Z/100 TB of data to be migrated each day\). If the migration speed of an ECS instance reaches 200 Mbit/s \(about 2 TB of data is migrated each day\), you need to purchase 36 ECS instances \(500/7/2\) in the preceding example\).

-   You have configured ossimport properly.

    To meet the large-scale migration requirements in this example, you must build ossimport in distributed mode on ECS. For more information about the configuration and definition of distributed deployment, such as `conf/job.cfg`, `conf/sys.properties`, and concurrency control, see [Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md). For more information about operations on distributed deployment such as downloading ossimport and troubleshooting common errors during configurations, see [Distributed deployment](/intl.en-US/Tools/ossimport/Distributed deployment.md).


## Migration solution

The process of migrating data from a third-party storage service to OSS in distributed mode is as follows:

**Note:** After you deploy ossimport in the distributed environment on the ECS instance, ossimport downloads data from the Guangzhou region of COS to the ECS instance located in the China \(Hangzhou\) region. We recommend that you use the Internet when performing data migration. To use ossimport to upload data from the ECS instance to the OSS instance within the China \(Hangzhou\) region, we recommend that you use the internal network.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6354449951/p1976.png)

Costs involved during the migration process include the fees incurred when the source and destination buckets are accessed, outbound traffic fees for the source bucket, ECS instance fees, data storage fees, and time costs. If there is more than 1 TB of data to be migrated, the storage cost is proportional to the migration period. Compared with the data transfer and storage fees, fewer fees are incurred when you use ECS. Having more ECS instances shortens the migration period.

## Implementation

1.  Migrate all data last modified before T1.

    For more information, see the [Running](/intl.en-US/Tools/ossimport/Distributed deployment.md) section in Distributed deployment.

    **Note:** T1 is a Unix timestamp representing the number of milliseconds that have elapsed since the epoch time January 1, 1970, 00:00:00 UTC. You can run the date +%s command to obtain the value.

2.  Configure a mirroring-based back-to-origin rule.

    The origin keeps generating new data during the migration. To ensure business continuity and a seamless switchover, you need to configure a back-to-origin rule. When objects that are requested by end users do not exist in OSS, OSS retrieves these objects from the origin and returns the objects to end users. For more information, see [Configure back-to-origin rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Configure back-to-origin rules.md).

3.  Switch the read/write operations on the business system to OSS. At this time, the business system records time at T2.

4.  Open the job.cfg configuration file. Specify importSince=T1. Reinitiate the migration task to migrate incremental data last modified between T1 and T2.

    **Note:**

    -   After step 4 is complete, all read and write operations on your business system are switched to OSS. Data stored in the third-party storage service is only a copy of historical data, which can be retained or deleted as needed.
    -   ossimport only migrates and verifies data, but does not delete any data.

## References

For more information about ossimport, see the following topics:

[Distributed deployment](/intl.en-US/Tools/ossimport/Distributed deployment.md)

[Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md)

[FAQ](/intl.en-US/Tools/ossimport/Troubleshooting.md)

