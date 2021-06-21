# Basic configurations of mirroring-based back-to-origin

Mirroring-based back-to-origin is used to seamlessly migrate data to OSS. This feature allows you to migrate a service that already runs on an origin that you create or on another cloud service to OSS without service interruptions. You can use mirroring-based back-to-origin rules during migration to obtain the data that is not migrated to OSS. This ensures service continuity.

## Procedure

When a requester accesses an object in the specified bucket but the object does not exist, you can specify the URL of the object in the origin and back-to-origin conditions to obtain the object. For example, you have a bucket named example that is located in the China \(Hangzhou\) region. You want your requester to obtain the required object in the examplefolder folder from `https://www.example.com/` when the requester accesses the object that does not exist in the examplefolder folder of the bucket root folder. To configure a back-to-origin rule, perform the following steps:

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  In the left-side navigation pane, choose **Basic Settings** \> **Back-to-Origin**.

4.  Click **Configure**. Click **Create Rule**.

5.  In the **Create Rule** panel, set **Mode** to **Mirroring**.

6.  Configure **Prerequisites** and **Origin URL**.

    |Parameter|Description|
    |---------|-----------|
    |**Prerequisite**|Select **File Name Prefix**, and then set File Name Prefix to **examplefolder/**. **Note:** File Name Prefix and File Name Suffix are optional. When you configure multiple back-to-origin rules, you must set different file name prefixes or suffixes to differentiate back-to-origin rules. |
    |**Origin URL**|Set the first column to **https**, the second column to **www.example.com**, and the third column to **examplefolder**.|

7.  Click **OK**.

    Access process after the back-to-origin rule is configured:

    1.  A requester accesses `https://examplebucket.oss-cn-hangzhou.aliyuncs.com/examplefolder/example.txt` for the first time.
    2.  If the examplefolder/example.txt object does not exist in examplebucket, OSS sends a request to `https://www.example.com/examplefolder/example.txt`.
    3.  If the required object is obtained, OSS writes the example.txt object to the examplefolder folder in examplebucket and then returns the object to the requester. If the required object is not obtained, HTTP status code 404 is returned to the requester.

## References

For more information about mirroring-based back-to-origin and some special scenarios, see [Special configurations of mirroring-based back-to-origin](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Special configurations of mirroring-based back-to-origin.md).

