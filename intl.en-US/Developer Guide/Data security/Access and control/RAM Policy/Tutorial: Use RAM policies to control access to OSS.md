# Tutorial: Use RAM policies to control access to OSS

This tutorial demonstrates how to use RAM policies to control access to OSS buckets, folders and objects.

## Background information

RAM policies are configured based on users. You can manage users by configuring RAM policies. For users such as employees, systems, or applications, you can control which resources are accessible. For example, you can create a RAM policy to grant users read permissions on a bucket.

RAM policies are in the JSON format. A RAM policy includes the following fields:

-   Statement: the authorization statement. A RAM policy can include multiple authorization statements.
-   Effect: the effect of the policy. Valid values: Allow and Deny.

    **Note:** If a RAM policy includes an Allow statement and a Deny statement at the same time, the Deny statement takes precedence over the Allow statement.

-   Action: the authorized actions on resources.

If you use RAM policies, we recommend that you use RAM Policy Editor to generate RAM policies. For more information, see [RAM Policy Editor](/intl.en-US/Tools/RAM Policy Editor.md).

Compared with RAM policies, bucket policies can be configured in the OSS console. The bucket owner can grant other users permissions to access OSS resources. For more information, see [Use bucket policies to authorize other users to access OSS resources](/intl.en-US/Console User Guide/Upload, download, and manage objects/Add bucket policies.md).

## Buckets and folders

Alibaba Cloud OSS uses a flat data model structure instead of a hierarchical one. All objects \(files\) are stored in buckets. Therefore, OSS does not have directories and subfolders that are used in hierarchical file systems. However, you can simulate a folder hierarchy in the OSS console to group, classify, and manage objects by folders. The following figure shows some sample folders in the OSS console.

![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864585061/p178620.png)

OSS is a distributed object storage service in which objects are identified as key-value pairs. You can retrieve the content of an object based on the object name. For example, an object named oss-dg.pdf and the following three folders are stored in a bucket named ramtest-bucket: Development, Marketing, and Private.

-   When you create the Development folder, the OSS console creates an object whose key is `Development/`. A forward slash \(`/`\) is included in the key as a delimiter.
-   When you upload an object named ProjectA.docx to the Development folder, the OSS console uploads the object and sets its key to `Development/ProjectA.docx`.

    In the key, `Development` is the prefix and the forward slash \(`/`\) is the delimiter. You can retrieve a list of all objects that share a common prefix and delimiter in the bucket. In the console, click the Development folder. The console lists the objects in the folder. The following figure shows the objects in the Development folder.

    ![development](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864585061/p178622.png)

    **Note:** To list objects in the Development folder of the ramtest-bucket bucket, the console sends a request to OSS to list objects whose names include the specified prefix `Development` and a forward slash \(`/`\) as the delimiter. As a result, the console lists the objects in the folder in the same way as file systems do. In the preceding example, three objects with the following keys are stored in the ramtest-bucket bucket: `Development/Alibaba Cloud.pdf`, `Development/ProjectA.docx`, and `Development/ProjectB.docx`.


Before you go into this tutorial, you must understand the concept of root-level bucket content. In this example, the following objects are stored in ramtest-bucket bucket:

-   Development/Alibaba Cloud.pdf
-   Development/ProjectA.docx
-   Development/ProjectB.docx
-   Marketing/data2016.xlsx
-   Marketing/data2016.xlsx
-   Private/2017/images.zip
-   Private/2017/promote.pptx
-   oss-dg.pdf

The keys of these objects determine a logical hierarchy with Development, Marketing, and Private as root-level folders and oss-dg.pdf as a root-level object. When you click the bucket name in the OSS console, the common prefix and delimiter shared by multiple objects \(Development/, Marketing/, and Private/\) are displayed as root-level folders. The oss-dg.pdf object does not have a prefix. Therefore, it is displayed as an root-level object.

![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864585061/p178620.png)

## Requests and responses

Before you grant permissions to RAM users, you must understand how the OSS console interacts with OSS when you click a bucket name in the console.

-   Send a request to access a bucket

    When you click the ramtest-bucket bucket in the OSS console, the console sends a [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md) request to OSS.

    -   Sample request

        ```
        GET /? prefix=&delimiter=/ HTTP/1.1
        Host: ramtest-bucket.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        In the preceding request, the value of the prefix parameter is empty and the value of the delimiter parameter is a forward slash \(/\).

    -   Sample response

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <? xml version="1.0" encoding="UTF-8"? >
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>ramtest-bucket</Name>
        <Prefix></Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>oss-dg.pdf</Key>
                ...
            </Contents>
           <CommonPrefixes>
                <Prefix>Development</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Marketing</Prefix>
           </CommonPrefixes>
              <CommonPrefixes>
                <Prefix>Private</Prefix>
           </CommonPrefixes>
        </ListBucketResult>
        ```

    -   Response resolution

        The console resolves the response returned by OSS and displays the objects and folders in the root directory of the bucket.

        ![ram](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864585061/p178620.png)

