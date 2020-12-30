# FAQ

This topic lists the causes and solutions for common errors you may encounter when you use OSS SDK for Java.

## JAR conflicts

-   Cause

    If a similar output is displayed when you use OSS SDK for Java, it indicates that your project has a JAR conflict.

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/ssl/TrustStrategy
        at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
        at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
        at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:77)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.ssl.TrustStrategy
        at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
        ... 3 more
                        
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoSuchFieldError: INSTANCE
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<init>(DefaultHttpRequestWriterFactory.java:52)
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<init>(DefaultHttpRequestWriterFactory.java:56)
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<clinit>(DefaultHttpRequestWriterFactory.java:46)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:82)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:95)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:104)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<clinit>(ManagedHttpClientConnectionFactory.java:62)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager$InternalConnectionFactory.<init>(PoolingHttpClientConnectionManager.java:572)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:174)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:158)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:149)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:125)
     at com.aliyun.oss.common.comm.DefaultServiceClient.createHttpClientConnectionManager(DefaultServiceClient.java:237)
     at com.aliyun.oss.common.comm.DefaultServiceClient.<init>(DefaultServiceClient.java:78)
     at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
     at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
     at OSSManagerImpl.upload(OSSManagerImpl.java:42)
     at OSSManagerImpl.main(OSSManagerImpl.java:63)
                        
    ```

    The error occurs because of a version conflict between the Apache HttpClient or Commons HttpClient of your project and Apache HttpClient 4.4.1 of OSS SDK for Java. To view the JAR package and its version that is used in your project, run the `mvn dependency:tree` command in the directory of the project. The following figure shows that the project uses Apache HttpClient 4.3.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1074459951/p13397.png)

-   Solution

    You can use one of the following methods to resolve the JAR package conflict:

    -   Use consistent versions of Apache HttpClient. If your project uses a version of Apache HttpClient that conflicts with Apache HttpClient 4.4.1, use Apache HttpClient 4.4.1 and delete all other Apache HttpClient version dependencies from the pom.xml file. If your project uses Commons HttpClient, conflicts may also occur. To resolve these conflicts, delete Commons HttpClient.
    -   Resolve dependency conflicts. If your project is dependent on multiple third-party packages and the third-party packages are dependent on different versions of Apache HttpClient, your project may contain dependency conflicts. Use exclusion to resolve these conflicts. For more information, visit [Maven Guides](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).
    OSS SDK for Java is dependent on the following package versions. The solution to conflicts is similar to that for HttpClient.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1074459951/p13398.png)


## Missing packages

-   Cause

    If a similar output is displayed when you use OSS SDK for Java, your project may lack the necessary packages to compile or run OSS SDK for Java:

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/auth/Credentials
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:76)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.auth.Credentials
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 3 more
                        
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/protocol/HttpContext
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:76)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.protocol.HttpContext
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 3 more
                        
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/jdom/input/SAXBuilder
            at com.aliyun.oss.internal.ResponseParsers.getXmlRootElement(ResponseParsers.java:645)
            at … … 
            at com.aliyun.oss.OSSClient.doesBucketExist(OSSClient.java:471)
            at com.aliyun.oss.OSSClient.doesBucketExist(OSSClient.java:465)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:82)
    Caused by: java.lang.ClassNotFoundException: org.jdom.input.SAXBuilder
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 11 more
                        
    ```

    Dependencies of OSS SDK for Java:

    -   aliyun-sdk-oss-2.2.1.jar
    -   hamcrest-core-1.1.jar
    -   jdom-1.1.jar
    -   commons-codec-1.9.jar
    -   httpclient-4.4.1.jar
    -   commons-logging-1.2.jar
    -   httpcore-4.4.1.jar
    -   log4j-1.2.15.jar
    All packages except the log4j-1.2.15.jar package are required for OSS SDK for Java. However, if you want to enable logging, you must also include the log4j-1.2.15.jar package.

-   Solution

    You can use one of the following methods to add OSS SDK for Java dependencies to your project:

    -   If your project is in Eclipse, import dependencies to Eclipse, as instructed in the Method 2 section of [Installation](/intl.en-US/SDK Reference/Java/Installation.md).
    -   If your project is in Ant, add OSS SDK for Java dependencies to the lib directory.
    -   If you directly use the .javac or .java file, run the `-classpath` or `-cp` command to specify the path where OSS SDK for Java dependencies are stored, or save SDK for Java dependencies to classpath.

