# Advanced settings

This topic describes how to configure ossfs.

ossfs is installed. For more information about how to install ossfs, see [Quick installation](/intl.en-US/Tools/ossfs/Quick installation.md).

## Configure the account information

When you use ossfs to access OSS buckets, you must configure your account information including your AccessKey ID and AccessKey secret. The account information must be written to the account configuration file in a specific format. ossfs obtains the account information in the `$bucket_name:$access_key_id:$access_key_secret` format from the account configuration file.

The default path for the account configuration file is /etc/passwd-ossfs. You can also use the -opasswd\_file=passwd-path option to specify a configuration file. The permission of a configuration file in the default path may be 640, while that in other paths must be 600.

-   An account configuration file can contain records for multiple accounts. Each line indicates the information of one account. When ossfs is used to attach a bucket, ossfs matches the bucket name with the correct account.

    Configuration examples:

    ```
    echo bucket-test-1:AAAIbZcdVCmQ****:AAA8x0y9hxQ31coh7A5e2MZEUz**** > /etc/passwd-ossfs
    echo bucket-test-2:BBBIbZcdVCmQ****:BBB8x0y9hxQ31coh7A5e2MZEUz**** >> /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    mkdir /tmp/ossfs-1
    mkdir /tmp/ossfs-2
    ossfs bucket-test-1 /tmp/ossfs-1 -ourl=http://oss-cn-hangzhou.aliyuncs.com
    ossfs bucket-test-2 /tmp/ossfs-2 -ourl=http://oss-cn-hangzhou.aliyuncs.com
    ```

-   If you want to attach multiple buckets, you can choose to write the information of every account to one account configuration file, or write the information of different accounts to different account configuration files. You can use the -opasswd\_file=xxx option to select the account configuration file.

    Configuration examples:

    ```
    echo bucket-test-3:CCCIbZcdVCmQ****:CCC8x0y9hxQ31coh7A5e2MZEUz**** > /etc/passwd-ossfs-3
    chmod 600 /etc/passwd-ossfs-3
    mkdir /tmp/ossfs-3
    ossfs bucket-test-3 /tmp/ossfs-3 -ourl=http://oss-cn-hangzhou.aliyuncs.com -opasswd_file=/etc/passwd-ossfs-3
    echo bucket-test-4:DDDIbZcdVCmQ****:DDD8x0y9hxQ31coh7A5e2MZEUz**** > /etc/passwd-ossfs-4
    chmod 600 /etc/passwd-ossfs-4
    mkdir /tmp/ossfs-4
    ossfs bucket-test-4 /tmp/ossfs-4 -ourl=http://oss-cn-hangzhou.aliyuncs.com -opasswd_file=/etc/passwd-ossfs-4
    ```


## Use instance RAM roles

In ECS, you can use ossfs based on instance RAM roles. You can bind a RAM role to an ECS instance to access OSS from the instance by using STS temporary credentials. STS temporary credentials are automatically generated and updated. Applications can obtain the STS temporary credentials by using the instance metadata URL. RAM helps protect the security of your AccessKey pair and facilitates fine-grained permission control and management. For more information about instance RAM roles, see [Instance RAM roles](/intl.en-US/Security/Instance RAM roles/Overview.md).

The following section describes how to use ossfs by using the EcsRamRoleOssTest instance RAM role:

1.  Create an instance RAM role named EcsRamRoleOssTest.

    For more information about how to create an instance RAM role, see [Step 1: Create an instance RAM role](/intl.en-US/Security/Instance RAM roles/Bind an instance RAM role.md).

2.  Grant a RAM role permissions to access OSS resources.

    For more information about how to grant permissions to an instance RAM role, see [Grant permissions to a RAM role](/intl.en-US/RAM Role Management/Grant permissions to a RAM role.md). In this example, the instance RAM role is granted the AliyunOSSReadOnlyAccess permission. You can customize the permission. For more information, see [Create a custom policy](/intl.en-US/Policy Management/Custom policies/Create a custom policy.md).

3.  Bind an ECS instance to the instance RAM role.

    For more information, see [Step 3: Bind the instance RAM role](/intl.en-US/Security/Instance RAM roles/Bind an instance RAM role.md).

4.  Use ossfs based on the instance metadata URL.

    1.  Log on to the ECS instance.

    2.  Use ossfs. Add the -oram\_role option.

        The bucket named Bucket1 in the China \(Hangzhou\) region is used in the example:

        ```
        ossfs bucket1 /tmp/ossfs -ourl=http://oss-cn-hangzhou.aliyuncs.com -oram_role=http://100.100.100.200/latest/meta-data/ram/security-credentials/EcsRamRoleOssTest
        ```


## Configure access permissions

By default, the directory to which ossfs attaches files can be accessed only by the owner \(the user who performs the mount operation\) of the mount point. To modify the default permission settings to allow other users or user groups to access the mount point, you can use the following options while you run ossfs:

-   allow\_other: authorizes other users to access the directory to which the bucket is attached, but not objects in the folder. To modify the access permission on the objects in the directory, you must use the chmod command. No value is available for this option. To grant permissions to other users, use the -oallow\_other option.
-   uid: specifies the user ID \(UID\) of the owner of a folder.
-   gid: specifies the group ID \(GID\) of the owner of a folder.
-   mp\_umask: specifies the permission mask set for the mount point. This option takes effect only when the allow\_other option is set. Default value: 000. This option is used in the same way as the umask command. For example, you can use the -oallow\_other -omp\_umask=007 option to set the permission of the mount point to 770, and use the -oallow\_other -omp\_umask=077 option to set the permission of the mount point to 700.

