# Configure back-to-origin rules

If you access data in a bucket that has no back-to-origin rules configured and the data does not exist, 404 Not Found is returned. However, if you configure back-to-origin rules that contain the correct origin URL, you can obtain the data based on the back-to-origin rules.

Back-to-origin supports the mirroring and redirection modes. You can configure back-to-origin rules for hot migration and specific request redirection.

-   Mirroring

    After mirroring-based back-to-origin rules are configured for a bucket, you can obtain an object in the bucket based on the rules when the requested object is not found. For example, when you perform the GetObject operation on an object and the object is not found, OSS retrieves the object based on the origin URL, returns the object, and writes the object to OSS. Mirroring-based back-to-origin rules are used to seamlessly migrate data to OSS. This feature allows you to migrate any service that already runs on a user-created origin or in another cloud service to OSS without interrupting services.

-   Redirection

    After redirection-based back-to-origin rules are configured for a bucket, you can obtain an object in the bucket based on the rules when the requested object is not found. For example, when you perform the GetObject operation on an object and the object is not found, OSS redirects the request to the origin URL, and then a browser or client returns the content from the origin. You can use this feature to redirect requests for objects and develop various services based on redirection.


You can configure up to 20 back-to-origin rules, which are run in a sequence that they are configured. For more information, see [Manage back-to-origin configurations](/intl.en-US/Developer Guide/Objects/Manage files/Manage back-to-origin configurations.md).

**Note:** Back-to-origin rules and versioning cannot be configured for a bucket at the same time.

## Configure a mirroring-based back-to-origin rule

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Basic Settings** \> **Back-to-Origin**.

4.  Click **Configure**. Click **Create Rule**.

5.  In the **Create Rule** pane, configure a back-to-origin rule.

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |**Mode**|Yes|Select **Mirroring**. In this mode, when a requested object cannot be found in OSS, OSS automatically retrieves the object from the origin, stores it in OSS, and returns the content to the requester.|
    |**Prerequisite**|Yes|Configure the conditions that trigger the back-to-origin rule. A rule is triggered only when all conditions are met.     -   **HTTP Status Code**: The back-to-origin rule is triggered when the specified HTTP status code is returned. The default HTTP status code is 404, which indicates that the rule is triggered when the requested object is not found in OSS and HTTP status code 404 is returned. By default, this option is selected when you select the **Mirroring** mode.
    -   **File Name Prefix**: The back-to-origin rule is triggered when the name of the requested object contains the specified prefix. For example, when this parameter is set to abc/, the back-to-origin rule is triggered when you access `https://bucketname.oss-endpoint.com/abc/image.jpg`.
    -   **File Name Suffix**: The back-to-origin rule is triggered when the name of the requested object contains the specified suffix. For example, when this parameter is set to .jpg, the back-to-origin rule is triggered when you access `https://bucketname.oss-endpoint.com/image.jpg`.
**Note:** File Name Prefix and File Name Suffix are optional when you configure only one back-to-origin rule. When you configure multiple back-to-origin rules, you must differentiate the rules by specifying a different prefix or suffix for each of them. |
    |**Replace or Delete File Prefix**|No|You can configure this parameter after you select and configure **File Name Prefix**. When OSS sends a request to the origin, the content of **File Name Prefix** is replaced with that of **Replace or Delete File Prefix**. Example: You want to store the object obtained from the origin in the mirror folder under the root folder of the bucket. If the name of the requested object is path/test/photo.jpg, set **File Name Prefix** to mirror/, **Replace or Delete File Prefix** to test/, and the third column of **Origin URL** to path. In this case, when you access `https://bucketname.oss-endpoint.com/mirror/photo.jpg`, if photo.jpg does not exist, OSS sends the `https://origin URL/path/test/photo.jpg` request to obtain this object. If the object is obtained from the origin, OSS stores this object in the mirror/ folder and returns the object to you. |
    |**Origin URL**|Yes|Configure the information about the origin URL.     -   First column: Select HTTP or HTTPS based on the protocol used by the origin.

**Note:** If SNI is enabled on the origin, HTTPS that you select does not take effect.

    -   Second column: Enter the domain name or IP address of the origin. Internal endpoints and IP addresses are not supported. If you enter an IP address, ensure that the origin corresponding to the IP address can be accessed by using the IP address.
    -   Third column: Enter the folder where the requested object is stored. Separate subfolders in a folder with forward slashes \(/\). Example: `abc/123`. |
    |**MD5 Verification**|No|If this option is selected, the MD5 hash of the object obtained from the origin is checked. When the response contains the Content-MD5 header value, OSS checks whether the MD5 hash of the object matches the Content-MD5 header value.     -   If the MD5 hash of the object matches the Content-MD5 header value, the client obtains the object, and OSS saves the object by using mirroring-based back-to-origin.
    -   If the MD5 hash of the object does not match the Content-MD5 header value, OSS calculates the Content-MD5 header value of the object based on the data integrity and does not save the object. However, the client can obtain the object because the object is returned to the client. |
    |**Keep Forward Slash in Origin URL**|No|OSS does not support object names that start with a forward slash \(/\). Therefore, when the name of the requested object starts with a forward slash \(/\), you must select this option to obtain the requested object based on the back-to-origin rules.For example, the origin is `https://www.example.com`, the name of the requested object is /object.txt, and the name of the bucket in the China \(Hangzhou\) region is examplebucket. If this option is selected, the default public endpoint to access the requested object is `https://examplebucket.oss-cn-hangzhou.aliyuncs.com//object.txt`, and the origin URL is `https://www.example.com//object.txt`. The object is obtained from OSS, returned to the client, and saved in the bucket as object.txt.

