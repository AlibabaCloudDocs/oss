# Application in browsers

This topic describes the application of OSS SDK for Browser.js in browsers.

OSS SDK for Browser.js supports the following browsers:

-   Internet Explorer 10 and later versions and Microsoft Edge.
-   Mainstream Chrome/Firefox/Safari versions
-   Default browsers of mainstream versions of Android/iOS/Windows Phone systems.

**Note:** To use OSS SDK for Browser.js in Internet Explorer, you must import the promise library after the SDK is imported.

## Application in browsers of earlier versions

OSS SDK for Browser.js uses the File API to perform operations on objects. Therefore, problems may occur when you use the SDK in browsers of earlier versions. We recommend that you use third-party plug-ins to call OSS API operations to perform operations on objects, such as upload and download objects. For detailed examples, see [Add signatures on the client by using JavaScript and upload data to OSS](/intl.en-US/Best Practices/Upload data to OSS through Web applications/Use PostObject to upload data to OSS through Web applications/Add signatures on the client by using JavaScript and upload data to OSS.md).

-   Related settings
    -   Bucket settings

        To access a bucket from a browser, you must configure a [CORS](/intl.en-US/Developer Guide/Buckets/Configure CORS.md) rule for the bucket in the following method.

        -   Set Sources to `*`.
        -   Set Allowed Methods to `GET, POST, PUT, DELETE, HEAD`.
        -   Set the Allowed Headers to `*`.
        -   Set Exposed Headers to `etag`.
        **Note:** Specify this rule as the first of all CORS rules.

    -   STS settings

        To prevent the AccessKey ID and AccessKey secret from being exposed, STS is used for temporary authorization. The authorization server is maintained by users. Only authenticated users can apply for a temporary access from the authorization server. For more information about how to set a policy for RAM users and STS, visit [A simple app server using STS](https://github.com/rockuw/node-sts-app-server) to build your own authorization server.

-   Examples

    Use OSS SDK for Browser.js to develop a web application that can perform the following operations: upload objects, upload text, list objects, and download objects. For the complete sample code, visit [oss-in-browser](https://github.com/rockuw/oss-in-browser).

    The following example shows how to develop a web application to upload an object.

    1.  Create a web page.

        -   Create a selection box to choose the object to upload.
        -   Create a text box to enter the name of the object to upload.
        -   Create a button used to start the upload.
        -   Create a progress bar that displays the upload progress.
        The following code provides an example on how to create the preceding four controls, in which `class="xxx"` is included to use the [Bootstrap](http://getbootstrap.com/) style.

        ```language-html
        <div class="form-group">
          <label>Select file</label>
          <input type="file" id="file" />
        </div>
        <div class="form-group">
          <label>Store as</label>
          <input type="text" class="form-control" id="object-key-file" value="object" />
        </div>
        <div class="form-group">
          <input type="button" class="btn btn-primary" id="file-button" value="Upload" />
        </div>
        <div class="form-group">
          <div class="progress">
            <div id="progress-bar"
                 class="progress-bar"
                 role="progressbar"
                 aria-valuenow="0"
                 aria-valuemin="0"
                 aria-valuemax="100" style="min-width: 2em;">
              0%
            </div>
          </div>
        </div>
                                    
        ```

    2.  Add a style.

        Bootstrap is used to add the style. The following code provides an example on how to add the style in the `<head>` field:

        ```language-html
        <head>
          <title>OSS in Browser</title>
          <link rel="stylesheet" href="bootstrap.min.css" />
        </head>
                                    
        ```

        The `bootstrap.min.css` style can be download from the official website of [Bootstrap](http://getbootstrap.com/).

    3.  Add scripts.

        The web page is still static after the preceding two steps. You must add JavaScript scripts to the web page to perform operations on objects, such as upload, download, and list objects.

        Import the package of OSS SDK for Browser.js in the `<head>` field.

        ```language-html
        <head>
          <title>OSS in Browser</title>
          <link rel="stylesheet" href="bootstrap.min.css" />
          <script type="text/javascript" src="http://gosspublic.alicdn.com/aliyun-oss-sdk-x.x.x.min.js"></script>
          <script type="text/javascript" src="app.js"></script>
        </head>
                                    
        ```

        The `app.js` script includes the following code used to upload objects:

        ```language-js
        const appServer = 'http://localhost:3000';
        const bucket = 'js-sdk-bucket-sts';
        const region = 'oss-cn-hangzhou';
        
        const urllib = OSS.urllib;
        
        const applyTokenDo = function (func) {
          const url = appServer;
          return urllib.request(url, {
            method: 'GET'
          }).then(function (result) {
            const creds = JSON.parse(result.data);
            const client = new OSS({
              region: region,
              accessKeyId: creds.AccessKeyId,
              accessKeySecret: creds.AccessKeySecret,
              stsToken: creds.SecurityToken,
              bucket: bucket
            });
        
            return func(client);
          });
        };
        
        let currentCheckpoint;
        const progress = async function progress(p, checkpoint) {
          currentCheckpoint = checkpoint;
          const bar = document.getElementById('progress-bar');
          bar.style.width = `${Math.floor(p * 100)}%`;
          bar.innerHTML = `${Math.floor(p * 100)}%`;
        };
        
        let uploadFileClient;
        const uploadFile = function (client) {
          if (! uploadFileClient || Object.keys(uploadFileClient).length === 0) {
            uploadFileClient = client;
          }
          
          const file = document.getElementById('file').files[0];
          const key = document.getElementById('object-key-file').value.trim() || 'object';
          console.log(`${file.name} => ${key}`);
        
          const options = {
            progress,
            partSize: 100 * 1024,
            meta: {
              year: 2017,
              people: 'test',
            },
          };
        
          return client.multipartUpload(key, file, options).then( (res) => {
            console.log('upload success: %j', res);
            currentCheckpoint = null;
            uploadFileClient = null;
          }).catch((err) => {
            if (uploadFileClient && uploadFileClient.isCancel()) {
              console.log('stop-upload!') ;
            } else {
              console.error(err);
            }
          });
        };
        
        window.onload = function () {
          document.getElementById('file-button').onclick = function () {
            applyTokenDo(uploadFile);
          }
        };
                                    
        ```

        The following steps are performed to upload an object:

        1.  Apply for a temporary access from the authorization server. The authorization server is a service built by website developers to grant temporary access to end users. Developers can provide different permissions for different users. To build an authorization server, visit [GitHub](https://github.com/rockuw/node-sts-app-server). In the example, the authorization server returns a temporary access credential to users.
        2.  Use the temporary key to create an OSSClient.
        3.  Use `multipartUpload` to upload the selected objects and use `progress` to configure the progress bar.

For more information about the sample code of uploading text, listing objects, and downloading objects, visit [OSS in Browser](https://github.com/rockuw/oss-in-browser).

