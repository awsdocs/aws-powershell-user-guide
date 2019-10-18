# Prerequisites for Setting up the AWS Tools for PowerShell<a name="pstools-getting-set-up-prereq"></a>

To use the AWS Tools for PowerShell, you must first complete the following steps\.

1\. Sign up for an AWS account\.  
If you don't have an AWS account, see the following topic for complete instructions on how to sign up:  
[https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

2\. Create an IAM user\.  
After you sign up for your account, you must create *users* in the AWS Identity and Access Management \(IAM\) service\. Each user has its own credentials and permissions\. The *credentials* are used to authenticate the user making a request\. The *permissions* determine which AWS resources and operations are authorized for that user\.  
Creating a user is outside the scope of this topic\. But if you're new to AWS, we recommend that you read the following:  
+ To understand user credentials and best practices for managing them, see [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in the *Amazon Web Services General Reference*\.
+ For a step\-by\-step tutorial on creating a user with "administrator" permissions that you can use to run AWS Tools for PowerShell commands, see [Creating Your First IAM Admin User and Group](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) in the *IAM User Guide*\.

3\. Create an access key for your IAM user\.  
The AWS Tools for PowerShell require that each cmdlet is sent using appropriate security credentials\. To do this, you typically must create an access key for each user that needs to use the AWS Tools for PowerShell cmdlets\. An access key consists of an *access key ID* and *secret access key*\. These are used to sign \(encrypt for the purpose of authentication\) programmatic requests that you make to AWS services\. If you don't have an access key, you can create it by using the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\. As described in [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html), we recommend that you use access keys for IAM users instead of AWS root account access keys\. IAM lets you securely control access to AWS services and resources in your AWS account\.   
As with any AWS operation, creating access keys requires that you have permissions to perform the related IAM actions\. For more information, see [Permissions for Administering IAM Identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_delegate-permissions.html) in the *IAM User Guide*\.  
After you create the access key for your first user in the AWS console, you can use that user and its access key to run AWS Tools for PowerShell cmdlets to create access keys for your other users\. The following example shows how to use the `New-IAMAccessKey` cmdlet to create an access key and secret key for an IAM user\.  

```
PS > New-IAMAccessKey -UserName alice

AccessKeyId     : AKIAIOSFODNN7EXAMPLE
CreateDate      : 9/4/19 12:46:18 PM
SecretAccessKey : wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Status          : Active
UserName        : alice
```
Save these credentials in a safe place\. You need them to configure the AWS Tools for PowerShell credentials file later\. For more information, see [Using AWS Credentials](specifying-your-aws-credentials.md)\.  
The only time you can see the secret access key \(the equivalent of a password\) is when you create the access key\. You cannot retrieve it later\. If you lose the secret key, you must delete the access key/secret key pair and recreate them\.
An IAM user can have only two access keys at any one time\. If you attempt to create a third set, the `New-IAMAccessKey` cmdlet returns an error\. To create another, you must first delete one of the existing two\.  
You can use the `Remove-IAMAccessKey` cmdlet to delete a set of credentials for an IAM user\. You must specify both the `UserName` and the `AccessKeyId`\.  

```
PS > Remove-IAMAccessKey -UserName alice -AccessKeyId -AccessKeyId AKIAIOSFODNN7EXAMPLE

Confirm
Are you sure you want to perform this action?
Performing the operation "Remove-IAMAccessKey (DeleteAccessKey)" on target "AKIAIOSFODNN7EXAMPLE".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): y
PS >
```