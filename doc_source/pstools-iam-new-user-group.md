# Create New IAM Users and Groups<a name="pstools-iam-new-user-group"></a>

This section describes how to create a new IAM group and a new IAM user and then add the user to the group\.

First, use the `New-IAMGroup` cmdlet to create the group\. Although we've included it here, the `-Path` parameter is optional\.

```
PS > New-IAMGroup -Path "/ps-created-groups/" -GroupName "powerUsers"

Path       : /ps-created-groups/
GroupName  : powerUsers
GroupId    : AGPAJPHUEYD5XPCGIUH3E
Arn        : arn:aws:iam::455364113843:group/ps-created-groups/powerUsers
CreateDate : 11/20/2012 3:32:50 PM
```

Next, use the `New-IAMUser` cmdlet to create the user\. Similar to the preceding example, the `-Path` parameter is optional\.

```
PS > New-IAMUser -Path "/ps-created-users/" -UserName "myNewUser"

Path       : /ps-created-users/
UserName   : myNewUser
UserId     : AIDAJOJSPSPXADHBT7IN6
Arn        : arn:aws:iam::455364113843:user/ps-created-users/myNewUser
CreateDate : 11/20/2012 3:26:31 PM
```

Finally, use the `Add-IAMUserToGroup` cmdlet to add the user to the group\.

```
PS > Add-IAMUserToGroup -UserName myNewUser -GroupName powerUsers

ServiceResponse
---------------
Amazon.IdentityManagement.Model.AddUserToGroupResponse
```

To verify that the `powerUsers` group contains the `myNewUser`, use the `Get-IAMGroup` cmdlet\.

```
PS > Get-IAMGroup -GroupName powerUsers

Group                         Users                         IsTruncated                   Marker
-----                         -----                         -----------                   ------
Amazon.IdentityManagement.... {myNewUser}                   False
```

You can also view IAM users and groups with the AWS Management Console
+  [http://console\.aws\.amazon\.com/iam/home?\#s=Users](https://console.aws.amazon.com/iam/home?#s=Users) \[Users View\]
+  [http://console\.aws\.amazon\.com/iam/home?\#s=Groups](https://console.aws.amazon.com/iam/home?#s=Groups) \[Groups View\]

## See Also<a name="pstools-seealso-iam-users"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Adding a New User to Your AWS Account \(IAM User Guide\)](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_SettingUpUser.html) 
+  [CreateGroup \(IAM Service Reference\)](https://docs.aws.amazon.com/IAM/latest/APIReference/API_API_CreateGroup.html) 