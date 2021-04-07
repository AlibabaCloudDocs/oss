# Use CSG to attach OSS to ECS

OSS uses a flat structure to store objects in buckets. However, OSS supports folders as a concept to group objects and simplify management. To use OSS as a file system, you can use Cloud Storage Gateway \(CSG\) to mount OSS buckets to ECS instances.

-   CSG is activated and is granted access permissions on OSS, ECS, and VPC. For more information, see [Alibaba Cloud Storage Gateway](https://sgwnew.console.aliyun.com/).
-   A Virtual Private Cloud \(VPC\) and a vSwitch are created in the same region as the bucket that you want to mount. For more information, see [Work with VPCs](/intl.en-US/VPCs and vSwitchs/Work with VPCs.md) and [Create a vSwitch](/intl.en-US/VPCs and vSwitchs/Work with vSwitches.md).
-   An ECS instance is created in the same region as the bucket that you want to mount. For more information about how to create an ECS instance, see [Quick start](/intl.en-US/Quick Start/Manage an ECS instance in the console (express version).md).

-   Restore Archive or Cold Archive objects

    When you use CSG to access Archive or Cold Archive objects, the objects are automatically restored. Archive objects take about a minute to restore while Cold Archive objects take more than an hour to restore. When you use CSG to access Archive or Cold Archive objects, different results are returned based on the protocol that you use.

    -   NFS protocol: Restoration is triggered when you request to access the object. After the object is restored, the object is automatically accessed.
    -   SMB protocol: Restoration is triggered when you request to access the object. However, an error message is returned. You must initiate another request to access the object after the object is restored.
-   Supported regions

    China \(Hangzhou\), China \(Shanghai\), China \(Qingdao\), China \(Beijing\), China \(Zhangjiakou\), China \(Hohhot\), and China \(Shenzhen\)


For more information about CSG, see [What is CSG?](/intl.en-US/Overview/What is CSG?.md).

## Step 1: Configure CSG.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket that you want to mount to the ECS instance.

3.  In the left-side navigation pane, choose **Files** \> **Attach OSS Bucket to ECS**, and then click **Cloud Storage Gateway Settings**.

4.  In the **Choose/Create Project** step in the wizard, select a CSG cluster from the drop-down list and then click **Next**.

    If no cluster is available, click **Create Cluster** to create a CSG cluster. The naming conventions for clusters are the same as those for the gateways that are specified in the next step.

5.  In the **Choose/Choose Gateway** step, configure the parameters described in the following table and then click **Next**.

    |Parameter|Description|
    |---------|-----------|
    |**Gateway Name**|Enter the name of the gateway that you want to create.A gateway name can contain only letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\). The name can only start with a letter. A gateway can be up to 60 characters in length. |
    |**Network**|Select the VPC and vSwitch of the ECS instance to which you want to mount the bucket. After you select the VPC and vSwitch, the bucket can be mounted only to ECS instances in the VPC.|
    |**Specification**|Select the specification of the gateway. This depends on the capacity of the bucket you want to mount and the bandwidth required for data transmission between the bucket and the ECS instance.For example, if 10 million or fewer objects are stored in the bucket, the capacity of the bucket does not exceed 64 TB, and the required bandwidth does not exceed 1 Gbit/s, you can select **Basic**. For more information about the specifications of CSG, see [Specifications](/intl.en-US/Overview/Specifications.md). |
    |**Type**|Select **File**. The bucket is mounted to the ECS instance as an file system.|
    |**Cache Volume**|Set the size of the cache, which must be at least 40 GB.To ensure data access performance, CSG reserves storage space in ECS equivalent the size specified in this parameter to cache hot data. |

6.  In the **Configure Protocol** step, configure the parameters described in the following table and then click **Next**.

    |Parameter|Description|
    |---------|-----------|
    |**Protocol Type**|Select the protocol used by the file gateway.    -   **NFS**: Applicable for Linux systems.
    -   **SMB**: Applicable for Windows systems. |
    |**Share Name**|Specify the name of the network share used to access the mounted bucket.The network share name can contain only letters, digits, periods \(.\), underscores \(\_\), and hyphens \(-\). A network share name can be up to 32 characters in length. |
    |**User Mapping**|Specify the user that is used to access the mounted bucket from an NFS client. This parameter can be configured only when you select **NFS** for **Protocol Type**. Valid values:    -   **NONE**: All users on the NFS client are directly used to access the mounted bucket.
    -   **ROOT\_SQUASH**: The root user on the NFS client is mapped to the nfsnobody user on the NFS server to access the mounted bucket.
    -   **ALL\_SQUASH** and **ALL\_ANOMNYMOUS**: All users on the NFS client are mapped to the nfsnobody user on the NFS server to access the mounted bucket.
