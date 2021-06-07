# Installation

ossfs allows you to mount OSS buckets as local file systems in Linux. Then, you can manage OSS objects in the same manner as you manage local files.

## Runtime environment

We recommend that you run ossfs in the following environments:

-   Linux-based operating systems
    -   CentOS 7.0 or later
    -   Ubuntu 14.04 or later
-   Fuse 2.8.4 or later

ossfs is subject to errors and disconnections when run on earlier versions of Linux kernel. OSS provides installation packages for earlier versions of Linux-based operating systems. However, we recommend that you upgrade your operating system to the preceding versions to ensure the stable operation of ossfs.

## Download URLs

|Linux distribution|URL|
|:-----------------|:--|
|Ubuntu 18.04 \(x64\)|[ossfs\_1.80.6\_ubuntu18.04\_amd64.deb](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_ubuntu18.04_amd64.deb)|
|Ubuntu 16.04 \(x64\)|[ossfs\_1.80.6\_ubuntu16.04\_amd64.deb](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_ubuntu16.04_amd64.deb)|
|Ubuntu 14.04 \(x64\)|[ossfs\_1.80.6\_ubuntu14.04\_amd64.deb](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_ubuntu14.04_amd64.deb)|
|CentOS 8.0 \(x64\)|[ossfs\_1.80.6\_centos8.0\_x86\_64.rpm](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos8.0_x86_64.rpm)|
|CentOS 7.0 \(x64\)|[ossfs\_1.80.6\_centos7.0\_x86\_64.rpm](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos7.0_x86_64.rpm)|
|CentOS 6.5 \(x64\)|[ossfs\_1.80.6\_centos6.5\_x86\_64.rpm](https://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos6.5_x86_64.rpm)|

The preceding table provides the download URLs of ossfs installation packages for common operating systems. If your operating system is not included in the table, install ossfs by compiling the source code. For the source code, visit [GitHub ossfs](https://github.com/aliyun/ossfs#ossfs).

**Note:** When you copy the URLs to the wget command to download ossutil, delete the `?spm=xxxx` section from the URLs.

## Installation

1.  Download the installation package.

    For example, run the following command to download the ossfs installation package for CentOS 7.0 \(x64\):

    ```
    wget http://gosspublic.alicdn.com/ossfs/ossfs_1.80.6_centos7.0_x86_64.rpm
    ```

2.  Install ossfs.

    -   Ubuntu

        For example, you can run the following command to install ossfs in Ubuntu 16.04 \(x64\):

        ```
        sudo apt-get update
        sudo apt-get install gdebi-core
        sudo gdebi ossfs_1.80.6_ubuntu16.04_amd64.deb
        ```

    -   CentOS

        For example, you can run the following command to install ossfs in CentOS 7.0 \(x64\):

        ```
        sudo yum install ossfs_1.80.6_centos7.0_x86_64.rpm
        ```

        If your client uses Yellowdog Updater, Modified \(YUM\) to install the RPM package, dependencies may fail to be downloaded when the client node network environment is not suitable. To resolve this issue, you can use YUM to download the dependencies over the normal network to a node that runs the same operating system version, and then copy the dependencies to the required node. For example, if ossfs requires fuse 2.8.4 or later to run, you can run the following command to download the latest version of fuse from the yum source to your local device:

        ```
        sudo yum install --downloadonly --downloaddir=./ fuse
        ```

        **Note:** To download other dependencies, replace fuse with the name of the required package.

3.  Configure access information of the account.

    Store the bucket name and the AccessKey pair that can be used to access the bucket in the /etc/passwd-ossfs object. Make sure that the permission on this object is set correctly. We recommend that you set the permission to 640.

    ```
    echo BucketName:yourAccessKeyId:yourAccessKeySecret > /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    ```

4.  Mount the bucket to the specified directory.

    ```
    ossfs BucketName mountfolder -o url=Endpoint
    ```

    Mount a bucket named `bucket-test` to the `/tmp/ossfs` directory.

    ```
    echo bucket-test:LTAIbZcdVCmQ****:MOk8x0y9hxQ31coh7A5e2MZEUz**** > /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    mkdir /tmp/ossfs
    ossfs bucket-test /tmp/ossfs -o url=http://oss-cn-hangzhou.aliyuncs.com
    ```

    **Note:** If you use an Alibaba Cloud Elastic Compute Service \(ECS\) instance to provide ossfs services, you can use an internal endpoint. In the preceding example, you can set the OSS endpoint to `oss-cn-hangzhou-internal.aliyuncs.com` to minimize costs. For more information about OSS internal endpoints, see [Regions and endpoints](/intl.en-US/Developer Guide/Endpoint/Regions and endpoints.md).

5.  If you no longer need the bucket, you can run the following command to unmount it.

    ```
    fusermount -u /tmp/ossfs
    ```


**Note:** For more information, visit [GitHub ossfs](https://github.com/aliyun/ossfs#ossfs).

## Version history

For more information about the version history of ossfs, visit [GitHub ChangeLog](https://github.com/aliyun/ossfs/blob/master/ChangeLog).

