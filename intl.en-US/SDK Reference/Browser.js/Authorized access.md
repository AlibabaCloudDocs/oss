# Authorized access

This topic describes how to authorize access to OSS.

## Use STS to authorize temporary access

You can use Alibaba Cloud Security Token Service \(STS\) to authorize temporary access to OSS. STS is a web service that provides temporary access tokens for cloud computing users. You can use STS to grant a third-party application or your RAM user an access credential with a customized validity period and permissions. For more information about STS, see [What is STS?](/intl.en-US/API Reference/API Reference (STS)/What is STS?.md)

STS has the following benefits:

-   You need only to generate an access token and send the access token to a third-party application, instead of exposing your long-term AccessKey pair to the third-party application. You can customize the access permissions and validity period of this token.
-   The access token automatically expires when the validity period ends.

For more information about how to set up a security token service \(STS\), see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md) in OSS Developer Guide.

The following code provides an example on how to use STS to create a signed request:

```
// Obtain a temporary access credential from the STS you set up.
fetch('http://your_sts_server/')
  .then(resp => resp.json())
  .then(result => {
    const store = new OSS({
      accessKeyId: result.AccessKeyId,
      accessKeySecret: result.AccessKeySecret,
      stsToken: result.SecurityToken,
      // Set region to the region where the requested bucket is located. For example, if the requested bucket is located in the China (Hangzhou) region, set region to oss-cn-hangzhou.
      region: 'oss-cn-hangzhou',
      bucket: 'examplebucket'
    });
    // Generate a signed URL.
    const url = store.signatureUrl('ossdemo.txt');
    console.log(url);
  })
```

## Use a signed URL to authorize temporary access

-   Generate a signed URL

    You can generate a signed URL and provide it to a visitor to grant temporary access. When you generate a signed URL, you can specify the validity period of the URL to limit the period of access from visitors.

-   Generate a signed URL for an object

    **Note:** name \{String\} specifies the name of the object stored in OSS. \[expires\] \{Number\} specifies the validity period of the URL. Unit: seconds. Default value: 1800. For more information about other parameters, visit [GitHub](https://github.com/ali-sdk/ali-oss#signatureurlname-options).

    The following code provides an example on how to generate a signed URL for an object:

    ```
    const url = store.signatureUrl('ossdemo.txt');
    console.log(url);
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      method: 'PUT'
    });
    console.log(url);
    
    //  put object with signatureUrl
    // -------------------------------------------------
    
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      method: 'PUT',
      'Content-Type': 'text/plain; charset=UTF-8',
    });
    console.log(url);
    
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.txt', {
      expires: 3600,
      response: {
        'content-type': 'text/custom',
        'content-disposition': 'attachment'
      }
    });
    console.log(url);
    
    // put operation
    ```

-   Generate a signed URL that contains IMG parameters

    ```
    const url = store.signatureUrl('ossdemo.png', {
      process: 'image/resize,w_200'
    });
    console.log(url);
    // --------------------------------------------------
    const url = store.signatureUrl('ossdemo.png', {
      expires: 3600,
      process: 'image/resize,w_200'
    });
    console.log(url);
    ```