-   Send a request to access a folder of the bucket

    When you click the Development/ folder of the ramtest-bucket bucket in the console, the console sends a [GetBucket \(ListObjects\)](/intl.en-US/API Reference/Bucket operations/Basic operations/GetBucket (ListObjects).md) request to OSS. The request includes the prefix and delimiter parameters.

    -   Sample request

        ```
        GET /? prefix=Development/&delimiter=/ HTTP/1.1
        Host: oss-example.oss-cn-hangzhou.aliyuncs.com
        Date: Fri, 24 Feb 2012 08:43:27 GMT
        Authorization: OSS qn6qrrqxo2oawuk53otf****:DNrnx7xHk3sgysx7I8U9I9IY****
        ```

        In the request, the value of the prefix parameter is `Development/` and the value of the delimiter parameter is a forward slash \(/\).

    -   Sample response

        In the response, OSS returns objects whose keys include the specified prefix.

        ```
        HTTP/1.1 200 OK
        x-oss-request-id: 534B371674E88A4D8906****
        Date: Fri, 7 Aug 2020 08:43:27 GMT
        Content-Type: application/xml
        Content-Length: 712
        Connection: keep-alive
        Server: AliyunOSS
        <? xml version="1.0" encoding="UTF-8"? >
        <ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
        <Name>ramtest-bucket</Name>
        <Prefix>Development/</Prefix>
        <Marker></Marker>
        <MaxKeys>100</MaxKeys>
        <Delimiter>/</Delimiter>
            <IsTruncated>false</IsTruncated>
            <Contents>
                <Key>ProjectA.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>ProjectB.docx</Key>
                ...
            </Contents>
            <Contents>
                <Key>Alibaba Cloud.pdf</Key>
                ...
            </Contents>
        </ListBucketResult>
        ```

    -   Response resolution

        The console resolves the response returned by OSS and displays the objects in the Development/ folder.

        ![development](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0864585061/p178622.png)


## Scenarios

You must create RAM users to perform the steps in this tutorial. Two RAM users named Anne and Leo are created by an Alibaba Cloud account in this topic as an example. For more information about how to create RAM users, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).

In this example, you must grant the following permissions to Anne and Leo:

-   Grant Anne permissions to access objects and subfolders only in the Development/ folder. For more information, see [Step 3: Grant RAM user Anne specific permissions](#section_gqp_p63_kl8).
-   Grant Leo permissions to access objects and subfolders only in the Marketing/ folder. For more information, see [Step 4: Grant RAM user Leo specific permissions](#section_fvm_dph_z1k).
-   Set the ACL of the Private/ folder to private, which indicates that all RAM users created by the Alibaba Cloud account cannot access the folder.

Based on the scenario in this topic, the following permissions must be granted to both Anne and Leo:

-   Permissions to perform oss:ListBuckets to list all buckets owned by the Alibaba Cloud account.
-   Permissions to perform oss:ListObjects to list objects and folders in the root directory of the ramtest-bucket bucket.

In this case, you can create RAM user groups to classify and authorize RAM users created by the Alibaba Cloud account for user and permission management. For more information about how to create a RAM user group and grant permissions to a user group, see [Step 2: Create a RAM user group and grant permissions to the group](#section_tu8_3br_l9p).

## Step 1: Create a bucket and upload objects to the bucket.

In this step, you can log on to the OSS console by using your Alibaba Cloud account and create a bucket. Then, create the Development, Marketing, and Private folders in the bucket and upload one or two sample objects to each folder in the console.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/) by using your Alibaba Cloud account.

2.  Create a bucket named ramtest-bucket. For more information, see [Create buckets](/intl.en-US/Console User Guide/Manage buckets/Create buckets.md).

3.  Upload an object to the root directory of the bucket. For more information, see [Upload objects](/intl.en-US/Console User Guide/Upload, download, and manage objects/Upload an object.md).

    In this example, an object named oss-dg.pdf is uploaded to the root directory of the bucket.

4.  Create the following folders in the bucket: Development, Marketing, and Private. For more information about how to create folders, see [Create folders](/intl.en-US/Console User Guide/Upload, download, and manage objects/Create folders.md).

5.  Upload one or two objects to each folder.

    In this example, the following objects are uploaded to the bucket:

    -   Development/Alibaba Cloud.pdf
    -   Development/ProjectA.docx
    -   Development/ProjectB.docx
    -   Marketing/data2016.xlsx
    -   Marketing/data2016.xlsx
    -   Private/2017/images.zip
    -   Private/2017/promote.pptx

## Step 2: Create a RAM user group and grant permissions to the group

In this topic, a user group named Staff is created as an example. For more information about how to create a user group, see [Create a RAM user group](/intl.en-US/RAM User Group Management/Create a RAM user group.md).

-   Grant RAM users permissions to list all buckets owned by the Alibaba Cloud account
    1.  Create a RAM policy.

        In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                  "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
           "Resource": [
             "acs:oss:*:*:*"
           ]
         }
        ]
        }
        ```

        For more information about how to create a RAM policy, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

    2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to the Staff group.

        For more information, see [Grant permissions to a RAM user](/intl.en-US/RAM User Management/Grant permissions to a RAM user.md).

-   Grant RAM users permissions to list items in the root directory of the bucket

    1.  Log on to the [RAM console](https://ram.console.aliyun.com).
    2.  Choose **Permissions** \> **Policies**.
    3.  Find the created `AllowGroupToSeeBucketListInConsole` policy and click the policy name.
    4.  On the page that appears, click the **Policy Document** tab.

        To list items in the bucket, the RAM user must have permissions to perform the `oss:ListObjects` operation.

        ```
        {
           "Version": "1",
           "Statement": [
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListBuckets",
                 "oss:GetBucketStat",
                 "oss:GetBucketInfo",
                 "oss:GetBucketAcl"
               ],
               "Resource": [
                 "acs:oss:*:*:*"
               ],
               "Condition": {}
             },
             {
               "Effect": "Allow",
               "Action": [
                 "oss:ListObjects"
               ],
               "Resource": [
                 "acs:oss:*:*:ramtest-bucket"
               ],
               "Condition": {
                 "StringLike": {
                   "oss:Prefix": [
                     ""
                   ],
                   "oss:Delimiter": [
                     "/"
                   ]
                 }
               }
             }
           ]
         }
        ```

    **Note:** You can modify a RAM policy for up to five times. To modify a RAM policy that is modified for five times, you must delete the policy, create a new one, and assign the new policy to the Staff group.


## Step 3: Grant RAM user Anne specific permissions

You can perform the following steps to grant RAM user Anne permissions to access all objects in the Development/ folder.

1.  In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:ramtest-bucket"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Development/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject"       
           ],
           "Resource": [
             "acs:oss:*:*:ramtest-bucket/Development/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to RAM user Anne.


To allow RAM user Anne to not only access objects in the Development/ folder but also write objects to the folder, create the following RAM policy:

```
{
   "Version": "1",
   "Statement": [
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:ramtest-bucket"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Development/*"
           ]
         }
       }
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:GetObject",
         "oss:PutObject",
         "oss:GetObjectAcl"
       ],
       "Resource": [
         "acs:oss:*:*:ramtest-bucket/Development/*"
       ],
       "Condition": {}
     }
   ]
 }
