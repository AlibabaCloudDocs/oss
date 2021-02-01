# Access monitoring data

The monitoring service of OSS provides metrics such as the running status, performance, and metering of the system. These metrics help you track requests, analyze usage, collect statistics for business trends, and discover and diagnose system problems at your earliest opportunity. This topic describes the parameters used to query OSS monitoring data by using the API or SDK provided by Cloud Monitor.

**Note:** For more information about Cloud Monitor SDK examples, see [Cloud Monitor SDK for Java](/intl.en-US/SDK Reference/Cloud Monitor SDK for Java.md).

## Space

Space is used to specify the cloud services to be monitored. The namespace used by the monitoring service of OSS is `acs_oss_dashboard`.

The following Java SDK code provides an example on how to specify the namespace of the monitoring service of OSS:

```
DescribeMetricListRequest request = new DescribeMetricListRequest();
request.setNamespace("acs_oss_dashboard");
```

## StartTime and EndTime

StartTime and EndTime specify the time range to query monitoring data. The value range of time parameters in Cloud Monitor uses a left-open and right-closed interval in the `(StartTime, EndTime]`format. You can query data based on the range from StartTime to EndTime \(including the EndTime value\). The interval between start time and end time cannot exceed 31 days. Only data generated within the last 31 days can be queried.

The following Java SDK code provides an example on how to specify the time range:

```
// Specify the end time based on which to query monitoring data.
request.setEndTime("2019-05-13 11:06:27");
// Specify the start time based on which to query monitoring data.
request.setStartTime("2019-05-13 10:20:27");
```

## Dimensions

