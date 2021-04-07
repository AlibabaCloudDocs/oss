# Troubleshooting

This topic provides answers to frequently asked questions about ossbrowser.

## What do I do if the "AccessDenied:You are forbidden to list buckets" error appears when I use an AccessKey pair to log on to OSS from ossbrowser?

Cause: You do not have permissions to access all buckets.

Solution:

-   Grant your Alibaba Cloud account the permissions and then log on to ossbrowser.
-   If your Alibaba Cloud account is not authorized to access all of the objects or buckets, log on to ossbrowser by using the AccessKey pair of an Alibaba Cloud account that has permissions to access all objects or buckets. Set **Preset OSS Paths** and **Region**.

## How do I increase the number of concurrent uploads and downloads when I need to upload or download a large number of objects?

Cause: By default, ossbrowser performs three upload or download tasks at the same time while the remaining tasks wait in the queue. Adjusting the number of concurrent uploads and downloads in the default setting will affect the transfer speed.

Solution: On the homepage of ossbrowser, click **Settings** in the top navigation bar. In the Settings dialog box, modify Transfer Settings. The number of concurrent uploads and downloads supported is determined by the hardware on which ossbrowser is installed. We recommend that you adjust and test the settings multiple times to optimize performance.

## What do I do if I cannot run ossbrowser on macOS?

Cause: Applications that are not verified by developers cannot be run on macOS due to security settings.

Solution: Modify the security settings on macOS. Allow applications to run from any source.

1.  On the command line in Linux, enter a command to disable the safe mode.

    ```
    sudo spctl --master-disable
    ```

2.  On the desktop of macOS, choose **Launchpad** \> **System Preferences** \> **Security & Privacy** \> **General**.
3.  Click the ![Lock](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1914459951/p133107.png) icon in the lower-left corner. In the dialog box that appears, set User Name and Password. Click **Unlock**.
4.  In the **Allow apps downloaded from** section, click **Anywhere**.

    After you complete these configurations, you can run ossbrowser on macOS.


## What do I do if I cannot run ossbrowser after I install a software on Windows?

Cause: The software that you install adds the `NODE_OPTIONS=--max-http-header-size=value` environment variable to Windows.

Solution: Delete the `NODE_OPTIONS=--max-http-header-size=value` environment variable. The following example describes how to delete the environment variable on the 64-bit version of Windows 10:

1.  Open the Control Panel. Click System and Security. On the page that appears, Click System.
2.  In the left-side navigation pane, click **Advanced system settings**.
3.  On the Advanced tab of the **System Properties** dialog box, click **Environment Variables**.
4.  In the **User variables** list, select **NODE\_OPTIONS=--max-http-header-size=value**, and then click **Delete**.
5.  Click **OK**.

    After you complete these configurations, you can run ossbrowser in Windows.


## How do I enable the debugging mode?

You can use one of the following methods to enable the debugging mode:

-   Method 1: Click the OSS Browser logo in the upper-left corner more than 10 consecutive times to go to the debugging panel.
-   Method 2: In the Settings dialog box, click **Open debug** to go to the debugging panel.

## References

For more information about frequently asked questions about ossbrowser, see [FAQ](https://github.com/aliyun/oss-browser/blob/master/docs/question.md).

