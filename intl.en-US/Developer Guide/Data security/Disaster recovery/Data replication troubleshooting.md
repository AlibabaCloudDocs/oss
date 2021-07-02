# Data replication troubleshooting

After you configure a cross-region replication \(CRR\) rule for a source bucket and a destination bucket, if the objects in the source bucket are not replicated to the destination bucket, check the following reasons to locate and resolve the problem.

-   Duration

    In CRR, data is asynchronously replicated in near real time. It takes several minutes to several hours to copy data from the source bucket to the destination bucket based on the size of the data. If objects in the source bucket are large in size, wait a moment and check whether the objects are replicated to the destination bucket.

-   Source bucket configurations
    -   Check whether the status of the data synchronization task is Enabled.
    -   Check whether the prefix of the objects to replicate is correctly configured.
        -   To synchronize objects whose names contained a specific prefix from the source bucket to the destination bucket, set the Prefix parameter to the prefix when you configure the data replication rule. For example, if you set the Prefix parameter to log, only objects whose names contain the log prefix, such as log/date1.txt and log/date2.txt, are replicated. Objects whose names do not contain the log prefix, such as date3.txt, are not replicated.
        -   To synchronize all objects from the source bucket to the destination bucket, set the Prefix parameter to empty.
-   Replication mechanism

    If an object in the source bucket is replicated from a bucket rather than the destination bucket based on another data replication rule, the object is not replicated to the destination bucket. For example, if you configure a data replication rule to replicate objects from Bucket A to Bucket B and another replication rule to replicate objects from Bucket B to Bucket C, objects that are replicated from Bucket A to Bucket B are not replicated to Bucket C.

-   Versioning status

    Both the source bucket and destination bucket must be versioned or unversioned.

-   Bucket policies

    To replicate objects from the source bucket to the destination bucket, you must have permissions to perform specific operations on the two buckets. If any of these operations is denied based on the bucket policies configured for the source and destination buckets, the replication task fails.

    -   If the following bucket policy is configured for the source bucket, the operations specified in the Action field are denied and the data replication task fails:

        ```
                "Effect": "Deny",
                "Action": [
                         "oss:ListObjects",
                         "oss:ListMultipartUploads",
                         "oss:ListParts",
                         "oss:GetObject",
                         "oss:GetObjectTagging",
                         "oss:GetObjectVersion"
                         ],  
        ```

    -   If the following bucket policy is configured for the destination bucket, the operations specified in the Action field are denied and the data replication task fails:

        ```
                "Effect": "Deny",
                "Action": [
                          "oss:GetObject",
                          "oss:PutObject",
                          "oss:RestoreObject",
                          "oss:DeleteObject",
                          "oss:AbortMultipartUpload",
                          "oss:ListParts",
                          "oss:PutObjectAcl",
                          "oss:GetObjectTagging",
                          "oss:PutObjectTagging",
                          "oss:DeleteObjectTagging",
                          "oss:GetObjectVersion",
                          "oss:DeleteObjectVersion",
                          "oss:PutObjectVersionAcl",
                          "oss:PutObjectVersionTagging",
                          "oss:GetObjectVersionTagging",
                          "oss:DeleteObjectVersionTagging",
                          "oss:RestoreObjectVersion"
                          ],  
        ```

    To make sure that objects can be replicated from the source bucket to the destination bucket, set the Effect field in the preceding bucket policies to `Allow` to allow the required operations on the source bucket and destination bucket.


