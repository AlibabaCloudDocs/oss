# Overview

Terraform is an open source automatic resource orchestration tool that supports multiple cloud service providers. Alibaba Cloud is the third largest cloud service provider. The Alibaba Cloud provider supports at least 90 resources and data sources across more than 20 services and products. An increasing number of developers contribute to the Alibaba Cloud Terraform ecosystem. For more information, visit [terraform-alicloud-provider](https://www.terraform.io/docs/providers/alicloud/index.html).

[HashiCorp Terraform](https://www.terraform.io/) is an automatic IT infrastructure orchestration tool that can use code to manage and maintain IT resources. The easy-to-use command line interface \(CLI\) of Terraform allows you to deploy configuration files on Alibaba Cloud or any other supported cloud and control the versions of the configuration files. The CLI provides code for infrastructure resources such as VMs, storage accounts, and network interfaces defined in the configuration files that describe the cloud resource topology. Terraform is a highly scalable tool that supports new infrastructures from providers. You can use Terraform to create, modify, or delete cloud resources, such as OSS objects, ECS instances, VPCs, ApsaraDB RDS instances, and SLB instances.

## Features of the OSS Terraform module

OSS Terraform Module provides bucket and object management features. Example:

-   Bucket management features:
    -   Create a bucket.
    -   Configure an ACL for a bucket.
    -   Configure Cross-Origin Resource Sharing \(CORS\) for a bucket.
    -   Configure logging for a bucket.
    -   Configure static website hosting for a bucket.
    -   Configure hotlink protection for a bucket.
    -   Configure the lifecycle rules of a bucket.
-   Object management features:
    -   Upload an object.
    -   Configure server-end encryption for an object.
    -   Configure an ACL for an object.
    -   Configure object metadata.

## References

-   For more information about how to install and use Terraform, see [Use Terraform to manage OSS](/intl.en-US/Best Practices/Terraform/Use Terraform to manage OSS.md).
-   For more information about the download address of the OSS Terraform module, visit [terraform-alicloud-modules](https://github.com/terraform-alicloud-modules/terraform-alicloud-oss-object).

-   For more information about the OSS Terraform module, visit [alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html).