**Note:** This feature is in public preview in the US \(Silicon Valley\), US \(Virginia\), and China \(Hangzhou\) regions.You can [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply for trial. |
    |**Other Parameter**|No|If this option is selected, the query string included in the request to OSS is transferred to the origin.|
    |**3xx Response**|No|If this option is selected, OSS follows the origin to direct the request to obtain the resource and stores the resource in buckets. If this option is not selected, OSS passes the 3xx response without obtaining the resource.|
    |**Set Transmission Rule of HTTP Header**|No|You can configure the transmission rule for HTTP headers to customize transmission actions such as passthrough, filtering, or modification. For more information, see the following examples of transmission rule configurations for HTTP headers.|

    Examples of transmission rule configurations for HTTP headers:

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2867549951/p9983.png)

    If the request sent to OSS contains the following HTTP headers:

    ```
    GET /object
    host : bucket.oss-cn-hangzhou.aliyuncs.com
    aaa-header : aaa
    bbb-header : bbb
    ccc-header : 111
    ```

    After the mirroring-based back-to-origin rule is triggered, the request that OSS sends to the origin is:

    ```
    GET /object
    host : source.com
    aaa-header : aaa
    ccc-header : ccc                       
    ```

    Transmission rules do not support the following HTTP headers:

    -   Headers that contain the following prefixes:
        -   x-oss-
        -   oss-
        -   x-drs-
    -   All standard HTTP headers. Examples:
        -   content-length
        -   authorization2
        -   authorization
        -   range
        -   date

## Configure a redirection-based back-to-origin rule

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).

2.  Click **Buckets**, and then click the name of the target bucket.

3.  Choose **Basic Settings** \> **Back-to-Origin**.

4.  Click **Configure**. Click **Create Rule**.

5.  In the **Create Rule** dialog box, configure a redirection-based back-to-origin rule.

    |Parameter|Required|Description|
    |:--------|:-------|:----------|
    |**Mode**|Yes|Select **Redirection**. In this mode, OSS redirects requests that meet the prerequisites to the origin URL over HTTP, and then a browser or client returns the content from the origin to the requester.|
    |**Prerequisite**|Yes|Configure the conditions that trigger the back-to-origin rule. A rule is triggered only when all conditions are met.     -   **HTTP Status Code**: The back-to-origin rule is triggered when the specified HTTP status code is returned. The default HTTP status code is 404, which indicates that the rule is triggered when the requested object is not found in OSS and HTTP status code 404 is returned.
    -   **File Name Prefix**: The back-to-origin rule is triggered when the name of the requested object contains the specified prefix. For example, when this parameter is set to abc/, the back-to-origin rule is triggered when you access `https://bucketname.oss-endpoint.com/abc/image.jpg`.
    -   **File Name Suffix**: The back-to-origin rule is triggered when the name of the requested object contains the specified suffix. For example, when this parameter is set to .jpg, the back-to-origin rule is triggered when you access `https://bucketname.oss-endpoint.com/image.jpg`.
**Note:** File Name Prefix and File Name Suffix are optional when you configure only one back-to-origin rule. When you configure multiple back-to-origin rules, you must differentiate the rules by specifying a different prefix or suffix for each of them. |
    |**Replace or Delete File Prefix**|No|You can configure this parameter after you select and configure **File Name Prefix**. When OSS sends a request to the origin, the content of **File Name Prefix** is replaced with that of **Replace File Name Prefix**. **Note:** If this option is selected, only **Origin URL** can be set for **Replace File Name Prefix**. |
    |**Origin URL**|Yes|Configure the information about the origin URL. You must configure the back-to-origin rule based on the origin URL. Rules of configuring the origin URL:

    -   First column: Select HTTP or HTTPS based on the protocol used by the origin.
    -   Second column: Enter the domain name or IP address of the origin.

**Note:** You can enter only IP addresses of origins that can be directly accessed by using IP addresses.

    -   Third column: Configure the redirection-based back-to-origin rule based on the prefix and suffix configurations.

        -   **Add Prefix or Suffix**: Add a prefix or suffix to the redirected URL. The prefix is configured in the third column. The suffix is configured in the fourth column.

You can select this option to configure correct information for the redirected URL when the URL of the requested data excludes a prefix or suffix. If you set the prefix to 123/ and the suffix to .jpg, access to `https://bucketname.oss-endpoint.com/image` is automatically redirected to `https://Origin URL/123/image.jpg`.

        -   **Redirect to Fixed URL**: Access to the requested object is redirected to a specified object. The object address is specified in the third column. Example: `abc/myphoto.jpg`.

**Note:** You can also set a website URL. For example, if you set the URL to `https://www.aliyun.com/index.html`, access to the bucket is redirected to the Alibaba Cloud homepage.

        -   **Replace File Name Prefix**: If you set this parameter and **File Name Prefix** for Prerequisite, the prefix in the name of the object to which the access is redirected is replaced with that of the third column. If you set this parameter without setting **File Name Prefix** for Prerequisite, the specified prefix is added to the name of the object to which the access is redirected.
Separate subfolders in a folder with forward slashes \(/\). The folder name must end with a forward slash \(/\). The folder name cannot contain asterisks \(\*\). |
    |**Other Parameter**|No|If this option is selected, the query string included in the request to OSS is transferred to the origin.|
    |**Redirection Code**|Yes|You can select the redirection code from the drop-down list. Select **Source from Alibaba Cloud CDN** if the redirect request is from Alibaba Cloud CDN.|

6.  Click **OK**.


