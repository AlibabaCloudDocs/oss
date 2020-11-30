# Configure CSG

OSS allows you to configure Cloud Storage Gateway \(CSG\) for your buckets. After CSG is configured for a bucket, the bucket can be attached to an ECS instance as a file system or storage volume. This topic describes how to configure CSG for buckets in the OSS console.

To configure CSG for a bucket in the OSS console, you must make sure that:

**Note:**

-   CSG is supported for Standard and Infrequent Access \(IA\) buckets. For Archive and \(Cold Archive\)buckets, only write operations are supported.
-   CSG is available in the China \(Hangzhou\), China \(Shanghai\), China \(Qingdao\), China \(Beijing\), China \(Zhangjiakou\), China \(Hohhot\), and China \(Shenzhen\) regions.

## Step 1: Create a gateway cluster

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Files** \> **Attach OSS Bucket to ECS**, and then click **Cloud Storage Gateway Settings**.

4.  In the Create Cloud Storage Gateway dialog box, click **Create Cluster** in the **Choose Create Project** step.

    You can select an available cluster from the drop-down list.

5.  In the **Create Cloud Storage Gateway** dialog box, specify **Cluster Name** \(required\) and **Cluster Remarks** \(optional\).

6.  Click **OK**.

7.  Select the cluster you just created and click **Next**.


## Step 2: Configure the gateway and protocol

1.  In the Create Cloud Storage Gateway dialog box, specify **Gateway Name** and configure the gateway in the **Choose/Choose Gateway** step. Click **Next**.

    ![gateway](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0077549951/p57178.png)

    The following section describes the parameters:

    -   **Network**: Select the VPC and VSwitch of the ECS instance to which you want to attach the bucket. After the gateway is configured, the bucket can be mounted only to ECS instances in the VPC.
    -   **Specification**: Select the specification for the gateway to create. Valid values: **Basic**, **Standard**, and **Enhanced**.
    -   **Type**: Select the type of the gateway to create. Valid values:
        -   **File**: If a file gateway is configured, the bucket can be mounted as a file system to an ECS instance by using the NFS or SMB protocol.
        -   **Block**: If a block gateway is configured, the bucket can be mounted as a storage volume to an ECS instance by using the iSCSI protocol.
    -   **Cache Volume**: CSG uses local storage to cache hot data to ensure the performance of data access. You can manually set the cache volume.

        **Note:** The cache volume of a file gateway cannot be smaller than 40 GB. The cache volume of a block gateway must be no less than 20 GB.

2.  In the Create Cloud Storage Gateway dialog box, perform the following configurations based on the gateway type in the **Configure Protocol** step. Click **Next**:

    -   If you select **File** in the previous step, the following configuration page is displayed.

        ![gateway3NFS](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0077549951/p57197.png)

        The following section describes the parameters:

        -   **Protocol Type**: Select the type of the protocol for use by the file gateway. Valid values: **NFS** and **SMB**. The NFS protocol allows you to access the mounted OSS resources in Linux, and the SMB protocol allows you to access the mounted OSS resources in Windows.
        -   **Share Name**: Specify the name of the network share after the bucket is mounted.
        -   **User Mapping**: Specify the mapping between users on the NFS client and users on the NFS server. Configure this parameter only when you select NFS for Protocol Type. Valid values:
            -   **NONE**: No user on the NFS client is mapped to the nfsnobody user on the NFS server.
            -   **ROOT\_SQUASH**: The root user on the NFS client is mapped to the nfsnobody user on the NFS server.
            -   **ALL\_SQUASH**: All users on the NFS client are mapped to the nfsnobody user on the NFS server.
            -   **ALL\_ANOMNYMOUS**: All users on the NFS client are mapped to the anonymous users on the NFS server.
        -   **Redirect Sync**: Choose whether to synchronize OSS metadata to your local device.
    -   If you select **Block** in the previous step, you must set **Volume Name** for the bucket to mount as a storage volume. The following configuration page is displayed.

        ![gatewat3iSCSI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0077549951/p57205.png)


## Step 3: Make payment to activate CSG

1.  In the **Payment URL** message, click **Click the link to complete the payment.**

2.  On the Cloud Storage Gateway buy page, set **Quantity** and **Billing Cycle**. Click **Buy Now**.

3.  After the payment is made, the system goes back to the bucket details page. When Cloud Storage Gateway starts to run, the details of the gateway are displayed on the page. Based on these details, you can mount a bucket to your ECS instance and access data in the bucket.


