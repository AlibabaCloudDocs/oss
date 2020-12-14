# Tutorial: Authorize a RAM user of another Alibaba Cloud account by creating a RAM role

By default, OSS resources can be accessed only by their owners. To authorize another user to access your OSS resources, you can grant permissions to the user by creating a RAM role.

Example: Company A wants to authorize Company B to access resources owned by Company A. However, Company A does not want to provide Company B with a RAM users' credentials. In this case, Company A can create a RAM role and grant the RAM role permissions to access the OSS resources of Company A. Company B can use a RAM user to assume the RAM role. This way, Company B can access the OSS resources owned by Company A.

## Step 1: Company A creates a RAM role and grant the RAM role permissions to access the OSS resources of Company A

Company A must create a RAM role that has permissions to access the OSS resources owned by Company A.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) as Company A.

2.  In the left-side navigation pane, click **RAM Roles**. On the RAM Roles page, click **Create RAM Role**.

3.  In the Create RAM Role pane, set Trusted entity type to **Alibaba Cloud Account** in the **Select Role Type** step. Click **Next**.

4.  Configure the following parameters in the **Configure Role** step:

    -   **RAM Role Name**: Enter a name for the RAM role. In this example, specify `admin-oss`.
    -   Note: optional. Enter a description for the RAM role. In this example, the field is left empty.
    -   **Select Trusted Alibaba Cloud Account**: Select **Other Alibaba Cloud Account** and enter the UID of an Alibaba Cloud account that belongs to Company B. In this example, specify `17464958576******`.
5.  Click **OK**.

6.  In the **Finish** step, click **Add Permissions to RAM Role**.

7.  In the Add Permissions pane, select **System Policy** in the Select Policy section. Find and click AliyunOSSReadOnlyAccess, which grants only the read permissions on OSS resources. The AliyunOSSReadOnlyAccess policy is displayed in the **Selected** section on the right side. Click **OK**.

    AliyunOSSReadOnlyAccess allows your customers to access all your buckets in OSS. You can customize a policy to grant permissions to read only a part of your buckets or folders. For more information, see [Implement access control based on RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Implement access control based on RAM policies.md).


If you want to specify that the RAM role can be assumed only by specified RAM users, you can modify the trusted entity of the RAM role. For more information, see [Edit the trust policy of a RAM role](/intl.en-US/RAM Role Management/Edit the trust policy of a RAM role.md).

## Step 2: Company B creates a RAM user and grants the RAM user permissions to assume RAM roles

Company B must create a RAM user that has permissions to assume RAM roles. Company B can use the RAM user to assume a RAM role from Company A.

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) as Company B.

2.  In the left-side navigation pane, choose **Identities** \> **Users**. On the Users page, click **Create User**.

3.  On the Create User page, enter a logon name in the **Logon Name** field. Enter a display name in the **Display Name** field. Select **Console Password Logon** as the access mode and configure the logon information.

4.  Click **OK**.

5.  In the **Users** list, select the user you create and click **Add Permissions**.

    Save RAM user information to prevent loss.

6.  In the Add Permissions pane, select **System Policy** in the Select Policy section. Find and click AliyunSTSAssumeRoleAccess, which grants the user permissions to call the AssumeRole operation in STS. The AliyunSTSAssumeRoleAccess policy is displayed in the **Selected** section on the right side. Click **OK**.


## Step 3: Company B uses the created RAM user to log on to the Alibaba Cloud Management console and assume the RAM role created by Company A

Company B uses the created RAM user to log on to the Alibaba Cloud Management console and switches the identity to the RAM role created by Company A.

1.  Log on to the Alibaba Cloud Management console as the RAM user of Company B. Switch [Log on to the console as a RAM user](/intl.en-US/RAM User Management/Log on to the console as a RAM user.md).

2.  Move the pointer over the profile picture in the upper-right corner of the console. Click **Switch Identity**.

3.  On the **Switch Role** page, enter the information about the RAM role and click **Switch**.

    Enter the following information for the RAM role:

    -   **Enterprise Alias/Default Domain Name**: Enter the alias or default domain name of Company A. For more information, see [Terms](/intl.en-US/Product Introduction/Terms.md).

        A default domain name is used in this example. Enter the default domain name `1178810717******.onaliyun.com`. `1178810717******` is the UID of an Alibaba Cloud account of Company A.

    -   **Role Name**: Enter `admin-oss`, which is the name of the RAM role created by Company A.
4.  Click [OSS console](https://oss.console.aliyun.com) to log on to the OSS console and manage the OSS resources owned by Company A.


## References

You can also authorize a RAM user of another Alibaba Cloud account by adding a bucket policy. For more information, see [Tutorial: Authorize a RAM user under another Alibaba Cloud account by adding a bucket policy](/intl.en-US/Developer Guide/Data security/Access and control/Bucket Policy/Tutorial: Authorize a RAM user under another Alibaba Cloud account by adding a bucket
         policy.md).

