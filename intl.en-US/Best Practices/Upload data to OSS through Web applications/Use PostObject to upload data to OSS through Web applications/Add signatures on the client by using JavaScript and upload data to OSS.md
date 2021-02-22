# Add signatures on the client by using JavaScript and upload data to OSS

This topic describes how to use JavaScript to add signatures on the client based on the POST policy and then upload data to OSS by using form upload.

## Usage notes

-   After you use JavaScript to add signatures on the client, you can directly upload data to OSS. However, your AccessKey ID and AccessKey secret may be exposed because they are included in the JavaScript code. We recommend that you add signatures on the server to upload data. For more information, see [Add signatures on the server for object upload](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server for object upload.md).
-   The application server code provided in this topic supports protocols such as HTML4, HTML5, Flash, and Silverlight. Ensure that your browser supports these protocols. If a prompt such as "Your browser does not support Flash, Silverlight, or HTML5!" appears, upgrade your browser.

## Step 1: Download the client code for browsers

Plupload is used in this example to upload form data \([PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md)\) to OSS. The sample code in JavaScript can be run on WeChat or browsers from computers or mobile phones. You can select multiple objects for uploads and specify the destination folder. In addition, you can specify whether the original names of the uploaded objects are retained or randomly specified by the system. You can view the upload progress by using the progress bar.

1.  Download the [browser client code](http://gosspublic.alicdn.com/doc/oss-h5-upload-js-direct.zip).

2.  Decompress the downloaded file.


**Note:** The frontend plug-in used in this example is Plupload. You can change the plug-in.

## Step 2: Modify the configuration file

Open the upload.js file and modify access configurations.

```
accessid= '<yourAccessKeyId>';
accesskey= <yourAccessKeySecret>';
host= <yourHost>';
// Security risks may arise if you use the AccessKey pair of an Alibaba Cloud account to log on to OSS because the account has permissions on all API operations. We recommend that you use your RAM user's credentials to call API operations or perform routine operations and maintenance. To create your RAM user, log on to [https://ram.console.aliyun.com](https://ram.console.aliyun.com/).
.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        ....
    };

// If you use STS access credentials to access OSS, modify the configurations.
accessid= 'accessid';
accesskey= 'accesskey';
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
STS_ROLE = ''; // eg: acs:ram::1069test7698:role/all

.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': STS.accessID, 
        'x-oss-security-token':STSToken,
        ....
    };
//===========
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
```

-   accessid: your AccessKey ID
-   accesskey: your AccessKey secret
-   STS\_ROLE: the Alibaba Cloud Resource Name \(ARN\) of the specified role. For more information about RoleArn, see [AssumeRole](/intl.en-US/API Reference/API Reference (STS)/Operation interfaces/AssumeRole.md).
-   STSToken: your STS token. If you use STS for authentication, you must call the STS API operation to obtain the STS AccessKey ID, STS AcessKey secret, and security token. For more information, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md). If you use the AccessKey pair of an Alibaba Cloud account or a permanent RAM user, you can leave this parameter unspecified.
-   host: your OSS endpoint used to access the bucket. The format is `BucketName.Endpoint`. Example: `post-test.oss-cn-hangzhou.aliyuncs.com`. For more information about OSS endpoints, see [OSS domain names](/intl.en-US/Developer Guide/Endpoint/OSS domain names.md).

## Step 3: Configure CORS

When you use form upload to upload data from the client to OSS, the client includes the `Origin` header in the request and sends the request from the browser to OSS. OSS verifies whether a request that includes the `Origin` header is a cross-origin request. Therefore, you must configure cross-origin resource sharing \(CORS\) rules for the target bucket to allow POST-based requests.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Access Control** \> **Cross-Origin Resource Sharing \(CORS\)**. In the **Cross-Origin Resource Sharing \(CORS\)** section, click **Configure**.

4.  Click **Create Rule**. The following figure shows the configurations of a rule.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9354449951/p12308.png)

    **Note:** For data security reasons, enter a required domain name in the **Sources** field. For more information about CORS configurations, see [Configure CORS rules](/intl.en-US/Console User Guide/Manage buckets/Access control/Configure CORS rules.md).


## Step 4: Use JavaScript to add signatures on the client and upload data to OSS

1.  Open the index.html file in the decompressed folder of the client code.

2.  Click **Select File**. Select one or more files to upload. Then, select the naming convention for the uploaded objects, and specify the folder that contains the uploaded objects.

3.  Click **Upload**. Wait until the uploads are complete.

4.  After the uploads are complete, you can log on to the OSS console to view the upload results.


## Core code analysis

OSS supports POST requests. Therefore, you need to add a signature to a POST request only when you use Plupload to send the request. The following code provides an example on how to add signatures to a POST request:

```
function set_upload_param(up, filename, ret)
{
  g_object_name = g_dirname;
  if (filename ! = '') {
      suffix = get_suffix(filename)
      calculate_object_name(filename)
  }
  new_multipart_params = {
      'key' : g_object_name,
      'policy': policyBase64,
      'OSSAccessKeyId': accessid,
      'success_action_status' : '200', // If you do not set success_action_status to 200, 204 is returned when the object is uploaded.
      'signature': signature,
  };

  up.setOption({
      'url': host,
      'multipart_params': new_multipart_params
  });

  up.start();
}
　....
```

In the preceding code, `'key': g_object_name` indicates the path of the uploaded object. If you want to retain the original name for the uploaded object, set this field to `'key': '${filename}'`.

To upload objects to a specified folder such as the abc without changing the object names, modify the code:

```
new_multipart_params = {
  'key' : 'abc/' + '${filename}',
  'policy': policyBase64,
  'OSSAccessKeyId': accessid,
  'success_action_status' : '200', // Set success_action_status to 200 so that 200 is returned if the request is successful. If this parameter is not specified, 204 is returned.
  'signature': signature,
};
```

-   Specify that the names of uploaded objects are randomly specified by OSS

    To instruct OSS to randomly specify the name of an uploaded object without changing its extension, modify the function:

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   Specify that object names remain unchanged

    To retain the original names of uploaded objects, modify the function:

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   Configure a destination folder

    You can upload objects to a specified folder. The following code provides an example on how to set the destination folder to abc/. The destination folder must end with a forward slash \(/\).

    ```
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   Sign the policyText value

    When you upload objects, you must sign the policyText value. Example:

    ```
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", // Configure the expiration time of the policy. If the validity period is exceeded, this policy cannot be used to upload objects.
        "conditions": [
        ["content-length-range", 0, 1048576000] // Set the size limit for objects to upload. If the size of the object to upload exceeds the limit, an error is returned.
        ]
    }
    ```


## FAQ

-   How do I specify the format of an object to upload?

    You can use Plupload filters to set upload conditions, such as uploading only images, the size of objects to upload, or disabling the upload of duplicate objects. For more information about complete code, see the "Set upload conditions" section in [Add signatures on the server, configure upload callback, and directly transfer data](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server, configure upload callback, and directly transfer data.md).

-   How do I obtain the URL of an uploaded object?

    You can obtain the URL of an uploaded object based on the endpoint of the bucket and the object path. For more information, see [How do I obtain the URL of an uploaded object?](/intl.en-US/Developer Guide/Objects/FAQ/How do I obtain the URL of an uploaded object?.md).

-   How do I obtain the MD5 hash of an uploaded object?

    Open the developer tools of your browser, and then upload an object. After the object is uploaded, you can view the MD5 hash in the response header.


If you have more questions, [submit a ticket](https://workorder-intl.console.aliyun.com/?spm=5176.2020520001.nav-right.ditem-sub.433c12d26HHFbc#/ticket/createIndex).

