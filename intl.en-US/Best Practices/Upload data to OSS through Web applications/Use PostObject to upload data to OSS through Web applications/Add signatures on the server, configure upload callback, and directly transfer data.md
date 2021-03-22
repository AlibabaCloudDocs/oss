# Add signatures on the server, configure upload callback, and directly transfer data

This topic describes how to use Post policies to add signatures on the server in various programming languages, configure upload callback, and directly transfer data to OSS by using form upload.

## Background information

When you upload data by using the solution described in [Add signatures on the server for object upload](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the server for object upload.md), the application server must identify the objects you have uploaded and the names of the objects. If images are uploaded, the application server must identify the sizes of the images. To address this need, OSS provides the upload callback solution.

## Process

The following process describes how to implement upload callback:

If the user wants to upload an object to OSS and expects the application server to receive the upload result, the user must configure a callback function so that OSS can send the request to the application server. After the user uploads the object, the result is not sent directly to the user. Instead, the result is sent to the application server and then forwarded to the user.

## Examples

The following examples describe how to add signatures on the server, configure upload callback, and directly transfer data in various programming languages.

-   [PHP]()
-   [Java]()
-   [Python]()
-   [Go]()
-   [Ruby]()
-   [.NET]()

## Process analysis

The service of signature-based upload on the server, configuring upload callback, and directly transferring data are available in PHP, Java, Python, Go, and Ruby. Process details:

1.  The user requests the upload policy and callback configurations from the application server.
2.  The application server returns the upload policy and the callback code to the user.

    The service of signature-based upload on the application server and directly transferring data processes the GET request from the client. You can configure the corresponding code so that the application server returns a correct message to the client. The configuration documents for different programming languages provide clear instructions for your reference.

3.  The user sends an object upload request directly to OSS.
4.  OSS sends a callback request to the application server based on the callback configurations of the user.

    After the client completes uploading the object to OSS, OSS analyzes the upload callback configurations of the client, and sends the POST callback request to the application server.

5.  The application server returns the response to OSS.

    The application server verifies the identification information based on `authorization` in the message from OSS. If the verification is successful, the application server returns the following message in the JSON format to OSS:

    ```
    String value: OK
    Key: Status
    ```

6.  OSS sends the message that is returned by the application server to the user.

## Client source code analysis

The client source code is available at [aliyun-oss-appserver-js-master.zip](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/86983/APP_zh/1537971352825/aliyun-oss-appserver-js-master.zip?spm=a2c4g.11186623.2.16.6267691cCjEAw3&file=aliyun-oss-appserver-js-master.zip).

