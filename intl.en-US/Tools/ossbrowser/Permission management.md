# Permission management

This topic describes how to perform simple permission management by using ossbrowser.

## Log on to ossbrowser as a RAM user

For data security, we recommend that you use the AccessKey pair of a RAM user to log on to ossbrowser.

**Note:** For more information about how to create a RAM user and AccessKey pair, see [Create a RAM user](/intl.en-US/RAM User Management/Create a RAM user.md).

RAM users can be classified into the following types based on their permissions:

-   Administrator RAM user: a RAM user who has administrative permissions. For example, a RAM user who can manage all buckets and authorize other RAM users is an administrator RAM user. You can log on to the RAM console by using your Alibaba Cloud account to create an administrator RAM user and grant permissions to the user, as shown in the following figure.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0914459951/p6324.png)

-   Operator RAM user: a RAM user who has the read-only permission on a bucket or folder. Administrator RAM users can use the simple policy feature to grant RAM users permissions. For more information, see [Grant permissions by using a simple policy](#section_rc9_ydw_85t).

    **Note:** You can grant fine-grained permissions to RAM users. For more information, see [Implement access control based on RAM policies](/intl.en-US/Developer Guide/Data security/Access and control/RAM Policy/Implement access control based on RAM policies.md).


## Log on to ossbrowser by using STS tokens

You can use an STS token to log on to ossbrowser. STS tokens can be provided for other authorized users for temporary access to a folder in your bucket. The STS token automatically becomes invalid after it expires.

1.  Log on to ossbrowser as an administrator RAM user.

    **Note:** When you log on to ossbrowser by using your Alibaba Cloud account or as an administrator RAM user, part of the features are inaccessible to ensure data security. Use the AccessKey pair of an administrator RAM user to log on to ossbrowser and generate a token. The administrator RAM user must have the permissions to manage a bucket or folder, manage RAM \(AliyunRAMFullAccess\), and call the STS AssumeRole operation \(AliyunSTSAssumeRoleAccess\).

2.  Select the objects or folders to be temporarily accessed by the authorized users, and choose **More** \> **Authorization Token**, as shown in the following figure.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0914459951/p6326.png)

3.  Save the obtained token.

4.  Log off from ossbrowser and use the STS token to log on, as shown in the following figure.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0914459951/p6327.png)


## Grant permissions by using a simple policy

After you log on to ossbrowser as an administrator RAM user, you can use the **Simplify Policy** feature to create an operator RAM user, or grant an operator RAM user the read-only or read/write permissions on a bucket or directory.

**Note:** The simple policy feature of ossbrowser is designed based on Alibaba Cloud RAM to control access. You can log on to the RAM console from the Alibaba Cloud website to manage your RAM users more precisely.

1.  Log on to ossbrowser as an administrator RAM user.

2.  Select one or more objects or folders to be temporarily accessed by the authorized users, and choose **More** \> **Simple Policy**.

3.  In the **Simplify policy authorization** dialog box, set Privileges.

4.  Grant permissions to an existing operator RAM user or create a new operator RAM user in this dialog box.

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0914459951/p6329.png)

    You can view, copy, and use the generated policy text. For example, you can copy the policy text and use it to edit the authorization policies for RAM users and roles in the RAM console.

    **Note:** To use the simple policy feature, you must log on to ossbrowser by using the AccessKey pair of a RAM user who has the RAM configuration permissions. For example, use the AccessKey pair of an administrator RAM user who has the RAM configuration permissions.


