# Configure object tagging

OSS allows you to configure object tagging to classify objects. You can configure object lifecycle rules and ACLs based on tags.

## Background information

When you configure object tagging, note the following items:

-   You can add tags to an object when you upload it or add tags to an uploaded object. If you add tags to an object that already has tags, the original tags are overwritten. For more information about how to configure object tagging, see [t159899.md\#](/intl.en-US/API Reference/Object operations/Tagging/PutObjectTagging.md).
-   To add tags to an object, you must have the permissions to call PutObjectTagging.
-   The Last-Modified value of an object is not updated when its tags are changed.
-   The key and value of the tag can contain letters, digits, spaces, and the following special characters:

    + ‑ = . \_ : /


Object tagging uses a key-value pair to identify objects. For more information about object tagging, see [Configure object tagging](/intl.en-US/Developer Guide/Objects/Manage files/Configure object tagging.md).

## Add tags to an object when you upload it

The following examples describe how to add tags to objects in simple upload, multipart upload, append upload, and resumable upload.

-   Add tags to an object when you upload it in simple upload.

    The following code provides an example on how to add tags to an object when you upload it by calling PutObject:

    ```
    using System.Text;
    using Aliyun.OSS;
    using System.Text;
    using Aliyun.OSS.Util;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    var objectContent = "More than just cloud." ;
    
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
        MemoryStream requestContent = new MemoryStream(binaryData);
    
        var meta = new ObjectMetadata();
        // Configure the tags in the HTTP header.
        string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
        meta.AddHeader("x-oss-tagging", str);
        var putRequest = new PutObjectRequest(bucketName, objectName, requestContent);
        putRequest.Metadata = meta;
    
        // Upload the object and add tags to it.
        client.PutObject(putRequest);
        Console.WriteLine("Put object succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("Put object failed, {0}", ex.Message);
    }
    ```

-   Add tags to an object when you upload it by using multipart upload.

    The following code provides an example on how to add tags to an object when you upload it by calling MultipartUpload:

    ```
    using Aliyun.OSS;
    using System.Text;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    var localFilename = "<yourLocalFilename>";
    
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    // Initialize a multipart upload task.
    var uploadId = "";
    try
    {
        // Set the name of the object and the bucket to which the object is uploaded. You can configure object metadata in InitiateMultipartUploadRequest, but you do not need to specify ContentLength.
        var request = new InitiateMultipartUploadRequest(bucketName, objectName);
    
        var meta = new ObjectMetadata();
        // Configure the tags in the HTTP header.
        string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
        meta.AddHeader("x-oss-tagging", str);
        request.Metadata = meta;
    
        var result = client.InitiateMultipartUpload(request);
        uploadId = result.UploadId;
        // Obtain the UploadId.
        Console.WriteLine("Init multi part upload succeeded");
        Console.WriteLine("Upload Id:{0}", result.UploadId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Init multi part upload failed, {0}", ex.Message);
    }
    // Calculate the total number of parts.
    var partSize = 100 * 1024;
    var fi = new FileInfo(localFilename);
    var fileSize = fi.Length;
    var partCount = fileSize / partSize;
    if (fileSize % partSize ! = 0)
    {
        partCount++;
    }
    // Start the multipart upload task. partETags is a list of partETag. OSS verifies the validity of each part after it receives the list of parts. After all the data parts are verified, OSS combines these parts into a complete object.
    var partETags = new List<PartETag>();
    try
    {
        using (var fs = File.Open(localFilename, FileMode.Open))
        {
            for (var i = 0; i < partCount; i++)
            {
                var skipBytes = (long)partSize * i;
                // Find the start position for this upload.
                fs.Seek(skipBytes, 0);
                // Calculate the part size in this upload. The size of the last part is the size of the remainder after the object is split by the calculated part size.
                var size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
                var request = new UploadPartRequest(bucketName, objectName, uploadId)
                {
                    InputStream = fs,
                    PartSize = size,
                    PartNumber = i + 1
                };
                // Call UploadPart to upload parts. The returned results contain the ETag values of parts.
                var result = client.UploadPart(request);
                partETags.Add(result.PartETag);
                Console.WriteLine("finish {0}/{1}", partETags.Count, partCount);
            }
            Console.WriteLine("Put multi part upload succeeded");
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Put multi part upload failed, {0}", ex.Message);
    }
    // Complete the multipart upload task.
    try
    {
        var completeMultipartUploadRequest = new CompleteMultipartUploadRequest(bucketName, objectName, uploadId);
        foreach (var partETag in partETags)
        {
            completeMultipartUploadRequest.PartETags.Add(partETag);
        }
        var result = client.CompleteMultipartUpload(completeMultipartUploadRequest);
        Console.WriteLine("complete multi part succeeded");
    }
    catch (Exception ex)
    {
        Console.WriteLine("complete multi part failed, {0}", ex.Message);
    }
    ```

-   Add tags to an object when you upload it by using append upload.

    The following code provides an example on how to add tags to an object when you upload it by calling AppendObject:

    ```
    using Aliyun.OSS;
    using System.Text;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    var localFilename = "<yourLocalFilename>";
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    // If the object is appended for the first time, the append position is 0. The returned value is the position for the next append. The position to start the next append is the total length of appended bytes and the append object.
    long position = 0;
    try
    {
        var metadata = client.GetObjectMetadata(bucketName, objectName);
        position = metadata.ContentLength;
    }
    catch (Exception) { }
    try
    {
        using (var fs = File.Open(localFilename, FileMode.Open))
        {
            var request = new AppendObjectRequest(bucketName, objectName)
            {
                ObjectMetadata = new ObjectMetadata(),
                Content = fs,
                Position = position
            };
    
            var meta = new ObjectMetadata();
            // Configure the tags in the HTTP header.
             string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
            meta.AddHeader("x-oss-tagging", str);
            request.Metadata = meta;
    
            // Start the first append. Only the tags configured when the object is appended for the first time are added to the object.
            var result = client.AppendObject(request);
            // Configure the position where the object is appended.
            position = result.NextAppendPosition;
            Console.WriteLine("Append object succeeded, next append position:{0}", position);
        }
        // Obtain the append position and append the object again.
        using (var fs = File.Open(localFilename, FileMode.Open))
        {
            var request = new AppendObjectRequest(bucketName, objectName)
            {
                ObjectMetadata = new ObjectMetadata(),
                Content = fs,
                Position = position
            };
            var result = client.AppendObject(request);
            position = result.NextAppendPosition;
            Console.WriteLine("Append object succeeded, next append position:{0}", position);
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine("Append object failed, {0}", ex.Message);
    }
    ```

-   Add tags to an object when you upload it by using resumable upload.

    The following code provides an example on how to add tags to an object when you upload it by using resumable upload:

    ```
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    using System.Text;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var bucketName = "<yourBucketName>";
    var objectName = "<yourObjectName>";
    var localFilename = "<yourLocalFilename>";
    string checkpointDir = "<yourCheckpointDir>";
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        // Configure parameters with UploadFileRequest.
        UploadObjectRequest request = new UploadObjectRequest(bucketName, objectName, localFilename)
        {
            // Specify the size of each part to upload.
            PartSize = 8 * 1024 * 1024,
            // Specify the number of concurrent threads.
            ParallelThreadCount = 3,
            // checkpointDir is used to record the result of multipart upload. This file stores information about upload progress. If you fail to upload a part, the part upload continues based on the recorded progress. If you set the checkpointDir to null, resumable upload does not take effect and objects are uploaded again when they fail to be uploaded.
            CheckpointDir = checkpointDir,
        };
    
        var meta = new ObjectMetadata();
        // Configure the tags in the HTTP header.
        string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
        meta.AddHeader("x-oss-tagging", str);
        request.Metadata = meta;
    
        // Start resumable upload.
        client.ResumableUploadObject(request);
        Console.WriteLine("Resumable upload object:{0} succeeded", objectName);
    }
    catch (OssException ex)
    {
        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID:{2}\tHostID:{3}",
            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failed with error info: {0}", ex.Message);
    }
    ```


## Add tags to an uploaded object or modify tags added to an object

The following code provides an example on how to add tags to an uploaded object or modify the tags added to an object:

```
using Aliyun.OSS;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var objectName = "<yourObjectName>";
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Configure tags.
    var setRequest = new SetObjectTaggingRequest(bucketName, objectName);

    var tag1 = new Tag
    {
        Key = "project",
        Value = "projectone"
    };

    var tag2 = new Tag
    {
        Key = "user",
        Value = "jsmith"
    };

    setRequest.AddTag(tag1);
    setRequest.AddTag(tag2);
    client.SetObjectTagging(setRequest);
    Console.WriteLine("set object tagging succeeded");
}
catch (Exception ex)
{
    Console.WriteLine("set object tagging failed. {0}", ex.Message);
}
```

## Add tags to an object when you copy it

You can specify one of the following tagging rules when you upload an object: Valid values:

-   Copy: The tag of the source object is copied to the destination object. This is the default value.
-   Replace: The tag of the destination object is set to the tag specified in the request instead of the tag of the source object.

The following examples describe how to add tags to objects smaller than 1 GB in simple copy mode and larger than 1 GB in multipart copy mode.

-   The following code provides an example on how to add tags to an object smaller than 1 GB when you copy it by calling CopyObject:

    ```
    using Aliyun.OSS;
    using Aliyun.OSS.Common;
    using System.Text;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var sourceBucket = "<yourSourceBucketName>";
    var sourceObject = "<yourSourceObjectName>";
    var targetBucket = "<yourDestBucketName>";
    var targetObject = "<yourDestObjectName>";
    
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        var metadata = new ObjectMetadata();
        metadata.AddHeader("mk1", "mv1");
        metadata.AddHeader("mk2", "mv2");
    
        // Configure the tags in the HTTP header.
        string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
        metadata.AddHeader("x-oss-tagging", str);
    
        var req = new CopyObjectRequest(sourceBucket, sourceObject, targetBucket, targetObject)
        {
            // If NewObjectMetadata is null, the Copy mode is used to copy the metadata of the source object. If the NewObjectMetadata is not null, the Replace mode is used to overwrite the metadata of the source object.
            NewObjectMetadata = metadata 
        };
        // Copy the object.
        client.CopyObject(req);
        Console.WriteLine("Copy object succeeded");
    }
    catch (OssException ex)
    {
        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID: {2} \tHostID: {3}",
            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failed with error info: {0}", ex.Message);
    }
    ```

-   The following code provides an example on how to add tags to an object larger than 1 GB when you copy it by calling MultipartUpload:

    ```
    using Aliyun.OSS;
    using System.Text;
    var endpoint = "<yourEndpoint>";
    var accessKeyId = "<yourAccessKeyId>";
    var accessKeySecret = "<yourAccessKeySecret>";
    var sourceBucket = "<yourSourceBucketName>";
    var sourceObject = "<yourSourceObjectName>";
    var targetBucket = "<yourDestBucketName>";
    var targetObject = "<yourDestObjectName>";
    var uploadId = "";
    var partSize = 50 * 1024 * 1024;
    String UrlEncodeKey(String key)
    {
    const string CharsetName = "utf-8";
    const char separator = '/';
    var segments = key.Split(separator);
    
    var encodedKey = new StringBuilder();
    encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
    for (var i = 1; i < segments.Length; i++)
        encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));
    
        if (key.EndsWith(separator.ToString()))
        {
            // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
            // so we have to append all the trailing slash to the uri.
            foreach (var ch in key)
            {
                if (ch == separator)
                    encodedKey.Append(separator);
                else
                    break;
            }
        }
    
    return encodedKey.ToString();
    }
    // Create an OSSClient instance.
    var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
    try
    {
        // Initialize a multipart copy task. You can call the InitiateMultipartUploadRequest operation to specify the metadata of the target object.
        var request = new InitiateMultipartUploadRequest(targetBucket, targetObject);
        var meta = new ObjectMetadata();
        // Configure the tags in the HTTP header.
        string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
        meta.AddHeader("x-oss-tagging", str);
        request.Metadata = meta;
    
        var result = client.InitiateMultipartUpload(request);
        // Obtain the UploadId.
        uploadId = result.UploadId;
        Console.WriteLine("Init multipart upload succeeded, Upload Id: {0}", result.UploadId);
        // Calculate the total number of parts.
        var metadata = client.GetObjectMetadata(sourceBucket, sourceObject);
        var fileSize = metadata.ContentLength;
        var partCount = (int)fileSize / partSize;
        if (fileSize % partSize ! = 0)
        {
            partCount++;
        }
        // Start the multipart copy task.
        var partETags = new List<PartETag>();
        for (var i = 0; i < partCount; i++)
        {
            var skipBytes = (long)partSize * i;
            var size = (partSize < fileSize - skipBytes) ? partSize : (fileSize - skipBytes);
            // Create an UploadPartCopyRequest. You can specify conditions by setting UploadPartCopyRequest.
            var uploadPartCopyRequest = new UploadPartCopyRequest(targetBucket, targetObject, sourceBucket, sourceObject, uploadId)
                {
                    PartSize = size,
                    PartNumber = i + 1,
                    // Use BeginIndex to find the start position to copy the part.
                    BeginIndex = skipBytes
                };
            // Call the uploadPartCopy operation to copy each part.
            var uploadPartCopyResult = client.UploadPartCopy(uploadPartCopyRequest);
            Console.WriteLine("UploadPartCopy : {0}", i);
            partETags.Add(uploadPartCopyResult.PartETag);
        }
        // Complete the multipart copy task.
        var completeMultipartUploadRequest =
        new CompleteMultipartUploadRequest(targetBucket, targetObject, uploadId);
        // partETags is a list of partETag. OSS verifies the validity of each part after it receives the list of parts. After all parts are verified, OSS combines these parts into a complete object.
        foreach (var partETag in partETags)
        {
            completeMultipartUploadRequest.PartETags.Add(partETag);
        }
        var completeMultipartUploadResult = client.CompleteMultipartUpload(completeMultipartUploadRequest);
        Console.WriteLine("CompleteMultipartUpload succeeded");
    }
    catch (OssException ex)
    {
        Console.WriteLine("Failed with error code: {0}; Error info: {1}. \nRequestID: {2} \tHostID: {3}",
            ex.ErrorCode, ex.Message, ex.RequestId, ex.HostId);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Failed with error info: {0}", ex.Message);
    }
    ```


## Add tags to a symbolic link object

The following code provides an example on how to add tags to a symbolic link object:

```
using Aliyun.OSS;
using System.Text;
var endpoint = "<yourEndpoint>";
var accessKeyId = "<yourAccessKeyId>";
var accessKeySecret = "<yourAccessKeySecret>";
var bucketName = "<yourBucketName>";
var targetObjectName = "<yourTargetObjectName>";
var symlinkObjectName = "<yourSymlinkObjectName>";
var objectContent = "More than just cloud." ;
String UrlEncodeKey(String key)
{
const string CharsetName = "utf-8";
const char separator = '/';
var segments = key.Split(separator);

var encodedKey = new StringBuilder();
encodedKey.Append(HttpUtils.EncodeUri(segments[0], CharsetName));
for (var i = 1; i < segments.Length; i++)
    encodedKey.Append(separator).Append(HttpUtils.EncodeUri(segments[i], CharsetName));

    if (key.EndsWith(separator.ToString()))
    {
        // String#split ignores trailing empty strings, e.g., "a/b/" will be split as a 2-entries array,
        // so we have to append all the trailing slash to the uri.
        foreach (var ch in key)
        {
            if (ch == separator)
                encodedKey.Append(separator);
            else
                break;
        }
    }

return encodedKey.ToString();
}
// Create an OSSClient instance.
var client = new OssClient(endpoint, accessKeyId, accessKeySecret);
try
{
    // Upload the object to which the symbolic link points.
    byte[] binaryData = Encoding.ASCII.GetBytes(objectContent);
    MemoryStream requestContent = new MemoryStream(binaryData);
    client.PutObject(bucketName, targetObjectName, requestContent);

    var meta = new ObjectMetadata();
    // Configure the tags in the HTTP header.
    string str = UrlEncodeKey("key1") + "=" + UrlEncodeKey("key1") + "&" + UrlEncodeKey("key2") + "=" + UrlEncodeKey("key2");
    meta.AddHeader("x-oss-tagging", str);
    var request = new CreateSymlinkRequest(bucketName, symlinkObjectName, targetObjectName);
    request.ObjectMetadata = meta;
    // Create a symbolic link object.
    client.CreateSymlink(request);
    // Obtain the name of the object to which the symbolic link points.
    var ossSymlink = client.GetSymlink(bucketName, symlinkObjectName);
    Console.WriteLine("Target object is {0}", ossSymlink.Target);
}
catch (Exception ex)
{
    Console.WriteLine("Failed with error info: {0}", ex.Message);
}
```