## Connection timeout

-   Cause

    If a similar output is displayed when you run the OSS SDK for Java program, possible causes are endpoint errors or unavailable networks:

    ```
    com.aliyun.oss.ClientException: SocketException
        at com.aliyun.oss.common.utils.ExceptionFactory.createNetworkException(ExceptionFactory.java:71)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:116)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequestImpl(ServiceClient.java:121)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequest(ServiceClient.java:67)
        at com.aliyun.oss.internal.OSSOperation.send(OSSOperation.java:92)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:140)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:111)
        at com.aliyun.oss.internal.OSSBucketOperation.getBucketInfo(OSSBucketOperation.java:1152)
        at com.aliyun.oss.OSSClient.getBucketInfo(OSSClient.java:1220)
        at com.aliyun.oss.OSSClient.getBucketInfo(OSSClient.java:1214)
        at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:94)
    Caused by: org.apache.http.conn.HttpHostConnectException: Connect to oss-test.oss-cn-hangzhou-internal.aliyuncs.com:80 [oss-test.oss-cn-hangzhou-internal.aliyuncs.com/10.84.135.99] failed: Connection timed out: connect
        at org.apache.http.impl.conn.DefaultHttpClientConnectionOperator.connect(DefaultHttpClientConnectionOperator.java:151)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.connect(PoolingHttpClientConnectionManager.java:353)
        at org.apache.http.impl.execchain.MainClientExec.establishRoute(MainClientExec.java:380)
        at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:236)
        at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
        at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
        at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
        at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:113)
        ... 9 more
                        
    ```

-   Solution

    You can use [ossutil](/intl.en-US/Tools/ossutil/Common commands/probe.md) to identify and troubleshoot problems.


## org.apache.http.NoHttpResponseException: The target server failed to respond

-   Cause analysis

    If a similar output is displayed when you run the OSS SDK for Java program:

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1074459951/p13399.png)

    The preceding error may occur if expired connections are used. This error occurs only when an SDK for Java version earlier than 2.1.2 is used.

-   Solution

    You can upgrade OSS SDK for Java to V2.1.2 or later.


## What do I do if JVM contains a large number of org.apache.http.impl.conn.PoolingHttpClientConnectionManager instances?

-   Cause

    ossClient is not disabled.

-   Solution

    You can disable the ossClient that is executed or use the single instance mode.


## What do I do if OSS SDK for Java stops responding when OSS SDK for Java is called?

-   Cause

    OSS SDK for Java stops responding when OSS SDK for Java is called. Run the `jstack -l pid` command to view the stack. A similar error is displayed:

    ```
    "main" prio=6 tid=0x000000000291e000 nid=0xc40 waiting on condition [0x0000000002dae000]
    java.lang.Thread.State: WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x00000007d85697f8> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2043)
        at org.apache.http.pool.PoolEntryFuture.await(PoolEntryFuture.java:138)
        at org.apache.http.pool.AbstractConnPool.getPoolEntryBlocking(AbstractConnPool.java:306)
        at org.apache.http.pool.AbstractConnPool.access$000(AbstractConnPool.java:64)
        at org.apache.http.pool.AbstractConnPool$2.getPoolEntry(AbstractConnPool.java:192)
        at org.apache.http.pool.AbstractConnPool$2.getPoolEntry(AbstractConnPool.java:185)
        at org.apache.http.pool.PoolEntryFuture.get(PoolEntryFuture.java:107)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.leaseConnection(PoolingHttpClientConnectionManager.java:276)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager$1.get(PoolingHttpClientConnectionManager.java:263)
        at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:190)
        at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
        at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
        at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
        at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:113)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequestImpl(ServiceClient.java:123)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequest(ServiceClient.java:68)
        at com.aliyun.oss.internal.OSSOperation.send(OSSOperation.java:94)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:146)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:113)
        at com.aliyun.oss.internal.OSSObjectOperation.getObject(OSSObjectOperation.java:229)
        at com.aliyun.oss.OSSClient.getObject(OSSClient.java:629)
        at com.aliyun.oss.OSSClient.getObject(OSSClient.java:617)
        at samples.HelloOSS.main(HelloOSS.java:49)
                        
    ```

    The error is caused by connection leaks in the connection pool, which occur when the ossObject is not closed after usage.

