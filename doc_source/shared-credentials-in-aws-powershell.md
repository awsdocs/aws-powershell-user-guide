# Shared Credentials in AWS Tools for PowerShell<a name="shared-credentials-in-aws-powershell"></a>

The Tools for Windows PowerShell support the use of the AWS shared credentials file, similarly to the AWS CLI and other AWS SDKs\. The Tools for Windows PowerShell now support reading and writing of `basic`, `session`, and `assume role` credential profiles to both the \.NET credentials file and the AWS shared credential file\. This functionality is enabled by a new `Amazon.Runtime.CredentialManagement` namespace\.

The new profile types and access to the AWS shared credential file are supported by the following parameters that have been added to the credentials\-related cmdlets, [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html), [New\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/New-AWSCredential.html), and [Set\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/Set-AWSCredential.html)\. In service cmdlets, you can refer to your profiles by adding the common parameter, `-ProfileName`\.

## Using an IAM Role with AWS Tools for PowerShell<a name="shared-credentials-assume-role"></a>

The AWS shared credential file enables additional types of access\. For example, you can access your AWS resources by using an IAM role instead of the long term credentials of an IAM user\. To do this, you must have a standard profile that has permissions to assume the role\. When you tell the AWS Tools for PowerShell to use a profile that specified a role, the AWS Tools for PowerShell looks up the profile identified by the `SourceProfile` parameter\. Those credentials are used to request temporary credentials for the role specified by the `RoleArn` parameter\. You can optionally require the use of an multi\-factor authentication \(MFA\) device or an `ExternalId` code when the role is assumed by a third party\.


****  

| Parameter Name | Description | 
| --- | --- | 
|  ExternalId  |  The user\-defined external ID to be used when assuming a role, if required by the role\. This is typically only required when you delegate access to your account to a third party\. The third party must include the ExternalId as a parameter when assuming the assigned role\. For more information, see [How to Use an External ID When Granting Access to Your AWS Resources to a Third Party](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-user_externalid.html) in the *IAM User Guide*\.  | 
|  MfaSerial  |  The MFA serial number to be used when assuming a role, if required by the role\. For more information, see [Using Multi\-Factor Authentication \(MFA\) in AWS](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_mfa.html) in the *IAM User Guide*\.  | 
|  RoleArn  |  The ARN of the role to assume for assume role credentials\. For more information about creating and using roles, see [IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) in the *IAM User Guide*\.  | 
|  SourceProfile  |  The name of the source profile to be used by assume role credentials\. The credentials found in this profile are used to assume the role specified by the `RoleArn` parameter\.  | 

### Setup of profiles for assuming a role<a name="setup"></a>

The following is an example showing how to set up a source profile that enables directly assuming an IAM role\. 

The first command creates a source profile that is referenced by the role profile\. The second command creates the role profile that which role to assume\. The third command shows the credentials for the role profile\.

```
PS > Set-AWSCredential -StoreAs my_source_profile -AccessKey access_key_id -SecretKey secret_key
PS > Set-AWSCredential -StoreAs my_role_profile -SourceProfile my_source_profile -RoleArn arn:aws:iam::123456789012:role/role-i-want-to-assume
PS > Get-AWSCredential -ProfileName my_role_profile

SourceCredentials                  RoleArn                                              RoleSessionName                           Options
-----------------                  -------                                              ---------------                           -------
Amazon.Runtime.BasicAWSCredentials arn:aws:iam::123456789012:role/role-i-want-to-assume aws-dotnet-sdk-session-636238288466144357 Amazon.Runtime.AssumeRoleAWSCredentialsOptions
```

To use this role profile with the Tools for Windows PowerShell service cmdlets, add the `-ProfileName` common parameter to the command to reference the role profile\. The following example uses the role profile defined in the previous example to access the [Get\-S3Bucket](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-S3Bucket.html) cmdlet\. AWS Tools for PowerShell looks up the credentials in `my_source_profile`, uses those credentials to call `AssumeRole` on behalf of the user, and then uses those temporary role credentials to call `Get-S3Bucket`\.

