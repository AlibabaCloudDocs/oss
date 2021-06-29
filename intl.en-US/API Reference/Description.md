# Description

Object Storage Service \(OSS\) is a secure, cost-effective, and highly reliable services provided by Alibaba Cloud to store large amounts of data. You can upload data to and download data from OSS anytime, anywhere, and on any Internet device by using the simple RESTful APIs. OSS enables you to develop a variety of services that are capable of processing large amounts of data. These services include multimedia sharing websites, online storage, and personal and corporate data backups.

## Limits

OSS imposes limits on the resources and features you use. For more information, see [Limits](/intl.en-US/Product Introduction/Limits.md).

## Usage notes

This topic describes the request syntax, parameters, sample requests, and sample responses for OSS API. If you want to perform additional development, we recommend that you use OSS SDKs. For more information about the installation and usage of OSS SDKs, see [Introduction](/intl.en-US/SDK Reference/Introduction.md).

## OSS pricing

For more information about the price of OSS, see [Object Storage Service Pricing](https://www.alibabacloud.com/product/oss#pricing).

## Terms

|Term|Name|Description|
|----|----|-----------|
|Bucket|Bucket|The container used to store objects in OSS. Every object is contained in a bucket.|
|Object|Object|The fundamental entities stored in OSS. Objects are also known as files. An object has metadata, data, and key. The key is the unique object name in a bucket.|
|Region|Region|The physical location of an OSS data center. You can select a region for data storage based on costs and request sources. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|Endpoint|Endpoint|The domain name used to access OSS. OSS provides external services by using HTTP-based RESTful APIs. Different regions use different endpoints. For the same region, access over the internal network or over the Internet also uses different endpoints. For more information, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).|
|AccessKey pair|AccessKey|An AccessKey \(AK\) pair consists of an AccessKey ID and an AccessKey secret. The AccessKey pair is used to perform access identity verification. OSS uses an AccessKey pair, which includes an AccessKey ID and an AccessKey secret to implement symmetric encryption and verify the identity of a requester. The AccessKey ID is used to verify the identity of the user, while the AccessKey secret is used to encrypt and verify the signature string. You must keep your AccessKey secret strictly confidential.|

