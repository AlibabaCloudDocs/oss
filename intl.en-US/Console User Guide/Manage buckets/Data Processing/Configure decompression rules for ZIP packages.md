# Configure decompression rules for ZIP packages

OSS can automatically decompress ZIP packages uploaded to buckets. After you configure decompression rules, all ZIP packages uploaded to the folders specified in the rules are automatically decompressed.

Function Compute is activated. You can activate Function Compute on the [Function Compute details](https://www.alibabacloud.com/zh/product/function-compute?spm=a2796.7919406.3156523820.144.5fd33c37asF0wx) page.

OSS uses Function Compute to decompress uploaded ZIP packages. The following flowchart shows the decompression process.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8867549951/p38103.png)

When you upload a ZIP package that matches the configured decompression rules to OSS, Function Compute is triggered to automatically decompress the ZIP package. Objects decompressed from the ZIP package are stored in the specified folder in OSS.

## Usage notes

Take note of the following items when you use the ZIP package decompression feature:

-   ZIP package decompression is available in all regions except the following regions: China \(Heyuan\), China \(Guangzhou\), China \(Chengdu\), China \(Ulanqab\), China \(HongKong\), Malaysia \(Kuala Lumpur\), UAE \(Dubai\), and UK \(London\).
-   We recommend that you encode your object or folder names in UTF-8 or GB 2312. Otherwise, the decompressed object or folder names may be corrupted or the decompression process may be interrupted.
-   If the storage class of a ZIP package is Archive or Cold Archive, you must restore the object before it can be decompressed.
-   If the decompression of a ZIP package takes more than 10 minutes, the ZIP package fails to be decompressed.
-   ZIP package decompression is a value-added service. Function Compute calculates the fees based on the amount of time it takes to decompress ZIP packages. For more information, see [Billing methods](https://www.alibabacloud.com/help/zh/doc-detail/54301.htm).

## Configure decompression rules for ZIP packages

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  In the left-side navigation pane, click **Buckets**. On the Buckets page, click the name of the bucket to which you want to upload objects.

3.  In the left-side navigation pane, choose **Data Processing** \> **Decompress ZIP Package**.

4.  Click **Decompress ZIP Package**. In the Decompress ZIP Package panel, configure decompression rules for ZIP packages.

    -   Parameter description

        |Parameter|Required|Description|
        |---------|--------|-----------|
        |**Service Authorization**|Yes|Authorize Function Compute to read data from and write data to OSS and execute functions.Click **Authorize**. Complete authorization on the page that appears. |
        |**Trigger Authorized**|Yes|Authorize OSS to access Function Compute.Click **Authorize**. Complete authorization on the page that appears. If OSS is authorized to access Function Compute, the **Trigger Role** parameter is displayed in this section. |
        |**Prefix**|No|Specify the prefix that package names must contain to trigger Function Compute. If you upload a ZIP package whose name contains the specified prefix or upload a ZIP package to the folder specified by the prefix, Function Compute is triggered to decompress the ZIP package. If you do not specify this parameter, all uploaded ZIP packages are decompressed.**Note:** If you do not specify this parameter, the decompression tasks may be repeatedly executed. Therefore, we recommend that you specify a prefix for each decompression rule. For more information, see [How can I avoid trigger loops?](https://www.alibabacloud.com/help/zh/doc-detail/181819.htm). |
        |**Destination Directory**|No|Specify the folder in which the objects decompressed from the ZIP package are stored. If you do not specify this parameter, Function Compute decompress the ZIP package to the root folder of the current bucket. If you want to store the objects decompressed from a ZIP package to a subfolder with the same name as the package within the destination folder, select **Add the compressed object name to the destination directory**. If you want to store the objects decompressed from a ZIP package to the destination folder, select **Decompress to the destination directory**. For more information about the configuration of this parameter, see the examples in the following table.|

    -   Configuration examples

        |Scenario|Configuration|File structure after decompression|
        |--------|-------------|----------------------------------|
        |All ZIP packages uploaded to the zipfolder folder are decompressed to the destfolder folder. Subfolders with the same name as the package names are not created.|        -   Set **Prefix** to zipfolder/.
        -   Set **Destination Directory** to destfolder.
        -   Select **Decompress to the destination directory**.
|        ```
bucket  
├─── zipfolder/   
│    ├─── a.zip
│    └─── b.zip
└─── destfolder/
     ├─── a.txt
     ├─── b.txt
     └─── ...
        ``` |
        |All ZIP packages uploaded to the zipfolder folder are decompressed to the subfolders with the same name as the package names within the root folder of the bucket.|Configure the following parameters:        -   Set **Prefix** to zipfolder/.
        -   Leave **Destination Directory** unspecified.
        -   Select **Add the compressed object name to the destination directory**.
|        ```
bucket  
├─── zipfolder/   
│    ├─── a.zip
│    └─── b.zip
├─── a/
│    ├─── a.txt
│    └─── ...
└─── b/
     ├─── b.txt
     └─── ...
        ``` |
        |All ZIP packages uploaded to the zipfolder folder are decompressed to the subfolders with the same name as the package names within the destfolder folder.|Configure the following parameters:        -   Set **Prefix** to zipfolder/.
        -   Set **Destination Directory** to destfolder.
        -   Select **Add the compressed object name to the destination directory**.
|        ```
bucket  
├─── zipfolder/   
│    ├─── a.zip
│    └─── b.zip
└─── destfolder/
     ├─── a/
     │    ├─── a.txt
     │    └─── ...
     └─── b/
          ├─── b.txt
          └─── ...
        ``` |

5.  Read and select **I have read the terms of service and agreed to activate Function Compute and process compressed files by using Function Compute. Only file or folder names encoded in UTF-8 or GB 2312 can be processed.** Click **OK**.


## Modify the decompression rules for ZIP packages

You can modify the decompression rules for ZIP packages.

-   Modify the prefix
    1.  Click the Decompress ZIP Package tab.
    2.  Click **Edit** in the Actions column corresponding to the trigger you want to modify.
    3.  On the **Edit Trigger** page in the Function Compute console, modify the **Prefix** value in the **Trigger Rule** section. Keep other parameters unchanged.
    4.  Click **OK**.
-   Modify function configurations
    1.  Click the Decompress ZIP Package tab.
    2.  Click **Edit** in the Actions column corresponding to the trigger you want to modify.
    3.  On the **Edit Trigger** page in the Function Compute console, click **Cancel**.
    4.  Click the **Overview** tab. Click **Configure**.
    5.  On the **Configure Function** page, modify the function configurations.

        You can modify the **Memory**, **Timeout**, and **Environment Variables** parameters:

        -   **Memory**: You can configure this parameter based on the size of the object to be processed. If the size of the ZIP package is small, set this parameter to a small value to reduce costs when the function is executed.
        -   **Timeout**: A timeout error is displayed if the function cannot be executed during the specified period. We recommend that you configure an appropriate timeout period to avoid timeout during function execution.
        -   **Environment Variables**: If you modify this parameter value, the destination directory of the extracted objects is modified.
    6.  Click **Submit**.

## Delete decompression rules for ZIP packages

You can manually delete decompression rules for ZIP packages.

1.  Click the Decompress ZIP Package tab.

2.  Click **Edit** in the Actions column corresponding to the trigger you want to delete.

3.  On the **Edit Trigger** page, click **Cancel**.

4.  On the Triggers tab, click **Delete** in the Actions column corresponding to the trigger you want to delete.

5.  In the dialog box that appears, click **OK**.


