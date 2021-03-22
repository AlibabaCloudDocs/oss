# 使用Terraform管理OSS

本文介绍Terraform的安装和配置详情，以及如何使用Terraform来管理OSS。

## 安装和配置Terraform

使用Terraform前，您需要按照以下步骤安装并配置Terraform。

1.  前往[Terraform官网](https://www.terraform.io/downloads.html)下载适用于您的操作系统的程序包。

    本文以Linux系统为例。

2.  将程序包解压到/usr/local/bin。

    如果将可执行文件解压到其他目录，则需要将路径加入到全局变量。

3.  运行Terraform验证路径配置，若显示可用的Terraform选项的列表，表示安装完成。

    ```
    [root@test bin]#terraform
    Usage: terraform [-version] [-help] <command> [args]
    ```

4.  创建RAM用户，并为其授权。

    1.  登录[RAM控制台](https://ram.console.aliyun.com/#/overview)。

    2.  创建名为Terraform的RAM用户，并为该用户创建 AccessKey。

        具体步骤参见[创建RAM用户](/cn.zh-CN/用户管理/创建RAM用户.md)。

    3.  为RAM用户授权。

        您可以根据实际的情况为Terraform授予合适的管理权限。具体步骤参见[为RAM用户授权](/cn.zh-CN/用户管理/为RAM用户授权.md)。

    **说明：** 请不要使用主账号的AccessKey配置Terraform工具。

5.  创建测试目录。

    因为每个 Terraform 项目都需要创建1个独立的工作目录，所以先创建一个测试目录terraform-test。

    ```
    [root@test bin]#mkdir terraform-test
    ```

6.  进入terraform-test目录。

    ```
    [root@test bin]#cd terraform-test
    [root@test terraform-test]#
    ```

7.  创建配置文件。

    Terraform在运行时，会读取该目录空间下所有 \*.tf和\*.tfvars文件。因此，您可以按照实际用途将配置信息写入到不同的文件中。下面列出几个常用的配置文件：

    ```
    provider.tf                -- provider 配置
    terraform.tfvars      -- 配置 provider 要用到的变量
    variable.tf                  -- 通用变量
    resource.tf                -- 资源定义
    data.tf                        -- 包文件定义
    output.tf                    -- 输出
    ```

    例如创建provider.tf文件时，您可按以下格式配置您的身份认证信息：

    ```
    [root@test terraform-test]# vim provider.tf
    provider "alicloud" {
        region           = "cn-beijing"
        access_key  = "LTA**********NO2"
        secret_key   = "MOk8x0*********************wwff"
    }
    ```

    更多配置信息请参见[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)。

8.  初始化工作目录。

    ```
    [root@test terraform-test]#terraform init
    
    
    Initializing provider plugins...
    - Checking for available provider plugins on https://releases.hashicorp.com...
    - Downloading plugin for provider "alicloud" (1.25.0)...
    
    
    
    
    The following providers do not have any version constraints in configuration,
    so the latest version was installed.
    
    
    To prevent automatic upgrades to new major versions that may contain breaking
    changes, it is recommended to add version = "..." constraints to the
    corresponding provider blocks in configuration, with the constraint strings
    suggested below.
    
    
    * provider.alicloud: version = "~> 1.25"
    
    
    Terraform has been successfully initialized!
    
    
    You may now begin working with Terraform. Try running "terraform plan" to see
    any changes that are required for your infrastructure. All Terraform commands
    should now work.
    
    
    If you ever set or change modules or backend configuration for Terraform,
    rerun this command to reinitialize your working directory. If you forget, other
    commands will detect it and remind you to do so if necessary.
    
                            
    ```

    **说明：** 每个Terraform项目在新建Terraform工作目录并创建配置文件后，都需要初始化工作目录。


以上操作完成之后，您就可以使用Terraform工具了。

## 使用Terraform管理OSS

Terraform安装完成之后，您就可以通过Terraform 的操作命令管理 OSS 了，下面介绍几个常用的操作命令：

-   terraform plan：预览功能，允许在正式执行配置文件之前，查看将要执行哪些操作。

    例如，您添加了创建Bucket的配置文件test.tf：

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    使用terraform plan可查看到将会执行的操作。

    ```
    [root@test terraform-test]# terraform plan
    Refreshing Terraform state in-memory prior to plan...
    The refreshed state will be used to calculate this plan, but will not be
    persisted to local or remote state storage.
    
    
    ------------------------------------------------------------------------
    
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
    
    Terraform will perform the following actions:
    
      + alicloud_oss_bucket.bucket-acl
          id:                <computed>
          acl:               "private"
          bucket:            "figo-chen-2020"
          creation_date:     <computed>
          extranet_endpoint: <computed>
          intranet_endpoint: <computed>
          location:          <computed>
          logging_isenable:  "true"
          owner:             <computed>
          referer_config.#:  <computed>
          storage_class:     <computed>
    
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    
    ------------------------------------------------------------------------
    
    Note: You didn't specify an "-out" parameter to save this plan, so Terraform
    can't guarantee that exactly these actions will be performed if
    "terraform apply" is subsequently run.
    ```

-   terraform apply：执行工作目录中的配置文件。

    例如您想创建名为figo-chen-2020的Bucket，您需要先添加创建 Bucket 的配置文件 test.tf。

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    之后使用terraform apply命令执行配置文件即可。

    ```
    [root@test terraform-test]#terraform  apply
    
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
    
    Terraform will perform the following actions:
    
      + alicloud_oss_bucket.bucket-acl
          id:                <computed>
          acl:               "private"
          bucket:            "figo-chen-2020"
          creation_date:     <computed>
          extranet_endpoint: <computed>
          intranet_endpoint: <computed>
          location:          <computed>
          logging_isenable:  "true"
          owner:             <computed>
          referer_config.#:  <computed>
          storage_class:     <computed>
    
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    
    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
    
      Enter a value: yes
    
    alicloud_oss_bucket.bucket-acl: Creating...
      acl:               "" => "private"
      bucket:            "" => "figo-chen-2020"
      creation_date:     "" => "<computed>"
      extranet_endpoint: "" => "<computed>"
      intranet_endpoint: "" => "<computed>"
      location:          "" => "<computed>"
      logging_isenable:  "" => "true"
      owner:             "" => "<computed>"
      referer_config.#:  "" => "<computed>"
      storage_class:     "" => "<computed>"
    alicloud_oss_bucket.bucket-acl: Creation complete after 1s (ID: figo-chen-2020)
    
    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
    ```

    **说明：** 此配置运行后，若figo-chen-2020这个Bucket不存在，则创建一个Bucket；若已存在，且为Terraform创建的空Bucket，则会删除原有 Bucket 并重新生成。

-   terraform destroy：可删除通过Terraform创建的空 Bucket。
-   导入 Bucket：若 Bucket 不是通过 Terraform 创建，可通过命令导入现有的Bucket。

    首先，创建名为main.tf 的文件，并写入Bucket相关信息：

    ```
    [root@test terraform-test]#vim main.tf
    resource "alicloud_oss_bucket" "bucket" { 
     bucket = "test-hangzhou-2025" 
     acl = "private"
    }
    ```

    使用如下命令导入test-hangzhou-2025这个Bucket：

    ```
    terraform import alicloud_oss_bucket.bucket test-hangzhou-2025
    ```


## 参考文档

-   更多Bucket配置操作示例请参见[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)。
-   更多Object相关配置操作示例请参见[alicloud\_oss\_bucket\_object](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket_object.html)。

