# Special configurations of mirroring-based back-to-origin

This topic describes how to configure mirroring-based back-to-origin rules in several special scenarios.

For more information about how to configure mirroring-based back-to-origin rules, see [t2010746.md\#]().

## Example 1

Customer A has a bucket named examplebucketA located in the China \(Hangzhou\) region. In this scenario, the following requirements must be met:

-   When a requester accesses an object in the examplefolder folder but the object does not exist, the request is redirected to `https://example.com` to access the required object in the destfolder folder.
-   Some objects whose names start with forward slashes \(/\) are stored in the origin. These objects must be obtained and then stored in examplebucketA.
-   The MD5 hashes of the objects in the origin must be checked. If the MD5 hashes of the objects in the origin do not match the MD5 hashes calculated by OSS, these objects cannot be stored in examplebucketA.

The following table describes the parameter configurations.

|Parameter|Description|
|---------|-----------|
|**Prerequisite**|Select **File Name Prefix**, and set File Name Prefix to **examplefolder/**.|
|**Replace or Delete File Prefix**|Select **Replace or Delete File Prefix**, and set Replace or Delete File Prefix to **destfolder/**.|
|**Origin URL**|Set the first column to **https** and the second column to **example.com**. Leave the third column empty.|
|**Keep Forward Slash in Origin URL**|Select **Keep Forward Slash \(/\) in Origin URL**. When the names of objects in the origin start with a forward slash \(/\), OSS stores the objects in the bucket after OSS deletes forward slashes \(/\).

**Note:** This feature is in public preview in the US \(Silicon Valley\), US \(Virginia\), and China \(Hangzhou\) regions. To apply for a trial, [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|**MD5 Verification**|Select **Perform MD5 verification**. When the response to the redirect request contains the Content-MD5 header value, OSS checks whether the MD5 hash of the object from the origin matches the Content-MD5 header value.

-   If the calculated MD5 hash of the object matches the Content-MD5 header value, the client obtains the object, and OSS saves the object by using mirroring-based back-to-origin.
-   If the calculated MD5 hash of the object does not match the Content-MD5 header value, OSS does not save the object although the object is returned to the client. Therefore, the client can still obtain the object. |

Access process after the rule is configured:

1.  A requester accesses `https://examplebucketA.oss-cn-hangzhou.aliyuncs.com///examplefolder/example.txt` for the first time.
2.  If examplebucketA does not contain the //examplefolder/example.txt object, OSS redirects the request to `https://example.com///destfolder/example.txt` to obtain the object.
3.  After the required object is obtained, OSS performs the following operations:
    -   If the response to the redirect request contains the Content-MD5 field, OSS calculates the MD5 hash of the object from the origin, and matches the calculated MD5 hash with the Content-MD5 field value. If the calculated MD5 hash matches the Content-MD5 field value, the object is renamed examplefolder/example.txt. The renamed object is stored in examplebucketA and returned to the requester. If the calculated MD5 hash does not match the Content-MD5 field value, the object is returned only to the requester. The object is not stored in examplebucketA.
    -   If the response to the redirect request does not contain the Content-MD5 field, OSS renames the required object examplefolder/example.txt, stores the renamed object in examplebucketA, and then returns the renamed object to the requester.

## Example 2

Customer B has a bucket named examplebucketB in the China \(Beijing\) region, and two origins, which are Origin A \(`https://exampleA.com`\), and Origin B \(`https://exampleB.com`\). The two origins have the same folders. In this scenario, the following requirements must be met:

-   When a requester accesses an object in the pathA/example folder but the object does not exist, the object is obtained from the example folder of `https://exampleA.com`.
-   When a requester accesses an object in the pathB/example folder but the object does not exist, the object is obtained from the example folder of `https://exampleB.com`.
-   Redirection policies are set for specific objects of the two origins. The redirection policies must be followed to obtain the final data and store the obtained data in exampleBucketB.

Two mirroring-based back-to-origin rules must be configured for this scenario. Configuration descriptions:

-   Rule 1

    |Parameter|Description|
    |---------|-----------|
    |**Prerequisite**|Select **File Name Prefix**, and set File Name Prefix to **A/example/**.|
    |**Replace or Delete File Prefix**|Select **Replace or Delete File Prefix**, and set Replace or Delete File Prefix to **example/**.|
    |**Origin URL**|Set the first column to **https** and the second column to **exampleA.com**. Leave the third column empty.|
    |**3xx Response**|Select **Follow Origin to Redirect Request**. **Note:** If **Follow Origin to Redirect Request** is not selected, OSS directly returns the URL specified in the redirection rule to the requester. |

-   Rule 2

    |Parameter|Description|
    |---------|-----------|
    |**Prerequisite**|Select **File Name Prefix**, and set File Name Prefix to **B/example/**.|
    |**Replace or Delete File Prefix**|Select **Replace or Delete File Prefix**, and set Replace or Delete File Prefix to **example/**.|
    |**Origin URL**|Set the first column to **https** and the second column to **exampleB.com**. Leave the third column empty.|
    |**3xx Response**|Select **Follow Origin to Redirect Request**.|


Access process after the rule is configured:

1.  A requester accesses `https://examplebucketB.oss-cn-beijing.aliyuncs.com/A/example/example.txt` for the first time.
2.  If the A/example/example.txt object does not exist in examplebucketA, OSS redirects the request to `https://exampleA.com/example/example.txt` to obtain the object.
3.  The request result varies based on whether a redirection rule is set for the origin.
    -   If a redirection rule is set for the example/example.txt folder of Origin A, OSS sends a new request to the URL specified in the redirection rule for the origin, renames the object A/example/example.txt after the object is obtained, stores the renamed object in examplebucketA, and then returns the renamed object to the requester.
    -   If no redirection rule is set for the example/example.txt folder of Origin A, OSS renames the object A/example/example.txt after the object is obtained, stores the renamed object in examplebucketA, and then returns the renamed object to the requester.

If a requester accesses `https://examplebucketB.oss-cn-beijing.aliyuncs.com/B/example/example.txt`, the object obtained by using the back-to-origin rule is stored in the B/example folder of examplebucketB.

## Example 3

Customer C has two buckets named examplebucketC and examplebucketD located in the China \(Shanghai\) region. The ACL of examplebucketC is public read. The ACL of examplebucketD is private. In this scenario, the following requirements must be met:

-   When a requester accesses an object in the examplefolder folder of the root folder of examplebucketC but the object does not exist, the required object can be obtained from the examplefolder folder of examplebucketD.
-   Allow the query string included in the URL of the required object to be transferred to the origin.
-   Allow the `headerA`, `headerB`, and `headerC` HTTP headers included in the URL of the required object to be transferred to the origin.

The following table describes the parameter configurations.

|Parameter|Description|
|---------|-----------|
|**Prerequisite**|Select **File Name Prefix**, and set File Name Prefix to **examplefolder/**.|
|**Type of Source**|Select **OSS Private Bucket** and select **examplebucketD** from the **Source Bucket** drop-down list. **Note:** When you configure **Type of Source**, OSS generates the `AliyunOSSMirrorDefaultRole` role and grants the role the read-only permissions \(AliyunOSSReadOnlyAccess\) on all buckets. |
|**Origin URL**|Set the first column to **https**. Leave other columns empty.|
|**Other Parameter**|Select **Transfer queryString**. OSS transfers the query string included in the URL of the required object to the origin. |
|**Set Transmission Rule of HTTP Header**|Select **Transmit Specified HTTP Header Parameters** and add the **headerA**, **headerB**, and **headerC** headers. Back-to-origin rules do not support some HTTP headers such as `authorization`, `authorization2`, `range`, `content-length`, and `date` and HTTP headers whose names start with `x-oss-`, `oss-`, and `x-drs-`. |

Access process after the rule is configured:

1.  A requester accesses `https://examplebucketC.oss-cn-shanghai.aliyuncs.com/examplefolder/example.png?caller=lucas&production=oss` for the first time.
2.  If the examplefolder/example.png object does not exist in examplebucketC, OSS sends a request to `https://examplebucketD.oss-cn-shanghai.aliyuncs.com/examplefolder/example.png?caller=lucas&production=oss` to obtain the object.
3.  Access information of examplebucketD is collected based on the transferred `?caller=lucas&production=oss` parameter. The example.png object is returned to OSS.
4.  OSS renames the obtained object examplefolder/example.png and stores the renamed object in examplebucketC.

If the request includes the **headerA**, **headerB**, and **headerC** HTTP headers, these headers are also transferred to examplebucketD.