Dimensions specifies the name of the bucket you want to query. If you do not specify this parameter, data of the user level is queried. For more information about levels, see [Metric](#section_csl_zgk_5db).

The following Java SDK code provides an example on how to specify the name of the bucket you want to query:

```
// Specify the name of the bucket you want to query.
request.setDimensions("{\"BucketName\":\"<yourBucketName>\"}");
```

## Period

Period specifies the period during which to query metering metrics. The period for metering metrics of the OSS monitoring service is 3,600s. The period for other metrics is 60s. For more information about the metrics, see [Metric](#section_csl_zgk_5db).

The following Java SDK code provides an example on how to specify the period for a non-metering metric:

```
request.setPeriod("60");
```

## Metric

Metric specifies the metric you want to query. The following Java SDK code provides an example on how to specify the name of the metric you want to query:

```
// Set the name of the metric.
request.setMetric("<MetricName>");
```

-   The following table describes the mappings between non-metering metric names and levels.

    |Metric|Description|Unit|Level|
    |:-----|:----------|:---|:----|
    |UserAvailability|Availability|%|User level|
    |UserRequestValidRate|Valid request rate|%|User level|
    |UserTotalRequestCount|Total number of requests|N/A|User level|
    |UserValidRequestCount|Number of valid requests|N/A|User level|
    |UserInternetSend|Internet outbound traffic|Byte|User level|
    |UserInternetRecv|Internet inbound traffic|Byte|User level|
    |UserIntranetSend|Internal network outbound traffic|Byte|User level|
    |UserIntranetRecv|Internal network inbound traffic|Byte|User level|
    |UserCdnSend|CDN outbound traffic|Byte|User level|
    |UserCdnRecv|CDN inbound traffic|Byte|User level|
    |UserSyncSend|Cross-region replication \(CRR\) outbound traffic|Byte|User level|
    |UserSyncRecv|CRR inbound traffic|Byte|User level|
    |UserServerErrorCount|Server-side error requests|N/A|User level|
    |UserServerErrorRate|Server-side error request rate|%|User level|
    |UserNetworkErrorCount|Network-side error requests|N/A|User level|
    |UserNetworkErrorRate|Network-side error request rate|%|User level|
    |UserAuthorizationErrorCount|Client-side authorization error requests|N/A|User level|
    |UserAuthorizationErrorRate|Rate of client-side authorization error requests|%|User level|
    |UserResourceNotFoundErrorCount|The number of client-side error requests that indicate resources not found|N/A|User level|
    |UserResourceNotFoundErrorRate|Rate of client-side error requests that indicate resources not found|%|User level|
    |UserClientTimeoutErrorCount|Client-side timeout error requests|N/A|User level|
    |UserClientOtherErrorRate|Client-side timeout error request rate|%|User level|
    |UserClientOtherErrorCount|Other client-side error requests|N/A|User level|
    |UserClientOtherErrorRate|Rate of other client-side error requests|%|User level|
    |UserSuccessCount|Successful requests|N/A|User level|
    |UserSuccessRate|Successful request rate|%|User level|
    |UserRedirectCount|Redirect requests|N/A|User level|
    |UserRedirectRate|Redirect request rate|%|User level|
    |Availability|Availability|%|Bucket level|
    |RequestValidRate|Valid request rate|%|Bucket level|
    |TotalRequestCount|Total number of requests|N/A|Bucket level|
    |ValidRequestCount|Number of valid requests|N/A|Bucket level|
    |InternetSend|Internet outbound traffic|Byte|Bucket level|
    |InternetRecv|Internet inbound traffic|Byte|Bucket level|
    |IntranetSend|Internal network outbound traffic|Byte|Bucket level|
    |IntranetRecv|Internal network inbound traffic|Byte|Bucket level|
    |CdnSend|CDN outbound traffic|Byte|Bucket level|
    |CdnRecv|CDN inbound traffic|Byte|Bucket level|
    |SyncSend|Cross-region replication \(CRR\) outbound traffic|Byte|Bucket level|
    |SyncRecv|CRR inbound traffic|Byte|Bucket level|
    |ServerErrorCount|Server-side error requests|N/A|Bucket level|
    |ServerErrorRate|Server-side error request rate|%|Bucket level|
    |NetworkErrorCount|Network-side error requests|N/A|Bucket level|
    |NetworkErrorRate|Network-side error request rate|%|Bucket level|
    |AuthorizationErrorCount|Client-side authorization error requests|N/A|Bucket level|
    |AuthorizationErrorRate|Rate of client-side authorization error requests|%|Bucket level|
    |ResourceNotFoundErrorCount|The number of client-side error requests that indicate resources not found|N/A|Bucket level|
    |ResourceNotFoundErrorRate|Rate of client-side error requests that indicate resources not found|%|Bucket level|
    |ClientTimeoutErrorCount|Client-side timeout error requests|N/A|Bucket level|
    |ClientTimeoutErrorRate|Client-side timeout error request rate|%|Bucket level|
    |ClientOtherErrorCount|Other client-side error requests|N/A|Bucket level|
    |ClientOtherErrorRate|Rate of other client-side error requests|%|Bucket level|
    |SuccessCount|Successful requests|N/A|Bucket level|
    |SuccessRate|Successful request rate|%|Bucket level|
    |RedirectCount|Redirect requests|N/A|Bucket level|
    |RedirectRate|Redirect request rate|%|Bucket level|
    |GetObjectE2eLatency|Average end-to-end latency of GetObject requests|millisecond|Bucket level|
    |GetObjectServerLatency|Average server latency of GetObject requests|millisecond|Bucket level|
    |MaxGetObjectE2eLatency|Maximum end-to-end latency of GetObject requests|millisecond|Bucket level|
    |MaxGetObjectServerLatency|Maximum server latency of GetObject requests|millisecond|Bucket level|
    |HeadObjectE2eLatency|Average end-to-end latency of HeadObject requests|millisecond|Bucket level|
    |HeadObjectServerLatency|Average server latency of HeadObject requests|millisecond|Bucket level|
    |MaxHeadObjectE2eLatency|Maximum end-to-end latency of HeadObject requests|millisecond|Bucket level|
    |MaxHeadObjectServerLatency|Maximum server latency of HeadObject requests|millisecond|Bucket level|
    |PutObjectE2eLatency|Average end-to-end latency of PutObject requests|millisecond|Bucket level|
    |PutObjectServerLatency|Average server latency of PutObject requests|millisecond|Bucket level|
    |MaxPutObjectE2eLatency|Maximum end-to-end latency of PutObject requests|millisecond|Bucket level|
    |MaxPutObjectServerLatency|Maximum server latency of PutObject requests|millisecond|Bucket level|
    |PostObjectE2eLatency|Average end-to-end latency of PostObject requests|millisecond|Bucket level|
    |PostObjectServerLatency|Average server latency of PostObject requests|millisecond|Bucket level|
    |MaxPostObjectE2eLatency|Maximum end-to-end latency of PostObject requests|millisecond|Bucket level|
    |MaxPostObjectServerLatency|Maximum server latency of PostObject requests|millisecond|Bucket level|
    |AppendObjectE2eLatency|Average end-to-end latency of AppendObject requests|millisecond|Bucket level|
    |AppendObjectServerLatency|Average server latency of AppendObject requests|millisecond|Bucket level|
    |MaxAppendObjectE2eLatency|Maximum end-to-end latency of AppendObject requests|millisecond|Bucket level|
    |MaxAppendObjectServerLatency|Maximum server latency of AppendObject requests|millisecond|Bucket level|
    |UploadPartE2eLatency|Average end-to-end latency of UploadPart requests|millisecond|Bucket level|
    |UploadPartServerLatency|Average server latency of UploadPart requests|millisecond|Bucket level|
    |MaxUploadPartE2eLatency|Maximum end-to-end latency of UploadPart requests|millisecond|Bucket level|
    |MaxUploadPartServerLatency|Maximum server latency of UploadPart requests|millisecond|Bucket level|
    |UploadPartCopyE2eLatency|Average end-to-end latency of UploadPartCopy requests|millisecond|Bucket level|
    |UploadPartCopyServerLatency|Average server latency of UploadPartCopy requests|millisecond|Bucket level|
    |MaxUploadPartCopyE2eLatency|Maximum end-to-end latency of UploadPartCopy requests|millisecond|Bucket level|
    |MaxUploadPartCopyServerLatency|Maximum server latency of UploadPartCopy requests|millisecond|Bucket level|
    |GetObjectCount|Number of successful GetObject requests|N/A|Bucket level|
    |HeadObjectCount|Number of successful HeadObject requests|N/A|Bucket level|
    |PutObjectCount|Number of successful PutObject Requests|N/A|Bucket level|
    |PostObjectCount|Number of successful PostObject requests|N/A|Bucket level|
    |AppendObjectCount|Number of successful AppendObject requests|N/A|Bucket level|
    |UploadPartCount|Number of successful UploadPart requests|N/A|Bucket level|
    |UploadPartCopyCount|Number of successful UploadPartCopy Requests|N/A|Bucket level|
    |DeleteObjectCount|Number of successful DeleteObject requests|N/A|Bucket level|
    |DeleteObjectsCount|Number of successful DeleteObjects requests|N/A|Bucket level|

-   The following table describes the mappings between metering metric names and levels.

    |Metric|Description|Unit|Level|
    |:-----|:----------|:---|:----|
    |MeteringStorageUtilization|Storage size|Byte|If you specify Dimensions, data is queried at the bucket level. If you do not specify Dimensions, data is queried at the user level.|
    |MeteringGetRequest|Number of GET requests|N/A|
    |MeteringPutRequest|Number of PUT requests|N/A|
    |MeteringInternetTX|Metered Internet outbound traffic|Byte|
    |MeteringCdnTX|Metered CDN outbound traffic|Byte|
    |MeteringSyncRX|Metered CRR inbound traffic|Byte|


