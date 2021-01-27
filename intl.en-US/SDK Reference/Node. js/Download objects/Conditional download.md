# Conditional download

You can specify conditions when you download objects. If the specified conditions are met, the objects can be downloaded. If the specified conditions are not met, an error is returned and the objects cannot be downloaded.

## Download objects that are modified earlier than the specified time

The following code provides an example on how to download objects that are modified earlier than the specified time:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
 // Upload an object named exampleobject.txt.
  await client.put('exampleobject.txt', Buffer.from('contenttest'))
 // Set the If-Modified-Since header in the request. If the value of this header is earlier than the time when the uploaded object is last modified, the object is downloaded.
let result = await client.get("exampleobject.txt", {
  headers: {
    "If-Modified-Since": new Date("1970-01-01").toGMTString(),
  },
});
console.log(result.content.toString() === "contenttest");
console.log(result.res.status === 200);

// If the value of the If-Modified-Since header is equal to or later than the time when the uploaded object is last modified, OSS returns 304 Not Modified.
result = await client.get("exampleobject.txt", {
  headers: {
    "If-Modified-Since": new Date().toGMTString(),
  },
});
console.log(result.content.toString() === "");
console.log(result.res.status === 304);  
console.log(e.code === "Not Modified");
}

main();
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

## Download objects that are modified no earlier than the specified time

The following code provides an example on how to download objects that are modified no earlier than the specified time:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
 // Upload an object named exampleobject.txt.
  await client.put('exampleobject.txt', Buffer.from('contenttest'))
// Set the If-Unmodified-Since header in the request. If the value of this header is equal to or later than the time when the uploaded object is last modified, the object is downloaded.
let result = await client.get("exampleobject.txt", {
  headers: {
    "If-Unmodified-Since": new Date().toGMTString(),
  },
});
console.log(result.content.toString() === "contenttest");
console.log(result.res.status === 200);
// If the value of the If-Unmodified-Since header is earlier than the time when the uploaded object is last modified, OSS returns 412 Precondition Failed.
try {
  await client.get("exampleobject.txt", {
    headers: {
      "If-Unmodified-Since": new Date("1970-01-01").toGMTString(),
    },
  });
} catch (e) {
  console.log(e.status === 412);
  console.log(e.code === "PreconditionFailed");
}

main();
```

## Download objects whose ETags match the specified ETag values

The ETag of an object can be used to check whether the object content changes. The following code provides an example on how to download objects whose ETags match the specified ETag values:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
  // Upload an object named exampleobject.txt and obtain the ETag of the object.
  let {res: {headers: {etag}}}  = await client.put('exampleobject.txt', Buffer.from('contenttest'))
}

// Set the If-Match header to an ETag value. If the specified ETag value matches the ETag of the object, the object is downloaded.
let result = await client.get("exampleobject.txt", {
  headers: {
    "If-Match": etag,
  },
});
console.log(result.content.toString() === "contenttest");
console.log(result.res.status === 200);

// If the specified ETag value does not match the ETag of the object, OSS returns 412 Precondition Failed.
try {
  await client.get("exampleobject.txt", {
    headers: {
      "If-Match": etag,
    },
  });
} catch (e) {
  console.log(e.status === 412);
  console.log(e.code === "PreconditionFailed");
}

main();
```

## Download objects whose ETags do not match the specified ETag values

The following code provides an example on how to download objects whose ETags do not match the specified ETag values:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
  // Upload an object named exampleobject.txt and obtain the ETag of the object.
  let {res: {headers: {etag}}}  = await client.put('exampleobject.txt', Buffer.from('contenttest'))
}

// Set the If-Match header to an ETag value. If the specified ETag value does not match the ETag of the object, the object is downloaded.
let result = await client.get("exampleobject.txt", {
  headers: {
    "If-None-Match": etag,
  },
});
console.log(result.content.toString() === "contenttest");
console.log(result.res.status === 200);

// If the specified ETag value matches the ETag of the object, OSS returns 304 Not Modified.
result = await client.get("exampleobject.txt", {
  headers: {
    "If-None-Match": etag,
  },
});
console.log(result.content.toString() === "");
console.log(result.res.status === 304);
console.log(e.code === "Not Modified");
}

main();
```

