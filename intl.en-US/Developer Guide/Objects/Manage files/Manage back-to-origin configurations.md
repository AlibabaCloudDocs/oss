# Manage back-to-origin configurations

If you access data in a bucket that has no back-to-origin rules configured and the data does not exist, 404 Not Found is returned. However, if you configure back-to-origin rules that contain a valid origin for the bucket, you can obtain the data based on the rules.

You can configure mirroring-based or redirection-based back-to-origin rules for hot migration and specific request redirection. For more information about API operations used to configure back-to-origin rules, see [PutBucketWebsite](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.md).

## Usage notes

-   You can configure up to 20 back-to-origin rules for a bucket. The rules are used to match a request in a sequence of their [RuleNumber](/intl.en-US/API Reference/Bucket operations/Static websites/PutBucketWebsite.mdtable_fjp_2as_60m) values. If a request matches a rule, subsequent rules are not used to match the request. Object Storage Service \(OSS\) determines whether a request matches a back-to-origin rule based on whether the request meets the conditions specified in the rule. OSS does not check whether a request can obtain the requested object from the origin.
-   The origin URL in a back-to-origin rule cannot be set to an address of the internal network.
-   Back-to-origin rules and versioning cannot be configured for a bucket at the same time.
-   In regions within China, the default queries per second \(QPS\) of mirroring-based back-to-origin is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin is 1,000, and the default bandwidth is 1 Gbit/s.
-   You can add Image Processing \(IMG\) parameters to origin URLs in mirroring-based back-to-origin rules. However, snapshot parameters cannot be added to origin URLs in mirroring-based back-to-origin rules.
-   By default, the timeout period of a mirroring-based back-to-origin request is 10 seconds.

## Implementation methods

|Implementation method|Description|
|---------------------|-----------|
|[Console](/intl.en-US/Console User Guide/Manage buckets/Basic settings/Back-to-origin rules/Overview.md)|A user-friendly and intuitive web application|
|[ossutil](/intl.en-US/Tools/ossutil/Common commands/website.md)|A high-performance command-line tool|

## Mirroring-based back-to-origin

When you call the GetObject operation to query an object that does not exist in a bucket, OSS sends a request to the origin URL specified in the back-to-origin rule to retrieve the object. After OSS retrieves the object, OSS stores the object in the bucket and returns the object to you. The following figure shows the detailed process of mirroring-based back-to-origin.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6182364951/p1580.png)

-   Scenarios

    Mirroring-based back-to-origin rules are used to seamlessly migrate data to OSS. This feature allows you to migrate services that already run on a user-created origin or in another cloud service to OSS without interrupting services. You can use mirroring-based back-to-origin to obtain the data that is not migrated to OSS during migration. This ensures service continuity. For detailed examples, see [Seamlessly migrate data from a cloud service to OSS]().

-   Detail analysis
    -   Trigger conditions

        OSS performs mirroring-based back-to-origin to retrieve an object from the origin only when 404 is returned for GetObject.

    -   Naming conventions

        The URL used by OSS to obtain an object is in the following format: `http(s)://MirrorURL+ObjectName`. ObjectName indicates the name of the requested object. For example, the origin URL configured for a bucket is `https://aliyun.com`, and the requested object named example.jpg does not exist in the bucket. OSS obtains the object by using `https://aliyun.com/example.jpg` and then stores the obtained object as example.jpg.

    -   Rules for failed back-to-origin requests

        If the object that you request is not found in the origin, the origin returns HTTP status code 404 to OSS. OSS then returns the same HTTP status code to you. If the origin returns a non-200 HTTP status code to OSS to indicate an error such as object retrieval failures due to network errors, OSS returns HTTP status code `424 MirrorFailed` to you.

    -   x-oss-tag response header

        When OSS returns an object obtained from the origin, OSS adds the `x-oss-tag` header to the response and sets its value to `MIRROR`. The header is in the following format: `x-oss-tag:MIRROR`

        After OSS obtains an object from the origin, this header is added to the response when the object is downloaded. This header indicates that the object is obtained by using mirroring-based back-to-origin.

    -   Update rules of objects obtained by using mirroring-based back-to-origin

        After OSS obtains an object by using mirroring-based back-to-origin and the object is modified in the origin, OSS does not update the obtained object.

    -   Object metadata obtained by using mirroring-based back-to-origin

        OSS stores the following HTTP headers returned from the origin as the object metadata:

        ```
        Content-Type
        Content-Encoding
        Content-Disposition
        Cache-Control
        Expires
        Content-Language
        Access-Control-Allow-Origin
        ```

    -   HTTP request rules
        -   Headers contained in the request sent to OSS are not contained in the request sent by OSS to the origin. Whether OSS sends the QueryString information to the origin depends on the back-to-origin rules configured in the OSS console.
        -   If the origin returns chunked-encoded data, OSS also returns chunked-encoded data to the user.

## Redirection-based back-to-origin

Redirection-based back-to-origin enables OSS to return a redirect response and status code 3xx based on the specified back-to-origin conditions and corresponding redirection configurations. The following figure shows the detailed process.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6182364951/p1591.png)

Scenarios:

-   Seamless data migration from other data sources to OSS

    When you use redirection-based back-to-origin to asynchronously migrate data from your data source to OSS, OSS returns a 302 redirect request by using URL rewrite for the data that is not migrated to OSS. Your client then reads the data from your data source based on the Location value in the 302 redirect request.

-   Page redirection

    For example, you can use redirection-based back-to-origin to hide objects whose names contain a specified prefix and return a specific page to visitors.

-   Page redirection for 404 or 500 errors

    You can use redirection-based back-to-origin to redirect users to a preset error page when a 404 or 500 error occurs. This method can prevent OSS from exposing system errors to users.


