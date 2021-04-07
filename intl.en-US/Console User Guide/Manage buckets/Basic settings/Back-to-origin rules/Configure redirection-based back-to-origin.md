# Configure redirection-based back-to-origin

After you configure redirection-based back-to-origin rules for a bucket, if an error occurs when a requester accesses the bucket, OSS redirects the request to the origin specified by the redirection-based back-to-origin rules. You can use this feature to redirect requests for objects and develop various services based on redirection.

## Procedure

If an error occurs when your visitor accesses a bucket, you can specify the URL of the object in the origin and back-to-origin conditions to redirect the access to the origin. For example, you have a bucket named example that is located in the China \(Hangzhou\) region. You want a requester to be redirected to obtain the required object in the examplefolder folder from `https://www.example.com/` when the requester accesses the object in the examplefolder folder of the bucket root folder but the object does not exist.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Back-to-Origin**.

4.  Click **Configure**. Click **Create Rule**.

5.  In the **Create Rule** panel, set **Mode** to **Redirection**.

6.  Configure **Prerequisite** and **Origin URL**.

    |Parameter|Description|
    |---------|-----------|
    |**Prerequisite**|    -   Select **HTTP Status Code**, and then set HTTP Status Code to **404**.

Valid values of HTTP status codes: 400 to 599. For more information about the error information of various HTTP status codes, see [Common errors and troubleshooting methods](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).

    -   Select **File Name Prefix**, and then set File Name Prefix to **examplefolder/**.

**Note:** File Name Prefix and File Name Suffix are optional. When you configure multiple back-to-origin rules, you must set different file name prefixes or suffixes to differentiate back-to-origin rules. |
    |**Origin URL**|Select **Add Prefix or Suffix**. Set the first column to **https** and the second column to **www.example.com**. Leave another column empty.|

7.  Click **OK**.

    Access process after the back-to-origin rule is configured:

    1.  A requester accesses `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/examplefolder/example.txt` for the first time.
    2.  If the examplefolder/example.txt object does not exist in examplebucket , OSS returns HTTP status code 301 to the requester and provides `https://www.example.com/examplefolder/example.txt` for redirection.
    3.  The requester accesses `https://www.example.com/examplefolder/example.txt`.
    The following table describes the parameters you can configure for different scenarios.

    |Scenario|Parameter|
    |--------|---------|
    |An object name prefix is different from the prefix of the name of the object in the origin.|Select **Replace or Delete File Prefix**, and set the third column of **Origin URL**. OSS replaces the **File Name Prefix** content with the content in the third column of **Origin URL**. You can configure this item after you configure **File Name Prefix**. |
    |Transfer the query string included in a request from OSS to the origin.|Select **Transfer queryString**.|
    |An HTTP redirect code must be replaced.|The default HTTP redirect code of a redirection rule is 301. You can select **302** or **307** from the **Redirection Code** drop-down list.|
    |A redirect request is from Alibaba Cloud CDN.|Specify whether to select **Source from Alibaba Cloud CDN**. When the redirect request is from Alibaba Cloud CDN: If you select **Source from Alibaba Cloud CDN**, CDN automatically follows redirection rules and then obtains content. If you do not select **Source from Alibaba Cloud CDN**, CDN automatically returns the URL for redirection to the client. |


