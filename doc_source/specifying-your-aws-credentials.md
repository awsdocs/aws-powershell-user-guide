# Using AWS Credentials<a name="specifying-your-aws-credentials"></a>

Each AWS Tools for PowerShell command must include a set of AWS credentials, which are used to cryptographically sign the corresponding web service request\. You can specify credentials per command, per session, or for all sessions\. 

As a best practice, to avoid exposing your credentials, do not put literal credentials in a command\. Instead, create a profile for each set of credentials that you want to use, and store the profile in either of two credential stores\. Specify the correct profile by name in your command, and the AWS Tools for PowerShell retrieves the associated credentials\. For a general discussion of how to safely manage AWS credentials, see [Best Practices for Managing AWS Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html) in the *Amazon Web Services General Reference*\.

**Note**  
You need an AWS account to get credentials and use the AWS Tools for PowerShell\. For information about how to sign up for an account, see [AWS Account and Access Keys](pstools-appendix-sign-up.md)\.

**Topics**
+ [Credentials Store Locations](#specifying-your-aws-credentials-store)
+ [Managing Profiles](#managing-profiles)
+ [Specifying Credentials](#specifying-your-aws-credentials-use)
+ [Credentials Search Order](#pstools-cred-provider-chain)
+ [Credential Handling in AWS Tools for PowerShell Core](#credential-handling-in-aws-tools-for-powershell-core)

## Credentials Store Locations<a name="specifying-your-aws-credentials-store"></a>

The AWS Tools for PowerShell can use either of two credentials stores:
+ The AWS SDK store, which encrypts your credentials and stores them in your home folder\. In Windows, this store is located at: `C:\Users\username\AppData\Local\AWSToolkit\RegisteredAccounts.json`\.

  The [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/) and [Toolkit for Visual Studio](https://aws.amazon.com/visualstudio/) can also use the AWS SDK store\.
+ The shared credentials file, which is also located in your home folder, but stores credentials as plain text\.

  By default, the credentials file is stored here:
  + On Windows: `C:\Users\username\.aws\credentials`
  + On Mac/Linux: `~/.aws/credentials` 

  The AWS SDKs and the AWS Command Line Interface can also use the credentials file\. If you're running a script outside of your AWS user context, be sure that the file that contains your credentials is copied to a location where all user accounts \(local system and user\) can access your credentials\.

## Managing Profiles<a name="managing-profiles"></a>

Profiles enable you to reference different sets of credentials with AWS Tools for PowerShell\. You can use AWS Tools for PowerShell cmdlets to manage your profiles in the AWS SDK store\. You can also manage profiles in the AWS SDK store by using the [Toolkit for Visual Studio](https://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html) or programmatically by using the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\. For directions about how to manage profiles in the credentials file, see [Best Practices for Managing AWS Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html)\.

### Add a New profile<a name="add-a-new-profile"></a>

To add a new profile to the AWS SDK store, run the command `Set-AWSCredential`\. It stores your access key and secret key in your default credentials file under the profile name you specify\.

```
PS > Set-AWSCredential `
                 -AccessKey AKIA0123456787EXAMPLE `
                 -SecretKey wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY `
                 -StoreAs MyNewProfile
```
+  `-AccessKey`– The access key ID\.
+  `-SecretKey`– The secret key\.
+  `-StoreAs`– The profile name, which must be unique\. To specify the default profile, use the name `default`\.

### Update a Profile<a name="update-a-profile"></a>

The AWS SDK store must be maintained manually\. If you later change credentials on the service—for example, by using the [IAM console](https://console.aws.amazon.com/iam/home)—running a command with the locally stored credentials fails with the following error message:

```
The AWS Access Key Id you provided does not exist in our records.
```

You can update a profile by repeating the `Set-AWSCredential` command for the profile, and passing it the new access and secret keys\.

### List Profiles<a name="list-profiles"></a>

You can check the current list of names with the following command\. In this example, a user named Shirley has access to three profiles that are all stored in the shared credentials file \(`~/.aws/credentials`\)\.

```
PS > Get-AWSCredential -ListProfileDetail

ProfileName  StoreTypeName         ProfileLocation
-----------  -------------         ---------------
default      SharedCredentialsFile /Users/shirley/.aws/credentials
production   SharedCredentialsFile /Users/shirley/.aws/credentials
test         SharedCredentialsFile /Users/shirley/.aws/credentials
```

### Remove a Profile<a name="remove-a-profile"></a>

To remove a profile that you no longer require, use the following command\.

```
PS > Remove-AWSCredentialProfile -ProfileName an-old-profile-I-do-not-need
```

The `-ProfileName` parameter specifies the profile that you want to delete\.

The deprecated command [Clear\-AWSCredential](https://docs.aws.amazon.com/powershell/latest/reference/items/Clear-AWSCredential.html) is still available for backward compatibility, but `Remove-AWSCredentialProfile` is preferred\.

## Specifying Credentials<a name="specifying-your-aws-credentials-use"></a>

There are several ways to specify credentials\. The preferred way is to identify a profile instead of incorporating literal credentials into your command line\. AWS Tools for PowerShell locates the profile using a search order that is described in [Credentials Search Order](#pstools-cred-provider-chain)\.

On Windows, AWS credentials stored in the AWS SDK store are encrypted with the logged\-in Windows user identity\. They cannot be decrypted by using another account, or used on a device that's different from the one on which they were originally created\. To perform tasks that require the credentials of another user, such as a user account under which a scheduled task will run, set up a credential profile, as described in the preceding section, that you can use when you log in to the computer as that user\. Log in as the task\-performing user to complete the credential setup steps, and create a profile that works for that user\. Then log out and log in again with your own credentials to set up the scheduled task\.

**Note**  
Use the `-ProfileName` common parameter to specify a profile\. This parameter is equivalent to the `-StoredCredentials` parameter in earlier AWS Tools for PowerShell releases\. For backward compatibility, `-StoredCredentials` is still supported\.

### Default Profile \(Recommended\)<a name="default-profile-recommended"></a>

All AWS SDKs and management tools can find your credentials automatically on your local computer if the credentials are stored in a profile named `default`\. For example, if you have a profile named `default` on the local computer, you don't have to run either the `Initialize-AWSDefaultConfiguration` cmdlet or the `Set-AWSCredential` cmdlet\. The tools automatically use the access and secret key data stored in that profile\. To use an AWS Region other than your default Region \(the results of `Get-DefaultAWSRegion`\), you can run `Set-DefaultAWSRegion` and specify a Region\.

If your profile is not named `default`, but you want to use it as the default profile for the current session, run `Set-AWSCredential` to set it as the default profile\.

Although running `Initialize-AWSDefaultConfiguration` lets you specify a default profile for every PowerShell session, the cmdlet loads credentials from your custom\-named profile, but overwrites the `default` profile with the named profile\.

We recommend that you do not run `Initialize-AWSDefaultConfiguration` unless you are running a PowerShell session on an Amazon EC2 instance that was not launched with an instance profile, and you want to set up the credential profile manually\. Note that the credential profile in this scenario would not contain credentials\. The credential profile that results from running `Initialize-AWSDefaultConfiguration` on an EC2 instance doesn't directly store credentials, but instead points to instance metadata \(that provides temporary credentials that automatically rotate\)\. However, it does store the instance's Region\. Another scenario that might require running `Initialize-AWSDefaultConfiguration` occurs if you want to run a call against a Region other than the Region in which the instance is running\. Running that command permanently overrides the Region stored in the instance metadata\.

```
PS > Initialize-AWSDefaultConfiguration -ProfileName MyProfileName -Region us-west-2
```

**Note**  
The default credentials are included in the AWS SDK store under the `default` profile name\. The command overwrites any existing profile with that name\.

If your EC2 instance was launched with an instance profile, PowerShell automatically gets the AWS credentials and Region information from the instance profile\. You don't need to run `Initialize-AWSDefaultConfiguration`\. Running the `Initialize-AWSDefaultConfiguration` cmdlet on an EC2 instance launched with an instance profile isn't necessary, because it uses the same instance profile data that PowerShell already uses by default\.

### Session Profile<a name="session-profile"></a>

Use `Set-AWSCredential` to specify a default profile for a particular session\. This profile overrides any default profile for the duration of the session\. We recommend this if you want to use a custom\-named profile in your session instead of the current `default` profile\.

```
PS > Set-AWSCredential -ProfileName MyProfileName
```

**Note**  
In versions of the Tools for Windows PowerShell that are earlier than 1\.1, the `Set-AWSCredential` cmdlet did not work correctly, and would overwrite the profile specified by "MyProfileName"\. We recommend using a more recent version of the Tools for Windows PowerShell\.

### Command Profile<a name="command-profile"></a>

On individual commands, you can add the `-ProfileName` parameter to specify a profile that applies to only that one command\. This profile overrides any default or session profiles, as shown in the following example\.

```
PS > Get-EC2Instance -ProfileName MyProfileName
```

**Note**  
When you specify a default or session profile, you can also add a `-Region` parameter to override a default or session Region\. For more information, see [Specifying AWS Regions](pstools-installing-specifying-region.md)\. The following example specifies a default profile and Region\.  

```
PS > Initialize-AWSDefaultConfiguration -ProfileName MyProfileName -Region us-west-2
```

By default, the AWS shared credentials file is assumed to be in the user's home folder \(`C:\Users\username\.aws` on Windows, or `~/.aws` on Linux\)\. To specify a credentials file in a different location, include the `-ProfileLocation` parameter and specify the credentials file path\. The following example specifies a non\-default credentials file for a specific command\.

```
PS > Get-EC2Instance -ProfileName MyProfileName -ProfileLocation C:\aws_service_credentials\credentials
```

**Note**  
If you are running a PowerShell script during a time that you are not normally signed in to AWS—for example, you are running a PowerShell script as a scheduled task outside of your normal work hours—add the `-ProfileLocation` parameter when you specify the profile that you want to use, and set the value to the path of the file that stores your credentials\. To be certain that your AWS Tools for PowerShell script runs with the correct account credentials, you should add the `-ProfileLocation` parameter whenever your script runs in a context or process that does not use an AWS account\. You can also copy your credentials file to a location that is accessible to the local system or other account that your scripts use to perform tasks\.

## Credentials Search Order<a name="pstools-cred-provider-chain"></a>

When you run a command, AWS Tools for PowerShell searches for credentials in the following order\. It stops when it finds usable credentials\.

1. Literal credentials that are embedded as parameters in the command line\.

   We strongly recommend using profiles instead of putting literal credentials in your command lines\.

1. A specified profile name or profile location\.
   + If you specify only a profile name, the command looks for the specified profile in the AWS SDK store and, if that does not exist, the specified profile from the AWS shared credentials file in the default location\.
   + If you specify only a profile location, the command looks for the `default` profile from that credentials file\.
   + If you specify both a name and a location, the command looks for the specified profile in that credentials file\.

   If the specified profile or location is not found, the command throws an exception\. Search proceeds to the following steps only if you did not specify a profile or location\.

1. Credentials specified by the `-Credential` parameter\.

1. The session profile, if one exists\.

1. The default profile, in the following order:

   1. The `default` profile in the AWS SDK store\.

   1. The `default` profile in the AWS shared credentials file\.

   1. The `AWS PS Default` profile in the AWS SDK store\.

1. If the command is running on an Amazon EC2 instance that is configured to use an IAM role, the EC2 instance's temporary credentials accessed from the instance profile\.

   For more information about using IAM roles for Amazon EC2 instances, see the [AWS SDK for \.NET](https://aws.amazon.com/sdk-for-net/)\.

If this search fails to locate the specified credentials, the command throws an exception\.

## Credential Handling in AWS Tools for PowerShell Core<a name="credential-handling-in-aws-tools-for-powershell-core"></a>

Cmdlets in AWS Tools for PowerShell Core accept AWS access and secret keys or the names of credential profiles when they run, similarly to the AWS Tools for Windows PowerShell\. When they run on Windows, both modules have access to the AWS SDK for \.NET credential store file \(stored in the per\-user `AppData\Local\AWSToolkit\RegisteredAccounts.json` file\)\. 

This file stores your keys in encrypted format, and cannot be used on a different computer\. It is the first file that the AWS Tools for PowerShell searches for a credential profile, and is also the file where the AWS Tools for PowerShell stores credential profiles\. For more information about the AWS SDK for \.NET credential store file, see [Configuring AWS Credentials](https://docs.aws.amazon.com/sdk-for-net/v3/ndg/net-dg-config-creds.html)\. The Tools for Windows PowerShell module does not currently support writing credentials to other files or locations\.

Both modules can read profiles from the AWS shared credentials file that is used by other AWS SDKs and the AWS CLI\. On Windows, the default location for this file is `C:\Users\<userid>\.aws\credentials`\. On non\-Windows platforms, this file is stored at `~/.aws/credentials`\. The `-ProfileLocation` parameter can be used to point to a non\-default file name or file location\.

The SDK credential store holds your credentials in encrypted form by using Windows cryptographic APIs\. These APIs are not available on other platforms, so the AWS Tools for PowerShell Core module uses the AWS shared credentials file exclusively, and supports writing new credential profiles to the shared credential file\.

The following example scripts that use the `Set-AWSCredential` cmdlet show the options for handling credential profiles on Windows with either the **AWSPowerShell** or **AWSPowerShell\.NetCore** modules\.

```
# Writes a new (or updates existing) profile with name "myProfileName"
# in the encrypted SDK store file

Set-AWSCredential -AccessKey akey -SecretKey skey -StoreAs myProfileName

# Checks the encrypted SDK credential store for the profile and then
# falls back to the shared credentials file in the default location

Set-AWSCredential -ProfileName myProfileName

# Bypasses the encrypted SDK credential store and attempts to load the
# profile from the ini-format credentials file "mycredentials" in the
# folder C:\MyCustomPath

Set-AWSCredential -ProfileName myProfileName -ProfileLocation C:\MyCustomPath\mycredentials
```

The following examples show the behavior of the **AWSPowerShell\.NetCore** module on the Linux or macOS operating systems\.

```
# Writes a new (or updates existing) profile with name "myProfileName"
# in the default shared credentials file ~/.aws/credentials

Set-AWSCredential -AccessKey akey -SecretKey skey -StoreAs myProfileName

# Writes a new (or updates existing) profile with name "myProfileName"
# into an ini-format credentials file "~/mycustompath/mycredentials"

Set-AWSCredential -AccessKey akey -SecretKey skey -StoreAs myProfileName -ProfileLocation ~/mycustompath/mycredentials

# Reads the default shared credential file looking for the profile "myProfileName"

Set-AWSCredential -ProfileName myProfileName

# Reads the specified credential file looking for the profile "myProfileName"

Set-AWSCredential -ProfileName myProfileName -ProfileLocation ~/mycustompath/mycredentials
```