**Note:** Plupload is used for the client JavaScript code. Plupload is a simple, easy-to-use, and powerful file uploading tool with extensive functions. It provides multiple upload methods, including uploads by using HTML, Flash, Silverlight, and HTML4. Plupload detects the current environment to select the most suitable upload method. HTML5 is prioritized. For more information about Plupload, visit [Plupload.com](https://www.plupload.com/).

The following code provides an example on how to implement some key functions.

-   Specify that the names of uploaded objects are randomly specified by the system

    To instruct the system to randomly specify the object name without changing the name extension, you can use the following code to modify the function:

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   Specify that the names of uploaded objects remain unchanged

    To retain the original object name, you can use the following code to modify the function:

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   Configure the destination directory

    The destination directory is specified by the server. You can upload objects only to a specified directory to implement isolation. The following code provides an example on how to set the destination directory to abc/ in PHP. The destination directory must end with a forward slash \(/\).

    ```
    $dir ='abc/';
    ```

-   Set upload conditions

    You can use Plupload filters to set upload conditions, such as uploading only images, the size of objects you want to upload, or disabling the upload of duplicate objects.

    ```
    var uploader = new plupload.Uploader({
        ……
        filters: {
            mime_types : [ // Only images and ZIP files can be uploaded.
            { title : "Image files", extensions : "jpg,gif,png,bmp" },
            { title : "Zip files", extensions : "zip" }
            ], 
            max_file_size : '400kb', // The size of the object you want to upload cannot exceed 400 KB.
            prevent_duplicates : true // Objects cannot be uploaded repeatedly.
        },
    ```

    -   mime\_types: specifies the extensions of names of the objects to upload.
    -   max\_file\_size: specifies the maximum size of the objects to upload.
    -   prevent\_duplicates: specifies that the same object cannot be uploaded repeatedly.
-   Obtain the names of uploaded objects

    You can use Pupload to call the FileUploaded event and obtain the name of an uploaded object.

    ```
    FileUploaded: function(up, file, info) {
                if (info.status == 200)
                {
                    document.getElementById(file.id).getElementsByTagName('b')[0].innerHTML = 'upload to oss success, object name:' + get_uploaded_object_name(file.name);
                }
                else
                {
                    document.getElementById(file.id).getElementsByTagName('b')[0].innerHTML = info.response;
                }
        }
    ```

    You can use the `get_uploaded_object_name(file.name)` function to obtain the name of the object that is uploaded to OSS, where the `file.name` parameter is the name of the object before the object is uploaded.

-   Upload the signature

    You can obtain the policyBase64, accessid, and signature variables from the server. An example of the core code:

    ```
    function get_signature()
    {
        // Determine whether the value of the expire parameter exceeds the current time. If the value is within three seconds after the current time, you can still obtain the signature.
        now = timestamp = Date.parse(new Date()) / 1000; 
        if (expire < now + 3)
        {
            body = send_request()
            var obj = eval ("(" + body + ")");
            host = obj['host']
            policyBase64 = obj['policy']
            accessid = obj['accessid']
            signature = obj['signature']
            expire = parseInt(obj['expire'])
            callbackbody = obj['callback'] 
            key = obj['dir']
            return true;
        }
        return false;
    };
    ```

    Analysis of the message returned by the server:

    **Note:** The message does not have a specific format. However, the accessid, policy, and signature fields are required in the message.

    ```
    {"accessid":"6MKO******4AUk44",
    "host":"http://post-test.oss-cn-hangzhou.aliyuncs.com",
    "policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0xMS0wNVQyMDoyMzoyM1oiLCJjxb25kaXRpb25zIjpbWyJjcb250ZW50LWxlbmd0aC1yYW5nZSIsMCwxMDQ4NTc2MDAwXSxbInN0YXJ0cy13aXRoIiwiJGtleSIsInVzZXItZGlyXC8iXV19",
    "signature":"I2u57FWjTKqX/AE6doIdyff151E=",
    "expire":1446726203,"dir":"user-dir/"}
    ```

    -   accessid: specifies the AccessKey ID that you request.
    -   host: specifies the endpoint to which you send the upload request.
    -   policy: specifies the form upload policy, which is a string encoded in Base64. For more information, see [PostObject](/intl.en-US/API Reference/Object operations/Basic operations/PostObject.md).
    -   signature: specifies the string that is obtained after signature parameters are added to the Policy variable.
    -   expire: specifies the time when the upload policy expires. The value of this parameter is specified by the server. Before the expiration time, you can repeatedly use the policy to upload objects. You do not need to obtain signatures from the server for each upload.
    **Note:** To reduce the pressure on the server side, obtain the signature for each object that you upload for the first time. When you upload the object again, you can compare the current time with the expiration time of the signature to verify whether the signature has expired. If the signature has expired, you must obtain the signature again. If the signature has not expired, you can use the existing signature.

    Policy analysis:

    ```
    {"expiration":"2015-11-05T20:23:23Z",
    "conditions":[["content-length-range",0,1048576000],
    ["starts-with","$key","user-dir/"]]
    ```

    In the preceding example, the starts-with field is added to the policy to specify that the object name must start with user-dir. You can also customize this field. The reason for adding the starts-with field is that in most scenarios, each application corresponds to a single bucket. To prevent data from being overwritten, objects uploaded to OSS by users can be assigned a prefix. However, the user can upload multiple objects when the policy is valid. Therefore, the user can manually modify the prefix of the object, and upload the object to the folder of another user. To resolve this problem, the prefix of the object is specified by the application server. However, even if the user obtains the policy, the user cannot upload the object to the directory of another user. Therefore, this method ensures data security.

-   Configure the URL of the application server

    In the `upload.js` file of the client source code package, the value of the `serverUrl` variable in the following snippet can be used to configure the URL of the application server. After the URL of the application server is configured, the client sends a GET request to the `serverUrl` to obtain required information.

    ```
    // serverUrl specifies the URL of the application server used to obtain information such as the signature and policy. Replace the IP address and port number with your actual information.
    serverUrl = 'http://88.88.88.88:8888'
    ```


## FAQ

How do I upload multiple objects to OSS at a time?

OSS cannot upload multiple objects at a time. To upload multiple objects to OSS at a time, you can repeat the process of single object upload described in this topic.

