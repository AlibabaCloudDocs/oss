# Configure OSS accelerator

OSS accelerator caches hot-spot objects stored in OSS and provides high-performance and high-throughput data access services. It is suitable for scenarios in which a large amount of bandwidth and repeated read operations are required, such as gene training, machine learning, data lakes, and big data computing.

**Note:** OSS accelerator is in public preview in the China \(Shanghai\) region. Contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for a trial.

## Configure an accelerator

1.  Create an accelerator.

    1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

    2.  Click **OSS Accelerator**. On the page that appears, click **Create Accelerator**.

    3.  On the Create Accelerator panel, configure the parameters described in the following table.

        |Parameter|Description|
        |---------|-----------|
        |**Accelerator Name**|Specify the name of the accelerator. The accelerator name must be 3 to 63 bytes in length.|
        |**Zone**|Select the zone of the accelerator.|
        |**Accelerated Domain Name**|The domain name used to access the accelerator.|
        |**Capacity**|Specify the capacity of the accelerator cache. Unit: TB.The value of this parameter must be greater than 20 and be a multiple of 5. |
        |**Throughput**|The throughput of the accelerator, which is calculated based on the following formula:`Throughput = Accelerator Capacity (TB) x 200 MB/s/TB` |

    4.  Click **OK**.

2.  Configure a policy for the accelerator.

    1.  Click **Configure Accelerated Paths** to the right of the created accelerator.

    2.  On the **Configure Accelerated Paths** panel, configure the parameters described in the following table.

        |Parameter|Description|
        |---------|-----------|
        |**Bucket Name**|Select the bucket for which you want to configure the accelerator.**Note:** If an accelerator is already configured for the bucket, the previous accelerator configurations are overwritten. |
        |**Applied To**|Select one of the following policies for the accelerator:        -   **Specific Paths**: Specify the paths that you want to accelerate. You can configure up to 10 paths. Only access to objects in the configured paths is accelerated. For example, if you want to accelerate the access to objects in the example folder in the root folder, specify the path as example/.
        -   **Entire Bucket**: Access to all objects in the bucket is accelerated. |

    3.  Click **Save**.

        You can also click **Add** to add multiple bucket for which you want to configure the accelerator by performing the preceding steps.


## Modify the accelerator capacity

You can scale in or out an accelerator.

1.  On the **OSS Accelerator** page, click **Edit** in the Action column corresponding to the accelerator that you want to modify.

2.  On the Modify Accelerator panel, modify the **Capacity** of the accelerator.

    The capacity of an accelerator must be greater than 20 TB and be a multiple of 5 TB. It takes about one minute to scale out an accelerator and one hour to scale in.


