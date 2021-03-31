# Set OSS paths

You can manually add an OSS path or specify that the OSS path is automatically added in the OSS console to quickly access the corresponding bucket or folder.

## Manually add an OSS path

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click the Add Authorized OSS Path icon on the right side of **My OSS Path**.

3.  In the Add Path panel, configure the following parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Region**|Select the region where the bucket for which you want to set an OSS path is located.|
    |**Bucket**|Specify the name of the bucket.|
    |**Select Authorized Bucket**|Click this item. You can select the required bucket you are authorized to access in the current account.|
    |**File Path**|Specify the folder that contains the required object. Example: **examplefolder**.|

4.  Click **OK**.

    After you add an OSS path, you can click the path to access the corresponding object. You can click the ![Sticky](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101551.png) icon on the right side of a path to sticky the path to the top. You can also click the ![Delete](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101553.png) icon on the right side of a path to delete the path.


## Automatically add an OSS path

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click the ![Settings](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3667549951/p101539.png) icon on the right side of **My OSS Path**.

3.  In the Manage Paths panel, you can configure whether to automatically record the most recently accessed OSS paths and the number of preserved paths.

    |Parameter|Description|
    |---------|-----------|
    |**Most Recently Accessed**|Select whether to record the recently accessed OSS paths.|
    |**Preserved Paths**|Specify the number of recorded OSS paths. Default value: 10. Valid values: 1 to 10.|

4.  Click **OK**.

    After OSS paths are configured, OSS records the mostly accessed paths based on the specified number of preserved paths.


