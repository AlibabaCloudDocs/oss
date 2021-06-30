# How do I obtain the URL of an uploaded object?

This topic describes how to obtain the URL of an uploaded object.

## Public read object

If the access control list \(ACL\) of an object is public read, the object can be accessed by anonymous users. The URL of the object is in the following format: `https://BucketName.Endpoint/ObjectName`. In the preceding URL, ObjectName is the full path of the object that includes the object prefix and suffix. For more information about the endpoints of each region, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

For example, an object named example.jpg is stored within the example directory of a bucket named bucketexample, which is located in the China \(Hangzhou\) region. The following URLs can be used to access the object:

-   URL for access over the Internet: `https://bucketexample.oss-cn-hangzhou.aliyuncs.com/example/example.jpg`.
-   URL for access over the internal network \(from Elastic Compute Service \(ECS\) instances that are located in the same region as the object\): `https://bucketexample.oss-cn-hangzhou-internal.aliyuncs.com/example/example.jpg`

**Note:** To make sure that an image object is previewed when you access the image object, you must map a custom domain name to your bucket and add a Canonical Name \(CNAME\) record. For more information, see [Use a custom domain name to access OSS resources](/intl.en-US/Quick Start/OSS console/Use a custom domain name to access OSS resources.md).

## Private object

If the ACL of an object is private, the URL of the object must be signed. The URL of a private object is in the following format: `https://BucketName.Endpoint/Object?SignatureParameters`. You can use the following methods to obtain the URL of a private object and set the validity period of the URL:

-   Console

    You can obtain the URL of a private object in the Object Storage Service \(OSS\) console. For more information, see [Share objects](/intl.en-US/Quick Start/OSS console/Share objects.md). The validity period of an object URL can be set to different value ranges by different accounts. Alibaba Cloud accounts can set the validity period of an object URL to a value up to 32,400 seconds \(9 hours\). Resource Access Management \(RAM\) users and temporary users authorized by Security Token Service \(STS\) can set the validity period of an object URL to a value up to 3,600 seconds \(1 hour\). To set the validity period of an object URL to a greater value, you can use ossutil, ossbrowser, or OSS SDKs.

-   ossutil

    For more information about how to use ossutil to obtain the URL of a private object and set the validity period of the URL, see [ossutil-sign](/intl.en-US/Tools/ossutil/Common commands/sign.md).

-   ossbrowser

    For more information about how to use ossbrowser to obtain the URL of a private object and set the validity period of the URL, see [ossutil-sign](/intl.en-US/Tools/ossbrowser/Use ossbrowser.md).

-   SDKs

    For more information about how to use OSS SDKs to obtain the URL of a private object and set the validity period of the URL, see the following topics:

    -   [OSS SDK for Java](/intl.en-US/SDK Reference/Java/Authorized access.md)
    -   [OSS SDK for Python](/intl.en-US/SDK Reference/Python/Authorized access.md)
    -   [OSS SDK for Go](/intl.en-US/SDK Reference/Go/Authorized access.md)
    -   [OSS SDK for PHP](/intl.en-US/SDK Reference/PHP/Authorized access.md)
    -   [OSS SDK for C](/intl.en-US/SDK Reference/C/Authorized access.md)
    -   [OSS SDK for .NET](/intl.en-US/SDK Reference/. NET/Authorized access.md)
    -   [OSS SDK for Android](/intl.en-US/SDK Reference/Android/Authorized access.md)
    -   [OSS SDK for iOS](/intl.en-US/SDK Reference/iOS/Authorized access.md)
    -   [OSS SDK for Node.js](/intl.en-US/SDK Reference/Node. js/Authorized access.md)
    -   [OSS SDK for Browser.js](/intl.en-US/SDK Reference/Browser.js/Authorized access.md)

## Objects in a bucket to which a custom domain name is mapped

If the bucket in which an object is stored is mapped to a custom domain name, the URL of the object is in the following format: `https://YourDomainName/ObjectName`. In the preceding URL, ObjectName is the full path of the object that includes the object prefix and suffix.

For example, a bucket named bucketexample is located in the China \(Hangzhou\) region, and the following custom domain name is mapped to the bucket: `img.example.com`. In this bucket, an object named example.jpg is stored in a directory named example. You can use the following URL to access the object: `https://img.example.com/example/example.jpg`.

