# Service monitoring, diagnosis, and troubleshooting

Despite reducing users’ costs of infrastructure construction and O&M cloud applications compared to traditional applications, cloud applications have complicated monitoring, diagnosis, and troubleshooting. The OSS storage service provides a wide array of monitoring and log information, helping you fully understand program behavior and promptly discover and locate problems.

**Overview**

This chapter instructs you how to monitor, diagnose, and troubleshoot OSS problems by using the OSS monitoring service, logging, and other third-party tools, helping you achieve the following goals:

-   Monitors in real time the running status and performance of OSS and provides prompt alarm notifications.
-   Provides effective methods and tools to help you locate problems.
-   Provides methods to help you quickly solve common OSS-related problems.

This chapter is organized as follows:

-   [OSS real-time monitoring](#section_k51_nlk_5db): Describes how to use the OSS monitoring service to continuously monitor the running status and performance of OSS.
-   [Tracking and diagnosis](#section_ixw_f4k_5db): Describes how to use the OSS monitoring service and logging function to diagnose problems, and how to associate the relevant information in log files for tracking and diagnosis.
-   [Troubleshooting](#section_hmn_1pk_5db): Describes typical problems and corresponding troubleshooting methods.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7795688951/p1222.jpg)

## OSS monitoring

Overall operating conditions

-   Availability and percentage of valid requests

    This is an important indicator related to system stability and the ability of users to correctly use the system. Any value lower than 100% indicates that some requests have failed.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2912700061/p1223.jpg)

    Availability may also temporarily fall below 100% due to system optimization factors, such as partition migration for load balancing. In these cases, OSS SDKs can provide relevant retry mechanisms to handle this type of intermittent failure, keeping the service end unware.

    Also, when the percentage of valid requests falls below 100%, you must analyze the issue based on your own usage. You can use request distribution statistics or request status details to determine the actual types of request errors. Then, you can use [Tracking and Diagnosis](#section_ixw_f4k_5db) to determine the cause and perform [Troubleshooting](#section_hmn_1pk_5db). In some business scenarios, a valid request rate is expected to fall below 100%. For example, you may need to first check that an object exists and then perform a certain operation based on the existence of the object. In this case, if the object does not exist, the read request that checks its existence returns a 404 error code \(resource does not exist error\). This inevitably produces a valid request rate of less than 100%.

    For businesses that require high system availability, you can set an alarm rule that is triggered when the indicator falls below the expected threshold value.

-   Total No. of requests and No. of valid requests

    This indicator reflects the system operation status from the perspective of the total traffic volume. When the No. of valid requests is not equal to the total No. of requests, this indicates that some requests have failed.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2912700061/p1224.jpg)

    You can watch the fluctuations in the total No. of requests and No. of valid requests, especially when they sharply increase or decrease. In such cases, follow-up action is required. You can set alarm rules to make sure you receive prompt notifications. For periodic businesses, you can set periodic alarm rules \(periodic alarms will be available soon\). For more information, see [Alarm Service User Guide](/intl.en-US/Developer Guide/Monitoring service/Alert service.md).

-   Request status distribution statistics

    When availability or the valid request rate falls below 100% \(or the No. of valid requests is not equal to the total No. of requests\), you can look at the request status distribution statistics to quickly determine the request error types. For more information about this metric indicator, see [OSS Metric Indicator Reference Manual](/intl.en-US/Developer Guide/Monitoring service/Metrics.md).

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7795688951/p1225.jpg)


Request status details monitoring

Request status details provides more details about the request monitoring status on the basis of request status distribution statistics. They let you monitor certain types of requests in more detail.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2912700061/p1226.jpg)

Performance monitoring

The monitoring service provides the following metric items that can be used as indicators for performance monitoring.

-   Average latency: E2E average latency and Server average latency

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2912700061/p1227.png)

-   Maximum latency: E2E maximum latency and Server maximum latency

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2912700061/p1228.png)

-   Successful request categories

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3912700061/p1229.jpg)

