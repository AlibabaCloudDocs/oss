# Limits

This topic describes the performance metrics and limits of Object Storage Service \(OSS\).

The following table describes the performance metrics and limits of OSS.

|Item|Limit|
|:---|:----|
|Bandwidth|Default bandwidth limit: 10 Gbit/s in mainland China regions and 5 Gbit/s in regions outside mainland China. If this limit is reached, requests are throttled. **Note:** When a request is throttled, the response to the request contains the `x-oss-qos-delay-time: number` header, in which `number` indicates the time period during which requests are throttled. Unit: ms. For an upload request, the exact time period during which requests are throttled is returned. For a download request, the estimated time period during which requests are throttled is returned. The time is estimated based on the throttling status and the object size.

If you require a greater bandwidth \(10 Gbit/s to 100 Gbit/s\), for example, if you have offline big data processing requirements, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|Queries per second \(QPS\)|The limit of the total QPS for a single account is 10,000. The actual values that can be achieved are different in the different read and write modes:-   Sequential read/write: 2,000

If you use sequential prefixes such as timestamps or letters in the names of large numbers of objects, multiple object indexes may be stored in a single partition and responsiveness may slow significantly. We recommend that you do not upload large number of objects by using sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Non-sequential read/write: 10,000

If you require a greater QPS, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|Bucket|-   An Alibaba Cloud account to create up to 100 buckets in the same region.
-   After a bucket is created, the name, region, and storage class of the bucket cannot be modified.
-   OSS does not impose limits on the capacity of a bucket. |
|Object|-   The size of the object to upload

When you use [simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md), [form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md), or [append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md) to upload a single object, the object cannot exceed 5 GB in size.

When you use [multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) to upload a single object, the object cannot exceed 48.8 TB in size.

-   The size of the object to rename

You can rename only objects that are up to 1 GB in size in the OSS console. To rename objects that are larger than 1 GB, we recommend that you use [ossutil](/intl.en-US/Tools/ossutil/Common commands/cp/Copy objects.md).

-   The number of objects to delete

You can delete up to 100 objects at a time in the OSS console.

You can delete up to 1,000 objects at a time when you use OSS SDKs, ossbrowser, and ossutil.

Warning:

Deleted objects cannot be recovered. Exercise caution when you delete objects.

-   Whether objects are overwritten by those with the same names

By default, when you upload an object that has the same name as an existing object, the existing object is overwritten. To prevent objects from being accidentally overwritten, you can enable versioning for the bucket in which the objects are stored. You can also include the x-oss-forbid-overwrite header in requests and set its value to true. |
|Data restoration|You must restore an Archive or Cold Archive object before you access the object. -   It takes about one minute to restore an Archive object.
-   For a Cold Archive object, the restoration period is determined based on the restoration priority of the object.
    -   Expedited: The object is restored within 1 hour.
    -   Standard: The object is restored within 2 to 5 hours.
    -   Batch: The object is restored within 5 to 12 hours. |
|[Custom domain name](/intl.en-US/Developer Guide/Buckets/Map custom domain names.md)|-   For domain names that are mapped to buckets located within mainland China regions, you must apply for an ICP filing at the Ministry of Industry and Information Technology \(MIIT\).
-   Up to 100 domain names can be mapped to a bucket. However, a domain name can be mapped only to a single bucket.
-   OSS does not impose limits on the number of domain names that can be mapped to the buckets of an Alibaba Cloud account. |
|[Lifecycle rules](/intl.en-US/Developer Guide/Buckets/Lifecycle/Lifecycle rules.md)|You can configure up to 1,000 lifecycle rules for each bucket.|
|[Back-to-origin rules](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md)|-   You can configure up to 20 back-to-origin rules for each bucket.
-   In regions within mainland China and the China \(Hong Kong\) region, the default QPS for mirroring-based back-to-origin requests is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin requests is 1,000, and the default bandwidth is 1 Gbit/s. |
|[IMG](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md)|-   Limits on images
    -   Source images
        -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported.
        -   The size of the source image cannot exceed 20 MB.
        -   For the rotate operation, the height or width of the source image cannot exceed 4,096 pixels. For other operations, the width or height of the source image cannot exceed 30,000 pixels, and the total pixel number of the source image cannot exceed 250 million.

The total pixel number of a dynamic image, such as a GIF image, is calculated by using the following formula: `Width × Height × Number of image frames`. The total pixel number of a static image, such as a PNG image, is calculated by using the following formula: `Width × Height`.

    -   Resized images

The width or height of the resized image cannot exceed 16,384 pixels. The total pixel number of the resized image cannot exceed 16,777,216.

-   Limits on image styles

You can create up to 50 image styles in each bucket. To create more styles for a bucket, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |

