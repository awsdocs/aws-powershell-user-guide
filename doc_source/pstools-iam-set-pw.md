# Set an Initial Password for an IAM User<a name="pstools-iam-set-pw"></a>

The following example demonstrates how to use the `New-IAMLoginProfile` cmdlet to set an initial password for an IAM user\. For more information about character limits and recommendations for passwords, see [Password Policy Options](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords_account-policy.html#password-policy-details) in the *IAM User Guide*\.

```
PS > New-IAMLoginProfile -UserName myNewUser -Password "&!123!&"

UserName                                                    CreateDate
--------                                                    ----------
myNewUser                                                   11/20/2012 4:23:05 PM
```

Use the `Update-IAMLoginProfile` cmdlet to change the password for an IAM user\.

## See Also<a name="pstools-seealso-iam-pw"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Managing Passwords](https://docs.aws.amazon.com/IAM/latest/UserGuide/Using_ManagingLogins.html) 
+  [CreateLoginProfile](https://docs.aws.amazon.com/IAM/latest/UserGuide/API_CreateLoginProfile.html) 