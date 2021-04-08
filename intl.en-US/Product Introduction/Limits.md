# Limits

This topic describes the performance metrics and limits of OSS.

The following table describes the performance metrics and limits of OSS.

|Item|Limit|
|:---|:----|
|Bandwidth|Default bandwidth limit: 10 Gbit/s in mainland China regions and 5 Gbit/s in regions outside mainland China. If this limit is reached, requests are throttled. **Note:** When requests are throttled, the response header contains `x-oss-qos-delay-time: number`, in which `number` indicates the time period during which requests are throttled. Unit: ms. For an upload request, the exact time period during which requests are throttled is returned. For a download request, the estimated time period during which requests are throttled is returned. The time is estimated based on the throttling status and the object size.

If you require a greater bandwidth \(10 Gbit/s to 100 Gbit/s\), for example, if you have offline big data processing requirements, contact the [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|Queries per second \(QPS\)|The limit of the total QPS for a single account is 10,000. The actual values that can be achieved are different in the different read/write modes:-   Sequential read/write: 2,000

If you use sequential prefixes such as timestamps or letters in the names of large numbers of objects, multiple object indexes may be stored in a single partition and responsiveness may slow significantly. We recommend that you do not upload large number of objects by using sequential prefixes. For more information about how to change sequential prefixes to random prefixes, see [OSS performance and scalability best practices](/intl.en-US/Best Practices/OSS performance and scalability best practices.md).

-   Non-sequential read/write: 10,000

If you require a greater QPS, contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |
|Bucket|-   You can use an Alibaba Cloud account to create up to 100 buckets in the same region.
-   After a bucket is created, the name, region, and storage class of the bucket cannot be modified.
-   OSS does not impose limits on the capacity of a bucket. |
|Object|-   An object uploaded to OSS by using [simple upload](/intl.en-US/Developer Guide/Objects/Upload files/Simple upload.md), [form upload](/intl.en-US/Developer Guide/Objects/Upload files/Form upload.md), or [append upload](/intl.en-US/Developer Guide/Objects/Upload files/Append upload.md) cannot exceed 5 GB in size. An object uploaded to OSS by using [multipart upload](/intl.en-US/Developer Guide/Objects/Upload files/Multipart upload and resumable upload.md) cannot exceed 48.8 TB in size.
-   When you change the storage class of objects by calling the CopyObject operation in the console, the object size cannot exceed 1 GB.
-   You can delete up to 100 objects at a time in the OSS console.
-   If you upload an object that has the same name as an existing object in the bucket, the existing object is overwritten.
-   Deleted objects cannot be recovered. |
|Data restoration|You must restore an Archive or Cold Archive object before you access the object. -   For an Archive object, it takes about one minute to restore the object.
-   For a Cold Archive object, the restoration time is determined based on the restoration priority of the object.
    -   Expedited: The object is restored within one hour.
    -   Standard: The object is restored within two to five hours.
    -   Batch: The object is restored within five to twelve hours. |
|[Custom domain name](/intl.en-US/Developer Guide/Buckets/Bind custom domain names.md)|-   For domain names bound to buckets located within mainland China regions, you must apply for an ICP filing at the Ministry of Industry and Information Technology \(MIIT\).
-   A single bucket can be bound to up to 100 domain names, but a domain name can be bound only to a single bucket.
-   OSS does not impose limits on the number of domain names that can be bound to the buckets of an Alibaba Cloud account. |
|[Lifecycle rules](/intl.en-US/Developer Guide/Objects/Object lifecycle/Lifecycle rules.md)|You can configure up to 1,000 lifecycle rules for each bucket.|
|[Back-to-origin rules](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md)|-   You can configure up to 20 back-to-origin rules for each bucket.
-   In regions within mainland China and the China \(Hong Kong\) region, the default QPS of mirroring-based back-to-origin is 2,000, and the default bandwidth is 2 Gbit/s. In regions outside China, the default QPS for mirroring-based back-to-origin is 1,000, and the default bandwidth is 1 Gbit/s. |
|[IMG](/intl.en-US/Developer Guide/Data Processing/Image Processing/Overview.md)|-   Limits on images
    -   Source images
        -   Only JPG, PNG, BMP, GIF, WebP, and TIFF images are supported.
        -   The size of the source image cannot exceed 20 MB.
        -   For the rotate operation, the height or width of the source image cannot exceed 4,096 pixels. For other operations, the width or height of the source image cannot exceed 30,000 pixels, and the total pixels of the source image cannot exceed 250 million.

The pixel number of a dynamic image, such as a GIF image, is calculated by using the following formula: `Width x Height x Number of image frames`. The pixel number of a static image, such as a PNG image, is calculated by using the following formula: `Width x Height`.

    -   Resized images

The width or height of the resized image cannot exceed 16,384 pixels. The total pixel number of the resized image cannot exceed 16,777,216.

-   Limits on image styles

You can create up to 50 image styles in each bucket. To create more styles for a bucket, contact [technical support](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). |

