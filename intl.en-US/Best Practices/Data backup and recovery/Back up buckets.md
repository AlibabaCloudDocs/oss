# Back up buckets

Alibaba Cloud offers multiple backup methods for data in OSS to suit different scenarios. This topic describes methods used to back up OSS data on the cloud.

## Back up data by using scheduled backup

You can create a backup plan in Hybrid Backup Recovery \(HBR\) to back up your OSS data. This allows you to recover your data when the data is lost due to an unintended modification or deletion. You can also use HBR to store historical OSS data for an extended period of time at low cost. For more information, see [Configure scheduled backup](/intl.en-US/Console User Guide/Upload, download, and manage objects/Configure scheduled backup.md).

## Back up data by using CRR

Cross-region replication \(CRR\) enables the automatic and asynchronous \(near real-time\) replication of objects across buckets in different OSS regions. Operations such as the creation, overwriting, and deletion of objects can be synchronized from a source bucket to a destination bucket. For more information, see [Configure CRR](/intl.en-US/Console User Guide/Manage buckets/Redundancy for fault tolerance/Configure CRR.md).

## Back up data by using Data Online Migration

Alibaba Cloud Data Online Migration is used as a data channel between different storage services. You can migrate data from third-party data stores to OSS or between OSS buckets by using Data Online Migration. For more information, see [Background information]().

## Back up data by using the ossimport tool

ossimport is a tool used to migrate data to OSS. You can deploy ossimport on the local server or ECS instance in the cloud to migrate data stored locally or in other cloud storage systems to OSS. For more information, see [Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