After the bucket is mounted to the ECS instance by using CSG, all users can read, write, and execute objects in the bucket. You can configure **User Mapping** in the following method based on your requirements:

    -   If you want to manage the access permissions on objects in the mounted bucket, select **NONE** for **User Mapping** and then use the root user to manage the access permissions. For more information, see [Step 4: Configure object access permissions.](#optionalstep)
    -   If you do not need to manage the access permissions on objects in the mounted bucket, configure any value for User Mapping. |
    |**Redirect Sync**|Select whether to synchronize the metadata of objects in the mounted bucket to on-premises disks. **Note:** If you enable this feature, all objects in the mounted bucket are scanned and API request fees are incurred. For more information about the billing methods, see [API operation calling fees](/intl.en-US/Pricing/Billing items and methods/API operation calling fees.md). |


## Step 2: Purchase the gateway.

1.  In the **Payment URL** message, click **Click the link to complete the payment**.

2.  On the purchase page of CSG, configure **Public Bandwidth** and **Quantity**, and then click **Buy Now**.

    For more information about the billing method of CSG, see [Billable items and billing methods](/intl.en-US/Pricing/Billable items and billing methods.md).


## Step 3: Mount the bucket to the ECS instance and access the bucket.

After you purchase the gateway, you can mount the bucket to the ECS instance. In the following steps, a file gateway that uses the NFS protocol is used to mount the bucket. To use a gateway that uses the SMB protocol to mount the bucket, see [t1427489.dita\#concept\_108285\_zh](/intl.en-US/User Guide (Alibaba Cloud)/File Gateway/Access shares/Access an SMB share directory.md).

1.  In the left-side navigation pane on the Buckets page, choose **Files** \> **Attach OSS Bucket to ECS**.

2.  In the gateway list, check the **Mounted Server** of the gateway that you want to use.

3.  Log on to the ECS instance to which you want to mount the bucket. The ECS instance runs the Linux system and is in the same region as the gateway.

    For more information about how to log on to the ECS instance, see [Connect to a Linux instance by using password authentication](/intl.en-US/Instance/Connect to instances/Connect to an instance by using VNC/Connect to a Linux instance by using password authentication.md).

4.  Run an NFS command to mount the bucket to the ECS instance.

    For example, if the value of Mounted Server is `172.16.0.2:/test` and the local path to which the bucket is mounted is `mnt/nfs/`, you can run the following command to mount the bucket to the ECS instance.

    ```
    mount.nfs 172.16.0.2:/test mnt/nfs/
    ```

5.  Access the mounted bucket.

    The following examples show how to use an NFS command to access the path to which the bucket is mounted:

    -   You can run the following command to list the objects in the root folder of the mounted bucket:

        ```
        ls mnt/nfs/
        ```

    -   You can download an object named example.txt in the root folder of the mounted bucket to the local root directory:

        ```
        cp mnt/nfs/example.txt  example.txt
        ```


## Step 4 \(optional\): Configure object access permissions.

1.  Log on to the NFS client as the root user.

2.  Configure access permissions on objects in the mounted bucket.

    For example, you can perform the following steps to grant read-only permissions on an object named example.txt to the nfsnobody user. By default, the UID and GID of the nfsnobody user are both `4294967294`.

    1.  Run the following command to change the user group associated with example.txt to the nfsnobody user group:

        ```
        chgrp -R 4294967294 example.txt
        ```

    2.  Run the following command to grant read-only permissions on example.txt to the nfsnobody user group:

        ```
        chmod  444 example.txt
        ```

3.  Change the value of User Mapping.

    After you configure access permissions on objects in the mounted bucket, we recommend that you change the value of **User Mapping** to **ROOT\_SQUASH**, **ALL\_SQUASH**, or **ALL\_ANOMNYMOUS** to limit the privilege of the root user.

    1.  Log on to the.

    2.  Click **Gateways**, and then click the gateway that is used to mount the bucket to the ECS instance.

    3.  Click **Share**. On the Shares page, click **Settings** in the **Actions** column corresponding to the share whose user mapping you want to modify.

    4.  In the **NFS Share Settings** dialog box, modify **User Mapping** based on your requirements.

    5.  Click **OK**.