```
PS > Get-S3Bucket -ProfileName my_role_profile

CreationDate           BucketName
------------           ----------
2/27/2017 8:57:53 AM   4ba3578c-f88f-4d8b-b95f-92a8858dac58-bucket1
2/27/2017 10:44:37 AM  2091a504-66a9-4d69-8981-aaef812a02c3-bucket2
```

## Using the Credential Profile Types<a name="using-the-credential-profile-types"></a>

To set a credential profile type, understand which parameters provide the information required by the profile type\.


****  

| Credentials Type | Parameters you must use | 
| --- | --- | 
|  **Basic** These are the long term credentials for an IAM user  |  `-AccessKey`  `-SecretKey`  | 
|  **Session**: These are the short term credentials for an IAM role that you retrieve manually, such as by directly calling the [Use\-STSRole](&url-twp-ref;items/Use-STSRole.html) cmdlet\.  |  `-AccessKey`  `-SecretKey` `-SessionToken`  | 
|  **Role**: These are are short term credentials for an IAM role that AWS Tools for PowerShell retrieve for you\.  |  `-SourceProfile` `-RoleArn`  optional: `-ExternalId` optional: `-MfaSerial`  | 

## The `ProfilesLocation` Common Parameter<a name="the-profileslocation-common-parameter"></a>

You can use `-ProfileLocation` to write to the shared credential file as well as instruct a cmdlet to read from the credential file\. Adding the `-ProfileLocation` parameter controls whether Tools for Windows PowerShell uses the shared credential file or the \.NET credential file\. The following table describes how the parameter works in Tools for Windows PowerShell\.


****  

| Profile Location Value | Profile Resolution Behavior | 
| --- | --- | 
|  null \(not set\) or empty  |  First, search the \.NET credential file for a profile with the specified name\. If the profile isn't found, search the AWS shared credentials file at `(user's home directory)\.aws\credentials`\.  | 
|  The path to a file in the AWS shared credential file format  |  Search only the specified file for a profile with the given name\.  | 

### Save Credentials to a Credentials File<a name="save-credentials-to-a-credentials-file"></a>

To write and save credentials to one of the two credential files, run the `Set-AWSCredential` cmdlet\. The following example shows how to do this\. The first command uses `Set-AWSCredential` with `-ProfileLocation` to add access and secret keys to a profile specified by the `-ProfileName` parameter\. In the second line, run the [Get\-Content](https://msdn.microsoft.com/en-us/powershell/reference/5.0/microsoft.powershell.management/get-content) cmdlet to display the contents of the credentials file\.

```
PS > Set-AWSCredential -ProfileLocation C:\Users\auser\.aws\credentials -ProfileName basic_profile -AccessKey access_key2 -SecretKey secret_key2
PS > Get-Content C:\Users\auser\.aws\credentials

aws_access_key_id=access_key2
aws_secret_access_key=secret_key2
```

## Displaying Your Credential Profiles<a name="showing-credential-profiles"></a>

Run the [Get\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSCredential.html) cmdlet and add the `-ListProfileDetail` parameter to return credential file types and locations, and a list of profile names\.

```
PS > Get-AWSCredential -ListProfileDetail

ProfileName                     StoreTypeName         ProfileLocation
-----------                     -------------         ---------------
source_profile                  NetSDKCredentialsFile
assume_role_profile             NetSDKCredentialsFile
basic_profile                   SharedCredentialsFile C:\Users\auser\.aws\credentials
```

## Removing Credential Profiles<a name="removing-credential-profiles"></a>

To remove credential profiles, run the new [Remove\-AWSCredentialProfile](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-AWSCredentialProfile.html) cmdlet\. [Clear\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/Clear-AWSCredential.html) is deprecated, but still available for backward compatibility\.

## Important Notes<a name="important-notes"></a>

Only [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html), [New\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/New-AWSCredential.html), and [Set\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/Set-AWSCredential.html) support the parameters for role profiles\. You cannot specify the role parameters directly on a command such as `Get-S3Bucket -SourceProfile source_profile_name -RoleArn arn:aws:iam::999999999999:role/role_name`\. That does not work because service cmdlets do not directly support the `SourceProfile` or `RoleArn` parameters\. Instead, you must store those parameters in a profile, then call the command with the `-ProfileName` parameter\.