-   Traffic

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3912700061/p1230.jpg)

    The preceding metric items \(except for ‘Traffic’\) implement categorized monitoring based on API operation types:

    -   GetObject
    -   HeadObject
    -   PutObject
    -   PostObject
    -   AppendObject
    -   UploadPart
    -   UploadPartCopy
    The latency indicators show the average or maximum time needed for API operation types to process requests. E2E latency is the indicator for end-to-end latency. Besides the time needed to process requests, it also includes the time needed to read requests and send responses, and the delay caused by network transmission. Server latency only includes the time needed to process the requests on the server, not the client-side transmission network latency. Therefore, if the E2E latency suddenly increases but the server latency does not change significantly, you can determine that the poor performance has been caused by network instability, instead of an OSS system fault.

    In addition to the APIs mentioned previously, "successful request operation categories" also monitors the quantity of requests for the following two API operation types:

    -   DeleteObject
    -   Deleteobjects
    The traffic indicator is used to monitor the overall situation for a user or a specific bucket. It looks at the usage of network resources in Internet, intranet, CDN origin retrieval, cross-domain replication, and other such scenarios.

    For performance-type indicators, we must focus on sudden and abnormal changes, such as when the average latency suddenly spikes or remains above the normal request latency baseline for a long period of time. You can set alarm rules that correspond to performance indicators, so that the relevant personnel are immediately notified if an indicator falls below or exceeds a threshold value. For businesses with periodic peaks and troughs, you can set periodic alarm rules for week on week, day on day, or hour on hour comparisons \(periodic alarms will be available soon\).


Billing monitoring

At press time, the OSS monitoring service can only monitor storage space, outbound Internet traffic, Put requests, and Get requests \(not including cross-domain replication outbound traffic and CDN outbound traffic\). It does not support alarm setting or API read operations for billing data.

The OSS monitoring service collects bucket-level billing monitoring data on an hourly basis. In the monitoring view for a specific bucket, you can see graphs of continuous monitoring trends. Using the monitoring view, you can analyze your businesses’ OSS resource usage trends and estimate future costs. See the following figure:

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/3912700061/p1231.png)

The OSS monitoring service also provides statistics on the quantity of user and bucket-level resources consumed each month. For example, the total amount of OSS resources consumed by an account or bucket starting from the 1st day of the month. These statistics are updated hourly. This increases your understanding of your resource usage and computation fees for the current month in real time, as shown in the following figure:

as shown in the following figure:

**Note:** In the monitoring service, the provided billing data is pushed to the maximum extent possible, but this may cause some discrepancies with the actual bill amount. Please note that the Billing Center data is used in actual billing applications.

## Tracking and diagnosis

Problem diagnosis

