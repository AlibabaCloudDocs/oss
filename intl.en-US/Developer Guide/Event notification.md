# Event notification

You can configure event notification rules in the OSS console for objects that you want to monitor. If the events that you specified in the rules occur on these objects, OSS immediately sends you a notification.

## Limits

-   The event notification feature is available in all regions except for the China \(Heyuan\), China \(Guangzhou\), China \(Hohhot\), China \(Ulanqab\), UAE \(Dubai\), and Malaysia \(Kuala Lumpur\)
-   You can configure up to 10 event notification rules in a region.
-   Notifications are not sent for TS and M3U8 objects that are generated in RTMP-based stream ingest. For more information about RTMP-based stream ingest, see[Overview](/intl.en-US/API Reference/LiveChannel-related operations/Overview.md).

## Usage notes

If an operation performed on your OSS resources triggers an event notification rule that you configure, Message Service \(MNS\) sends the information about the operation to the HTTP server or MNS queue that you specify in the rule. The following figure shows the detailed process of event notification.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8795688951/p63749.png)

## Events

|Event|Description|
|-----|-----------|
|PutObject|An object is created or overwritten by using simple upload.|
|PostObject|An object is created or overwritten by using form upload.|
|CopyObject|An object is created or overwritten by copying an object.|
|InitiateMultipartUpload|A multipart upload task is initiated.|
|UploadPart|An object is created or overwritten by uploading parts.|
|UploadPartCopy|An object is created or overwritten by using multipart upload.|
|CompleteMultipartUpload|A multipart upload task is complete.|
|AppendObject|An object is created or appended by using append upload.|
|GetObject|An object is obtained by using simple download.|
|DeleteObject|A single object is deleted.|
|DeleteObjects|Multiple objects are deleted.|
|ObjectReplication:ObjectCreated|An object is created by using cross-region replication \(CRR\).|
|ObjectReplication:ObjectRemoved|An object is deleted by using CRR.|
|ObjectReplication:ObjectModified|An object is overwritten by using CRR.|
|ObjectCreatedGroup|An object is created or overwritten.|
|ObjectDownloadedGroup|An object is obtained.|
|ObjectRemovedGroup|An object is deleted.|

**Note:** The ObjectCreatedGroup, ObjectDownloadedGroup, and ObjectRemovedGroup events are available only in the China \(Hong Kong\), US \(Silicon Valley\), US \(Virginia\), Germany \(Frankfurt\), Australia \(Sydney\), Singapore, and UK \(London\) regions.

## Notifications

The content of OSS-based event notification is in the JSON format and is Base64-encoded. The following code provides an example of the decoded content:

```
{"events": [
      {
        "eventName": "",  // The event name.
        "eventSource": "", // The resources that trigger event notification. Valid value: acs:oss.
        "eventTime": "", // The time when the event occurred. The returned data is a timestamp that complies with ISO 8601.
        "eventVersion": "", // The version of event notification. The current version is 1.0.
        "oss": {
            "bucket": {
                "arn": "", // The ARN of the bucket, which is in the following format: acs:oss:region:uid:bucketname.
                "name": "", // The name of the bucket for which the event notification rule is configured.
                "ownerIdentity": "" // The owner of the bucket.
            }, 
            "object": {
                "deltaSize": "", // The size difference between the new object and the original object. If you create an object, the value of this parameter is the same as the size of the object. If you overwrite an object, the value is the difference between the sizes of the new object and the original object. If the size of the original object is smaller than the size of the new object, the difference is a negative value.
                "eTag": "", // Specify the ETag value of the object.
                "key": "", // The name of the object.
                "position": "", // This parameter applies only to the ObjectCreated:AppendObject event and indicates the position from which the AppendObject operation starts. The first AppendObject operation on an object starts from byte 0 of the object.
                "readFrom": "", // This parameter applies only to the ObjectDownloaded:GetObject event and indicates the position from which the GetObject operation starts. If the GetObject operation is not performed by range, the value of this parameter is 0. Otherwise, the value is the byte from which the GetObject operation starts.
                "readTo": "", // This parameter applies only to the ObjectDownloaded:GetObject event and indicates the position at which the last GetObject operation ends. If the GetObject operation is not performed by range, the value of this parameter is the object size. Otherwise, the value is the byte next to the byte at which the last GetObject operation ends.
                "size": "" // The size of the object.
                }, 
        "ossSchemaVersion": "", // The version of the schema. The current value is 1.0.
        "ruleId": "GetObject", // The ID of the rule that matches the event.
        "region": "", // The region in which the bucket is located.
        "requestParameters": {
            "sourceIPAddress": "" // The source IP address from which the request is sent.
            }, 
        "responseElements": {
            "requestId": "" // The ID of the request.
            }, 
        "userIdentity": {
            "principalId": "" // The UID of the requester.
            }, 
        "xVars": {  // Custom parameters for OSS callback.
            "x:callback-var1":"value1",
            "x:vallback-var2":"value2"
            }
        }        
     }
  ]
}
```

The following code provides an example of a notification:

```
{"events": [
      {
        "eventName": "ObjectDownloaded:GetObject",
        "eventSource": "acs:oss",
        "eventTime": "2016-07-01T11:17:30.000Z",
        "eventVersion": "1.0",
        "oss": {
            "bucket": {
                "arn": "acs:oss:cn-shenzhen:114895646818****:event-notification-test-shenzhen",
                "name": "event-notification-test-shenzhen",
                "ownerIdentity": "114895646818****"},
            "object": {
                "deltaSize": 0,
                "eTag": "0CC175B9C0F1B6468E1199E269772661",
                "key": "test",
                "readFrom": 0,
                "readTo": 1,
                "size": 1
            },
        "ossSchemaVersion": "1.0",
        "ruleId": "GetObjectRule",
        "region": "cn-shenzhen",
        "requestParameters": {
            "sourceIPAddress": "198.51.100.1"
            },
        "responseElements": {
            "requestId": "5FF16B65F05BC932307A3C3C"
            },
        "userIdentity": {
            "principalId": "114895646818****"
            },
        "xVars": {
            "x:callback-var1":"value1",
            "x:vallback-var2":"value2"
            }
        }        
     }
  ]
}
```

## Configuration methods

You can enable event notification in the OSS console. For more information, see [Configure event notification rules](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Configure event notification rules.md).

