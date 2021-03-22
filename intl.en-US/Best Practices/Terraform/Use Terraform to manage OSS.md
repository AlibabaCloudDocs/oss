# Use Terraform to manage OSS

This topic describes how to install, configure, and use Terraform to manage OSS.

## Install and configure Terraform

Before you use Terraform, perform the following steps to install and configure Terraform:

1.  Download the software package applicable to your operating system from [Download Terraform](https://www.terraform.io/downloads.html).

    In this topic, Terraform is installed and configured in a Linux operating system as an example.

2.  Decompress the package to /usr/local/bin.

    To decompress the executable file to another directory, you must add the path to global variables.

3.  Run Terraform to verify the path configuration. If a list of available Terraform options is displayed, the installation is complete.

    ```
    [root@test bin]#terraform
    Usage: terraform [-version] [-help] <command> [args]
    ```

4.  Create a RAM user and grant permissions to the user.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com/#/overview).

    2.  Create a RAM user named Terraform. Create an AccessKey pair for the user.

        For more information, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).

    3.  Authorize the RAM user.

        You can add relevant management permissions to the Terraform RAM user. For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

    **Note:** For security reasons, do not use the AccessKey pair of your Alibaba Cloud account to configure Terraform.

5.  Create a test directory.

    You must create a separate working directory for each Terraform project. In this example, create a test directory named terraform-test.

    ```
    [root@test bin]#mkdir terraform-test
    ```

6.  Go to the Terraform-test directory.

    ```
    [root@test bin]#cd terraform-test
    [root@test terraform-test]#
    ```

7.  Create a configuration file.

    Terraform reads all \*.tf and \*.tfvars files in the directory when Terraform runns. You can write configuration information to different files. Frequently used configuration files:

    ```
    provider.tf                -- used to configure providers
    terraform.tfvars      -- used to configure the variables required to configure providers
    variable.tf                  -- used to configure universal variables
    resource.tf                -- used to define resources
    data.tf                        -- used to define package files
    output.tf                    -- used to configure the output
    ```

    For example, when you create the provider.tf file, you can configure your authentication information based on the following format:

    ```
    [root@test terraform-test]# vim provider.tf
    provider "alicloud" {
        region           = "cn-beijing"
        access_key  = "LTA**********NO2"
        secret_key   = "MOk8x0*********************wwff"
    }
    ```

    For more information about configurations, see [alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html).

8.  Initialize your working directory.

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

    **Note:** After you create a working directory and the configuration file for a Terraform project, you must initialize the working directory.


You can use Terraform after you complete the preceding steps.

## Use Terraform to manage OSS

After Terraform is installed, you can run Terraform commands to manage OSS. Some common command:

-   terraform plan: You can run this command to view the operations to be performed by a configuration file.

    Assume that you add a configuration file named test.tf that is used to create a bucket:

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    You can run the terraform plan command to view the operations to be performed by the test.tf configuration file.

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

-   terraform apply: You can run this command to execute a configuration file in the working directory.

    To create a bucket named figo-chen-2020, you must add a configuration file named test.tf.

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    Run the terraform apply command to execute the configuration file.

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

    **Note:** After you execute the configuration file, a new bucket is created if the figo-chen-2020 bucket does not exist. If the figo-chen-2020 bucket exists and contains no data, the existing bucket is overwritten by the new bucket.

-   terraform destroy: You can run this command to delete an empty bucket created by Terraform.
-   terraform import: You can run this command to import a bucket that is not created by Terraform.

    To run this command, create a file named main.tf and write information about the bucket:

    ```
    [root@test terraform-test]#vim main.tf
    resource "alicloud_oss_bucket" "bucket" { 
     bucket = "test-hangzhou-2025" 
     acl = "private"
    }
    ```

    Run the following command to import the test-hangzhou-2025 bucket:

    ```
    terraform import alicloud_oss_bucket.bucket test-hangzhou-2025
    ```


## References

-   For more information about bucket configuration examples, see [alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html).
-   For more information about object configuration examples, see [alicloud\_oss\_bucket\_object](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket_object.html).