-   Performance diagnosis

    Many subjective factors are involved in the determination of application performance. You must use the satisfaction of your business needs in your specific business scenario as a baseline, to determine if a performance problem occurs. Also, when a client initiates a request, factors that may cause performance problems may come from anywhere in the request chain. For example, problems may be caused by OSS overloads, client TCP configuration problems, or traffic bottlenecks in the basic network architecture.

    Therefore, when diagnosing performance problems, you must first set a reasonable baseline. Then, you use the performance indicators provided by the monitoring service to determine the potential root cause of any performance problem. Next, you find detailed information in the relevant logs to help you further diagnose and troubleshoot any faults.

    In the [Troubleshooting](#section_hmn_1pk_5db) section, we give examples of many common performance problems and troubleshooting measures. This can be used as a reference.

-   Error diagnosis

    When requests from client applications are at fault, the clients receive error information from the server. The monitoring service records these errors and shows statistics for the various types of errors that may affect requests. You can also retrieve detailed information for individual requests from the server log, client log, and network log. Generally, the returned HTTP status code, OSS error code, and OSS error information can indicate the cause of the request failure.

    For error response information details, see [OSS error responses](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md).

-   Using the logging function

    OSS provides a server logging function for user requests. This helps you track end-to-end detailed request logs.

    For instructions on the activation and use of the logging function, see [Set logging](/intl.en-US/Console User Guide/Manage buckets/Manage logs/Configure logging.md).

    For more information on Log Service naming rules and record formats, see [Set access logging](/intl.en-US/Developer Guide/Manage logs/Logging.md).

-   Using network logging tools

    In many situations, you can diagnose problems by using the logging function to record storage log and client application log data. However, in certain situations, you may need more details by using network logging tools.

    This is because capturing traffic exchanged between clients and the server can give you more detailed information on the data exchanged between clients and server and the underlying network conditions, which can help you investigate problems. For example, in some situations, user requests may report an error, but no request can be seen in the server log. In such cases, you can use the records logged by the OSS logging function to see if the cause of the problem lies with the client, or you can use network monitoring tools to check for a network problem.

    [Wireshark](http://www.wireshark.org/) is one of the most common network log analysis tools. This free protocol analyzer runs on the packet level and provides a view of detailed packet information for various network protocols. This can help you troubleshoot packet loss and connection problems.

    see [Wireshark User Guide](http://www.wireshark.org/docs/wsug_html_chunked/).


E2E tracking and diagnosis

Requests are initiated by a client application process and pass through the network environment to the OSS server, where they are processed. Then, a response is sent by the server over the network environment and received by the client. This is an end-to-end tracking process. Associating client application logs, network tracking logs, and server logs provides detailed information for you to troubleshoot the root cause of a problem and discover potential problems.

In OSS, the provided RequestIDs serve as identifiers used to associate the information from various logs. In addition, the log timestamps not only allow you to quickly query specific log time ranges, but can also show you the time points when request events and other client application, network, and service system events occurred during this period. This helps you analyze and investigate problems.

-   RequestID

    Whenever the OSS receives a request, it allocates it a unique server request ID, its RequestID. In different logs, the RequestID is located in different fields:

    -   In server logs recorded by the OSS logging function, the RequestID is located in the “Request ID” column.
    -   In the process of network tracking \(for example, when using Wireshark to capture data streams\), the RequestID is the x-oss-request-id header value in the response message.
    -   In client applications, you must use the client code to manually print the RequestID in the client log. At the press time, the latest Java SDK version already supported printing RequestID information for normal requests. You can use the getRequestId operation to retrieve RequestIDs from the results returned by different APIs. All OSS SDK versions allow you to print RequestIDs for abnormal requests. You can call the OSSException’s getRequestId method to obtain this information.
-   Timestamps

    You can use timestamps to find relevant log entries. You must note that there may be some deviations between the client time and server time. On a client, you can use timestamps to search for server log entries recorded by the logging function. For this, you must add or subtract 15 minutes.


## Troubleshooting

Common performance-related problems

-   High average E2E latency, with low average server latency

    We have already discussed the differences between average E2E latency and average server latency. Therefore, we can say that high E2E latency and low server latency are caused by two possible reasons:

    -   Slow client application response speed
    -   Network factors
    A slow client application response speed can be caused by several possible reasons:

    -   Limited number of available connections or threads

        -   Use the relevant command to check if the system has a large number of connections in the TIME\_WAIT status. If yes, adjust the core parameters to solve this problem.
        -   When the number of available threads is limited, first check for bottlenecks affecting the client CPU, memory, network, or other resources. If no bottleneck is found, increase the number of concurrent threads properly.
        -   If the problem persists, you have to optimize the client code. For example, you can use an asynchronous access method. You can also use the performance analysis function to analyze client application hotspots, and then perform the necessary optimization.
    -   Insufficient resources, such as CPU, memory, or bandwidth

        For this type of problem, you must first use the relevant system monitoring function to find client resource bottlenecks. Then, optimize the client code to rationalize resource usage or increase the client resources \(increase the number of cores or the memory\).

    Investigate network latency problems

    Generally, high E2E latency due to network factors is temporary. You can use Wireshark to investigate temporary and persistent network problems, such as packet loss problems.

-   Low average E2E latency, low average server latency, but high client request latency

    When the client experiences high request latency, the most probable cause is that the requests are not reaching the server. Therefore, we must find out why the client requests are not arriving at the server.

    Two client-side factors can cause high client request sending latency:

    -   A limited number of available connections or threads: see the solution described in the preceding section.
    -   Client requests are retried multiple times: In this situation, you must find and solve the cause of the request retries based on the retry information. You can follow these steps to determine if the client has a retry problem:

        -   Check the client log. The detailed log entries indicate if retries have occurred. Using the OSS Java SDK as an example, you can search for the following warn or info-level log entries. If such entries are found in the log, this indicates that requests have been retried.

            ```
            [Server]Unable to execute HTTP request:
               Or
              [Client]Unable to execute HTTP request:
            ```

        -   If the client log level is debug, search for the following log entries \(again we are using the OSS Java SDK as an example\). If such entries exist, this indicates requests have been retried.

            ```
            Retrying on
            ```

    If no problem with the client occurs, you must check for potential network problems, such as packet loss. You can use a tool such as Wireshark to investigate network problems.

-   High average server latency

    If the server latency during downloads or uploads is high, this may be caused by the following two factors:

    -   A large number of clients are frequently accessing the same small object.

        In this situation, you can view the server log recorded by the logging function to determine if a small object or a group of small objects are being frequently accessed in a short period of time.

        For download scenarios, we suggest you activate the CDN service for this bucket, to improve performance. This also reduces your traffic fees. In the case of upload, you may consider revoking write permissions for this object \(bucket\), if this does not affect your business.

    -   Internal system factors

        For internal system problems or problems that cannot be solved through optimization, please provide our system staff with the RequestIDs in your client logs or in the logs recorded by the logging function, and they can help you solve the problem.


Server errors

When the number of server-side errors increases, two scenarios must to be considered:

-   Temporary increase

    For this type of problem, you must adjust the retry policy in the client program and adopt a reasonable concession mechanism, such as exponential backoff. This not only avoids temporary service unavailability due to system optimization, upgrades, and other such operations \(such as partition migration for system load balancing\), but also avoids high pressure during business peaks.

-   Permanent increase

    When the number of server-side errors sustainably increases, please provide our back-end staff with the RequestIDs in your client logs or in the logs recorded by the logging function, and they can help you find the problem.


Network errors

Network errors occur when the server is processing a request and the connection is lost \(not due to a server-side issue\), so the HTTP request header cannot be returned. In such a situation, the system records an [HTTP Status Code of 499](https://httpstatuses.com/499)for this request. In the following situations, the server may change the request status code to 499:

-   Before processing a received read/write request, if the server detects that the connection is unavailable, the request is recorded as 499.
-   When the server is processing a request and the client preemptively closes the connection, the request is recorded as 499.

In summary, a network error occurs during the request process when a client independently closes the request or the client is disconnected from the network. If the client independently closes requests, you can check the client code, to identify the cause and time of the client’s disconnection from OSS. When the client loses its network connection, you can use a tool such as Wireshark to investigate network connection problems.

Client errors

-   Increase in client authorization errors

    If you detect an increase in client authorization errors or the client receives a large number of 403 request errors, this is most commonly caused by the following problems:

    -   The bucket domain name accessed by the user is incorrect.

        -   If the user uses a third-level or second-level domain name to access a bucket, this may cause a 403 error if the bucket is not in the region indicated by the domain name. For example, if you have created a bucket in the Hangzhou region, but a user attempts to access it using the domain name Bucket.oss-cn-shanghai.aliyuncs.com. In this case, you must confirm the bucket’s region and then correct the domain name information.
        -   If you have activated the CDN acceleration service, this problem may occur when CDN binds an incorrect origin retrieval domain name. In this case, check that the CDN origin retrieval domain name is the bucket’s third-level domain name.
        -   If you encounter 403 errors when using JavaScript clients, this may be caused by a problem in the CORS \(Cross-Origin Resource Sharing\) settings, because web browsers implement “same source policy” security restrictions. In this case, you must check the bucket’s CORS settings and correct any errors. For information about CORS settings, see [CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md).
    -   Access control problems can be divided into four types:

        -   When you use a primary AK for access, you must check the AK settings for errors if the AK is invalid.
        -   When you use a RAM sub-account for access, you must check that the sub-account is using the correct sub-account AK and that the sub-account has the relevant permissions.
        -   When you use temporary STS tokens for access, you must confirm that the temporary token has not expired. If the token has expired, apply for a new one.
        -   If you use bucket or object settings for access control, you must check that the bucket or object to be accessed supports the relevant operations.
    -   When you authorize third-party downloads \(using signed URLs to access OSS resources\), if access was previously normal and then suddenly reports a 403 error, it is likely that the URL has expired.

    -   When RAM sub-accounts use OSS utilities, this may also produce 403 errors. These utilities include ossftp, ossbrowser, and the OSS console client. When you enter the relevant AK information during logon and the system throws an error, if you entered the correct AK, you must check that the AK is a sub-account AK and that this sub-account has permission for GetService and other operations.
-   Increase in client-side ‘resource does not exist’ errors

    When the client receives a 404 error, this means that you are attempting to access a resource or information that does not exist. When the monitoring service detects an increase in ‘resource does not exist’ errors, this is most likely caused by one of the following problems:

    -   Service usage: For example, when you first need to check that an object exists before performing another operation and you call the doesObjectExist method \(using the Java SDK as an example\), if the object does not exist, the client receives the value "false". However, the server actually produces a 404 request error. Therefore, in this business scenario, 404 errors are normal.

    -   The client or another process previously deleted this object. You can confirm this problem by searching for the relevant object operation in the server log recorded by the logging function.

    -   Network faults case packet loss and retries. For example, the client may initiate a delete operation to delete a certain object. The request reaches the server and successfully executes the delete operation. However, if the response packet is lost during transmission on the network, the client initiates a retry. This second request then produces a 404 error. You can confirm that network problems are producing 404 errors using the client log and server log:

        -   Check for retry requests in the client application log.
        -   Check if the server log shows two delete operations for this object and that the first delete operation has an HTTP status of 2xx.
-   Low valid request rate and high number of other client-side request errors

    The valid request rate is the number of requests that return an HTTP status code of 2xx/3xx as a percentage of total requests. Status codes of 4XX or 5XX indicate a failed request and reduce the valid request rate. Other client-side request errors indicate requests errors other than the following: server errors \(5xx\), network errors \(499\), client authorization errors \(403\), resource does not exist errors \(404\), and client time-out errors \(408 or OSS error code: RequestTimeout 400\).

    Check the server log recorded by the logging function to determine the specific errors encountered by these requests. You can see [OSS error responses](/intl.en-US/Developer Guide/Troubleshooting/Error responses.md) to find a list of common error codes returned by OSS. Then, check the client code to find and solve the specific cause of these errors.


Abnormal increase in storage capacity

If storage capacity increases abnormally without a corresponding increase in upload requests, this is generally caused by a delete problem. In such a case, check for the following two factors:

-   When the client application uses a specific process to regularly delete storage objects to free up space: The investigation processes for this request are as follows:
    1.  Check if the valid request rate has decreased, because a failed delete request may cause storage objects to fail to be deleted as expected.
    2.  Find the specific cause for the decrease in the valid request rate by looking at the error types of the requests. Then, you can combine the specific client logs to see the detailed error information \(for example, the STS temporary token used to free up storage space may have expired\).
-   When the client sets a LifeCycle to delete storage objects: Use the console or an API to check that the current bucket LifeCycle value is the same as before. If not, modify the configuration and use the server log recorded by the logging function to find information on the previous modification of this value. If the LifeCycle is normal but inactive, contact an OSS system administrator to help identify the problem.

Other OSS problems

If the Troubleshooting section did not cover your problem, use one of the following methods to diagnose and troubleshoot the problem.

1.  View the OSS monitoring service, to see if there have been any changes compared to the expected baseline behavior. Using the monitoring view, you may be able to determine if this problem is temporary or permanent and which storage operations are affected.
2.  The monitoring information can help you search the server log data recorded by the logging function, to find information on any errors that may have occurred when the problem started. This information may be able to help you find and solve the problem.
3.  If the information in the server log is insufficient, use the client long to investigate the client application, or use a network tool such as Wireshark to check your network for problems.

