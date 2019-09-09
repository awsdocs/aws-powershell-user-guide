# Set an IAM Policy for an IAM User<a name="pstools-iam-policy"></a>

The following commands show how to assign an IAM policy to an IAM user\. The policy specified below provides the user with "Power User Access"\. This policy is identical to the *Power User Access* policy template provided in the IAM console\. The name for the policy shown below follows the naming convention used for IAM policy templates such as the template for *Power User Access*\. The convention is

```
<template name>+<user name>+<date stamp>
```

In order to specify the policy document, we use a PowerShell here\-string\. We assign the contents of the here\-string to a variable and then use the variable as a parameter value in `Write-IAMUserPolicy`\.

```
PS > $policyDoc = @"
>> {
>>   "Version": "2012-10-17",
>>   "Statement": [
>>     {
>>       "Effect": "Allow",
>>       "NotAction": "iam:*",
>>       "Resource": "*"
>>     }
>>   ]
>> }
>> "@
>> 

PS > Write-IAMUserPolicy -UserName myNewUser -PolicyName "PowerUserAccess-myNewUser-201211201605" -PolicyDocument $policyDoc

ServiceResponse
---------------
Amazon.IdentityManagement.Model.PutUserPolicyResponse
```

## See Also<a name="pstools-seealso-iam-policy"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Using Windows PowerShell "Here\-Strings"](http://technet.microsoft.com/en-us/library/ee692792.aspx) 
+  [PutUserPolicy](https://docs.aws.amazon.com/IAM/latest/APIReference/API_PutUserPolicy.html) 