-   Solution

    Check your program to ensure that no connection leaks occur. The following code provides an example on how to close ossObject:

    ```
    // Read an object.
    OSSObject ossObject = ossClient.getObject(bucketName, objectName);
    // Perform operations on OSS.
    // Shut down the ossObject.
    ossObject.close();
                        
    ```


## Connection closure

-   Cause

    If a similar output is displayed when you use ossClient.getObject:

    ```
    Exception in thread "main" org.apache.http.ConnectionClosedException: Premature end of Content-Length delimited message body (expected: 11990526; received: 202880)
        at org.apache.http.impl.io.ContentLengthInputStream.read(ContentLengthInputStream.java:180)
        at org.apache.http.impl.io.ContentLengthInputStream.read(ContentLengthInputStream.java:200)
        at org.apache.http.impl.io.ContentLengthInputStream.close(ContentLengthInputStream.java:103)
        at org.apache.http.impl.execchain.ResponseEntityProxy.streamClosed(ResponseEntityProxy.java:128)
        at org.apache.http.conn.EofSensorInputStream.checkClose(EofSensorInputStream.java:228)
        at org.apache.http.conn.EofSensorInputStream.close(EofSensorInputStream.java:174)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at com.aliyun.oss.event.ProgressInputStream.close(ProgressInputStream.java:147)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at samples.HelloOSS.main(HelloOSS.java:39)
                        
    ```

    The error occurs because the interval between the two data reading attempts exceeds one minute. OSS closes the connection if no data is sent or received for more than one minute.

-   Solution

    If you must read part of the data during unspecified time periods, we recommend that you use range download to avoid connection closure during data reading. For more information, see [Range download](/intl.en-US/SDK Reference/Java/Download objects/Range download.md).


## Memory leaks

-   Cause

    Memory leaks occur when the OSS SDK for Java program runs for an extended period of time. This period can range from a few hours to several days based on the amount of access traffic. We recommend that you use the [Eclipse Memory Analyzer \(MAT\)](http://www.eclipse.org/mat/downloads.php?) to analyze memory usage. For more information, visit [Use MAT for Heap Dump File Analysis](https://www.ibm.com/developerworks/cn/opensource/os-cn-ecl-ma/).

    If a similar analysis result is shown in the following figure, PoolingHttpClientConnectionManager takes up 96% of memory. The possible cause is that the new OSSClient command is run multiple times whereas ossClient.shutdown is not called. As a result, memory leaks occur.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2074459951/p13400.png)

-   Solution

    Make sure that ossClient.shutdown is called after the new OSSClient command is run.


## InterruptedException occurs when ossClient.shutdown is called

-   Cause

    A similar error is displayed when ossClient.shutdown is called:

    ```
    java.lang.InterruptedException: sleep interrupted
            at java.lang.Thread.sleep(Native Method)
            at com.aliyun.oss.common.comm.IdleConnectionReaper.run(IdleConnectionReaper:76)
                        
    ```

    The error occurs because the backend thread \(IdleConnectionReaper\) of ossClient closes idle connections periodically. If ossClient.shutdown is called when IdleConnectionReaper is in Sleep mode, the preceding exception occurs.

-   Solution

    This exception has been resolved in OSS SDK for Java V2.3.0.

    For versions earlier than OSS SDK for Java V2.3.0, use the following code to ignore the exception:

    ```
    try {
        ossClient.shutdown();
    } catch(Exception e) {
    }
                        
    ```


## What do I do if the "SDK.ServerUnreachable: Specified endpoint or uri is not valid" exception occurs?

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7576498951/p38848.png)

-   Cause

    Possible causes: requests sent from the client to STS with high concurrency, timeout of connections to the server, and the invalid STS SDK or the SDK core version. Consequently, the connections to the Alibaba Cloud endpoint fail.

-   Solution
    -   When ECS instances on the client or local PC are unable to support bursts of requests, you can reduce the amount of concurrent requests sent to STS.
    -   If the connection to the server times out, you can check and analyze captured packets.
    -   We recommend that you upgrade STS SDK or SDK core to the latest version.

## NoSuchKey

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7576498951/p38849.jpg)

-   Cause

    This error is displayed because the source file does not exist.