Configuration examples:

-   Set the permission to 777 to allow access from all users

    ```
    ossfs bucket_name mount_point -ourl=endpoint -oallow_other
    ```

-   Set the permission to 770 to allow access from users only in the same group.

    ```
    ossfs bucket_name mount_point -ourl=endpoint -oallow_other -omp_umask=007
    ```

-   When you attach the bucket, specify the user and user group, and then set the permission to 770 to allow access from users only in the same group

    The user www is used in the example. You can run the id command to obtain the UID or GID of the user, and then specify the uid or gid parameter after you attach the bucket.

    ```
    id www
    uid=1000(www) gid=1000(web) groups=1000(web)
    ossfs bucket_name mount_point -ourl=endpoint -oallow_other -ouid=1000 -ogid=1000 -omp_umask=007
    ```


## Attach a specific folder

You can use ossfs to attach a specific folder in a bucket to the local file system by specifying a prefix. Command syntax:

```
ossfs bucket:/prefix mount_point -ourl=endpoint
```

Ensure that an object named $\{prefix\}/ exists in the bucket. You can run the [stat](/intl.en-US/Tools/ossutil/Common commands/stat.md) command of ossutil to check whether the object exists.

Example: Attach the folder folder in the bucket-ossfs-test bucket in the China \(Hangzhou\) region to /tmp/ossfs-folder.

```
ossfs bucket-ossfs-test:/folder /tmp/ossfs-folder -ourl=http://oss-cn-hangzhou.aliyuncs.com
```

## Attach a folder on startup

1.  Write the bucket name, AccessKey ID, and AccessKey secret to the /etc/passwd-ossfs file, and change the permission to 640.

    For more information, see [Quick installation](/intl.en-US/Tools/ossfs/Quick installation.md).

2.  Enable automatic mount on startup.

    The method to enable automatic mount on startup varies with the version of your operating system.

    -   Enable automatic mount on startup by using the fstab file for Ubuntu 14.04 and CentOS 6.5
        1.  Add the following command to the /etc/fstab file:

            ```
            ossfs#bucket_name mount_point fuse _netdev,url=url,allow_other 0 0
            ```

        2.  Save the /etc/fstab file. Run the mount -a command. The settings are correct if no errors are reported.
        3.  After you complete the preceding operations, automatic mount on startup is enabled in Ubuntu 14.04. For CentOS 6.5, you must also run the following command:

            ```
            chkconfig netfs on
            ```

    -   Enable automatic mount on startup by using the script for CentOS 7.0 or later
        1.  Create the ossfs file in the /etc/init.d/ directory, and copy the content of the [template](https://github.com/aliyun/ossfs/blob/master/scripts/automount.template) to this file. Replace your\_xxx with actual conditions.
        2.  Run the following command to allow the ossfs script to be executed:

            ```
            chmod a+x /etc/init.d/ossfs
            ```

            After the preceding command is run, you can execute the script. If the content of the script is correct, the OSS bucket is attached to the specified directory.

        3.  Run the following command to start the ossfs script as a service on startup:

            ```
            chkconfig ossfs on
            ```

        4.  After you complete the preceding operations, automatic mount on startup is enabled.

## Start ossfs by using Supervisor

Supervisor is a universal process management program of Python. Supervisor can turn a general command line process into a background daemon and monitor the process. Supervisor automatically restarts the process when the process stops unexpectedly. Perform the following steps to start ossfs by using Supervisor:

1.  Install Supervisor.

    -   CentOS

        ```
        yum install supervisor
        ```

    -   Ubuntu

        ```
        sudo apt-get install supervisor
        ```

2.  Create an ossfs startup script.

    1.  Create the start\_ossfs.sh file.

        ```
        mkdir /root/ossfs_scripts
        vi /root/ossfs_scripts/start_ossfs.sh
        ```

    2.  Write the startup script.

        ```
        # Remove the mount point.
        fusermount -u /mnt/ossfs
        # Attach the OSS bucket again. You must use the -f parameter to run ossfs on the frontend.
        exec ossfs bucket_name mount_point -ourl=endpoint -f
        ```

3.  Edit the /etc/supervisor/supervisord.conf file. Add the following content to the end of the file:

    ```
    [program:ossfs]
    command=bash /root/ossfs_scripts/start_ossfs.sh
    logfile=/var/log/ossfs.log
    log_stdout=true
    log_stderr=true
    logfile_maxbytes=1MB
    logfile_backups=10
    ```

4.  Run Supervisor.

    ```
    supervisord
    ```

5.  Verify whether Supervisor runs properly.

    ```
    ps aux | grep supervisor # View the Supervisor process.
    ps aux | grep ossfs # View the ossfs process.
    kill -9 ossfs # Terminate the ossfs process. Supervisor restarts ossfs. Do not use the killall command because this command sends the SIGTERM signal to terminate the ossfs process. In this case, Supervisor will not run ossfs again.
    ps aux | grep ossfs # View the ossfs process.
    ```


## Enable debug logging

You may encounter problems when you use ossfs. In this case, you must enable the debug logging feature to analyze and locate the problems based on the logs. You can enable debug logging by using one of the following methods:

-   Use the -d -odbglevel=debug -ocurldbg option when you attach a folder. ossfs stores the logs in the system logs.
    -   CentOS

        Logs are stored in /var/log/messages.

    -   Ubuntu

        Logs are stored in /var/log/syslog.

-   Use the -d -odbglevel=debug -ocurldbg -f option when you attach a directory. ossfs displays the logs on the frontend.

