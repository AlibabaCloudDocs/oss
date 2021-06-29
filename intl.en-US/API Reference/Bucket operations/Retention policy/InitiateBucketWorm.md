# InitiateBucketWorm

You can call this operation to create a retention policy.

## Usage notes

OSS supports the Write Once Read Many \(WORM\) strategy that prevents an object from being deleted or overwritten for a specified period of time. You can configure a time-based retention policy for buckets. This policy has a protection period ranging from one day to 70 years.

-   Within 24 hours after a retention policy is created for a bucket, if the retention policy is still not locked, the bucket owner and authorized users can modify or delete the policy. When a retention policy is locked, you can read objects from or upload objects to buckets. However, the objects or retention policies within the retention period cannot be deleted. You can delete objects only after their retention period ends. For more information about the retention policy, see [Retention policy](/intl.en-US/Developer Guide/Data security/Retention policy.md).
-   Versioning cannot be configured with retention policies or mirroring-based back-to-origin at the same time. For more information about the Versioning, see [Overview](/intl.en-US/Developer Guide/Data security/Versioning/Overview.md).

## Request elements

|Element|Type|Required|Description|
|-------|----|--------|-----------|
|InitiateWormConfiguration|Container|Yes|The root node. Child nodes: RetentionPeriodInDays |
|RetentionPeriodInDays|Positive integer|Yes|The number of days for which objects can be retained.|

## Examples

-   Sample requests

    ```
    POST /? worm HTTP/1.1
    Date: Thu, 15 May 2014 11:18:32 GMT
    Content-Length: 556
    Content-Type: application/xml
    Host: BucketName.oss.aliyuncs.com
    Authorization: OSS nxj7dtlhcyl5hp****:COS3OQkfQPnKmYZTEHYv2**** 
    
    <InitiateWormConfiguration>
      <RetentionPeriodInDays>365</RetentionPeriodInDays>
    </InitiateWormConfiguration>
    ```

-   Sample responses

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 5374A2880232A65C2300****
    x-oss-worm-id: 1666E2CFB2B3418****
    Date: Thu, 15 May 2014 11:18:32 GMT
    ```