-   Solution

    For more information, visit [Troubleshoot 404 errors](https://yq.aliyun.com/articles/657166).


## SocketException

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7576498951/p38851.png)

-   Cause

    The error is displayed because the socket failed to be specified during initialization. As a result, the request was not received by OSS.

-   Solution

    We recommend that you troubleshoot the exception based on the following aspects:

    -   Check whether jitters occur.
    -   Check whether socket resources are used up by other processes.
    -   Check the maximum number of connections configured in the SDK. If the actual number of connections exceeds this connection limit, a socket exception occurs.
    If the problem persists, we recommend that you use tcpdump or wireshark to capture packets, reproduce the exception, and analyze the packets.


## What do I do if the callback operation of OSS PostObject fails but the callback operation of PutObject succeeds?

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7576498951/p38913.png)

In most cases, if the JSON format is invalid or a callback fails, an error message is returned. In this case, you must test whether the callback operations of PUT and POST are triggered.

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7576498951/p38868.png)

-   Cause

    The callback parameter follows the file parameter when you send the request.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8576498951/p38909.png)

-   Solution

    You can adjust the position of the callback parameter.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8576498951/p38910.png)

    The test result shows that the server captures the request.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8576498951/p38911.png)


## Connection pool shut down

```
Caused by: java.lang.IllegalStateException: Connection pool shut down
  at org.apache.http.util.Asserts.check(Asserts.java:34)
  at org.apache.http.pool.AbstractConnPool.lease(AbstractConnPool.java:184)
  at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.requestConnection(PoolingHttpClientConnectionManager.java:251)
  at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:175)
  at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
  at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
  at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
  at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
  at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:124)
  at com.aliyun.oss.common.comm.ServiceClient.sendRequestImpl(ServiceClient.java:133)
  ... 8 more
```

-   Cause

    After ossClient.shutdown\(\) is called, requests are sent by using ossClient.

-   Solution

    You can check the call logic. Ensure that no requests are sent by using ossClient after ossClient.shutdown\(\) is called.


## What do I do if "Request has expired" is returned when the request is generated by using generatePresignedUrl of OSS SDK for Java?

-   Cause

    An integer overflow occurs, which results in the Year 2038 problem.

    An upload request is initiated when the expiration time specified for the URL is exceeded.

-   Solution

    If the integer overflows, we recommend that you set the expiration time for the URL in OSS SDK for Java to a value earlier than the year 2038.

    If an upload request is initiated after the expiration time specified by the URL is exceeded, we recommend that you set a proper expiration time that is later than the time when you initiated the request.


## What do I do if the "SignatureDoesNotMatch" error is returned?

-   Cause 1

    -   The version of the OSS SDK for Java that you use is earlier than 3.7.0 and the httpclient of version 4.5.9 and later is used in your project.
    -   The name of the uploaded object contains a plus sign \(`+`\). However, the httpclient of version 4.5.9 does not encode the plus sign \(`+`\) by using URL encoding. Therefore, this error is returned to indicate that the signatures on the client and server does not match.
    ![1](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/9831239061/p184586.png)

    Solution

    -   We recommend that you upgrade your OSS SDK for Java to version 3.11.1 or later to ensure the compatibility with the httpclient of version 4.5.9.
    -   Remove the unnecessary dependency on httpclient. The dependency on httpclient is automatically imported when you import OSS SDK for Java. If httpclient is imported by third-party libraries, see the solution described in [JAR conflicts](#section_u5d_qgd_kfb).
-   Cause 2

    The httpclient of version 4.5.10 or later is imported in your project and the request headers contain characters that are not supported by ISO/9959-1. For example, the headers whose names start with `x-oss-meta-` contain Chinese characters.

    Solution

    -   Follow the solution described in [JAR conflicts](#section_u5d_qgd_kfb) to remove the httpclient of version 4.5.10 or later.
    -   Ensure that the characters contained in request headers are supported by ISO/9959-1.

## What do I do if the "Invalid Response" or "Implementation of JAXB-API has not been found on module path or classpath" exception occurs?

-   Cause

    Java 9 or later is installed and the dependencies on JAXB are not added.

-   Solution

    For more information about how to add the dependencies on JAXB, see [Install OSS SDK for Java](/intl.en-US/SDK Reference/Java/Installation.md).


## Other errors

For more information about how to resolve other errors returned by OSS, see [OSS 403]().

