# Range download

If you need only part of the data in an object, you can use range download to download data within a specified range.

## Specify a valid range to download data

If the start and end values of the range that you specify are within the size of the object, the content within the specified range is downloaded. For example, if the object from which you want to download data is 1,000 bytes in size, the valid range is from byte 0 to byte 999.

The following code provides an example on how to specify a valid range to download data:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
  let start = 1, end = 900;
  // yourObjectName indicates the complete path of the object excluding the bucket name. Example: destfolder/examplefile.txt.
  // Obtain the data that is within the range from byte 0 to byte 900, which includes a total of 900 bytes.
  // If the start or end value of the specified range is not within the valid range, the entire object is downloaded.
  let result = await client.get("<yourObjectName>", {
    headers: {
      Range: `bytes=${start}-${end}`,
    },
  })
  console.log(result.content.toString())
};

main();
```

For more information about the naming conventions for buckets, see [bucket](/intl.en-US/Developer Guide/Terms.md). For more information about the naming conventions for objects, see [object](/intl.en-US/Developer Guide/Terms.md).

## Specify compatible behaviors to download data by range

You can add the `x-oss-range-behavior:standard` header in the request to modify the download behavior when the specified range is not within the valid range.

The following code provides an example on how to specify compatible behaviors to download data by range:

```
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create a RAM user, log on to the RAM console.
const accessKeyId = "yourAccessKeyId";
const accessKeySecret = "yourAccessKeySecret";
// Specify the endpoint of the region in which the bucket is located. For example, if the bucket is located in the China (Hangzhou) region, set the endpoint to https://oss-cn-hangzhou.aliyuncs.com.
const endpoint = "yourEndpoint";
// Specify the bucket name.
const bucket = "yourBucketName";

async function main() {
  // Upload an object named exampleobject.txt, which is 10 bytes in size.
  let buf = Buffer.from("abcdefghij");
  await client.put("exampleobject.txt", buf);
  let result = await client.get("exampleobject.txt", {
  // Set Range to bytes=5-15. In this case, the end value of the range is not within the valid range. Therefore, OSS returns the HTTP status code 206 and the content from byte 6 to byte 10.
    headers: {
      Range: "bytes=5-15",      
      "x-oss-range-behavior": "standard",
    },
  });
  console.log(result.content.toString() === 'fghij')
  console.log(result.res.status === 206)
  try{
    await client.get("exampleobject.txt", {
      // Set Range to bytes=15-25. In this case, the start value of the range is not within the valid range. Therefore, OSS returns the HTTP status code 416 and the error code InvalidRange.
      headers: {
        Range: "bytes=15-25",        
        "x-oss-range-behavior": "standard",
      },
    })
  }catch(e) {
    console.log(e.status === 416);
    console.log(e.name === 'InvalidRangeError')
  }
}

main();
```