```

## Step 4: Grant RAM user Leo specific permissions

You can perform the following steps to grant RAM user Leo permissions to access all objects in the Marketing/ folder.

1.  In this topic, the following RAM policy named `AllowGroupToSeeBucketListInConsole` is created as an example.

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:ramtest-bucket"
           ],
           "Condition": {
             "StringEquals": {
               "oss:Prefix": [
                 "Marketing/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject"        
           ],
           "Resource": [
             "acs:oss:*:*:ramtest-bucket/Marketing/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

2.  Assign the `AllowGroupToSeeBucketListInConsole` policy to RAM user Leo.


## Step 5: Prohibit RAM users from accessing the Private folder

In this example, you must set the ACL of the Private/ folder to private, which indicates that all RAM users created by the Alibaba Cloud account cannot access the folder. Therefore, you must add a policy that explicitly denies access to the Private folder. This policy takes precedence over other permissions. You can perform the following steps to add the policy.

-   Add the following statement to explicitly deny any access to the Private folder \(ramtest-bucket/Private/\*\):

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:*"
        ],
        "Resource": [
          "acs:oss:*:*:ramtest-bucket/Private/*"
        ],
        "Condition": {}
      }
    ```

-   Deny `ListObjects` requests sent to access objects whose names contain the Private prefix.

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:ListObjects"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ],
        "Condition": {
          "StringEquals": {
            "oss:Prefix": [
              "Private/"
            ]
          }
        }
      }
    ```

    After the preceding policy is added, OSS returns an error when the RAM users send requests to list the Private/2017/images.zip and Private/2017/promote.pptx objects in the Private folder.

-   Replace the policy `AllowGroupToSeeBucketListInConsole` for the Staff group with the updated policy that contains the preceding deny statements.

    After the updated policy is applied, none of the RAM users in the user group can access the Private folder in your bucket.

    ```
    {
    "Version": "1",
    "Statement": [
     {
       "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketStat",
             "oss:GetBucketInfo",
             "oss:GetBucketAcl"
           ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:ramtest-bucket"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
              ""
            ],
           "oss:Delimiter": [
             "/"
           ]
         }
       }
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:*"
       ],
       "Resource": [
         "acs:oss:*:*:ramtest-bucket/Private/*"
       ],
       "Condition": {}
     },
     {
       "Effect": "Deny",
       "Action": [
         "oss:ListObjects"
       ],
       "Resource": [
         "acs:oss:*:*:*"
       ],
       "Condition": {
         "StringEquals": {
           "oss:Prefix": [
             "Private/"
           ]
         }
       }
     }
    ]
    }
    ```


