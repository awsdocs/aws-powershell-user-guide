# Configuring Federated Identity with the AWS Tools for PowerShell<a name="saml-pst"></a>

To let users in your organization access AWS resources, you must configure a standard and repeatable authentication method for purposes of security, auditability, compliance, and the capability to support role and account separation\. Although it's common to provide users with the ability to access AWS APIs, without federated API access, you would also have to create AWS Identity and Access Management \(IAM\) users, which defeats the purpose of using federation\. This topic describes SAML \(Security Assertion Markup Language\) support in the AWS Tools for PowerShell that eases your federated access solution\.

SAML support in the AWS Tools for PowerShell lets you provide your users federated access to AWS services\. SAML is an XML\-based, open\-standard format for transmitting user authentication and authorization data between services; in particular, between an identity provider \(such as [Active Directory Federation Services](http://technet.microsoft.com/library/bb897402.aspx)\), and a service provider \(such as AWS\)\. For more information about SAML and how it works, see [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) on Wikipedia, or [SAML Technical Specifications](http://saml.xml.org/saml-specifications) at the Organization for the Advancement of Structured Information Standards \(OASIS\) website\. SAML support in the AWS Tools for PowerShell is compatible with SAML 2\.0\.

## Prerequisites<a name="saml-pst-prerequisites"></a>

You must have the following in place before you try to use SAML support for the first time\.
+ A federated identity solution that is correctly integrated with your AWS account for console access by using only your organizational credentials\. For more information about how to do this specifically for Active Directory Federation Services, see [About SAML 2\.0 Federation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html) in the *IAM User Guide*, and the blog post, [Enabling Federation to AWS Using Windows Active Directory, AD FS, and SAML 2\.0](http://aws.amazon.com/blogs/security/enabling-federation-to-aws-using-windows-active-directory-adfs-and-saml-2-0/)\. Although the blog post covers AD FS 2\.0, the steps are similar if you are running AD FS 3\.0\.
+ Version 3\.1\.31\.0 or newer of the AWS Tools for PowerShell installed on your local workstation\.

## How an Identity\-Federated User Gets Federated Access to AWS Service APIs<a name="saml-pst-federated-process"></a>

The following process describes, at a high level, how an Active Directory \(AD\) user is federated by AD FS to gain access to AWS resources\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/powershell_ADFSauth_using_vsd.png)

1. The client on federated user's computer authenticates against AD FS\.

1. If authentication succeeds, AD FS sends the user a SAML assertion\.

1. The user's client sends the SAML assertion to the AWS Security Token Service \(STS\) as part of a SAML federation request\.

1. STS returns a SAML response that contains AWS temporary credentials for a role the user can assume\.

1. The user accesses AWS service APIs by including those temporary credentials in request made by AWS Tools for PowerShell\.

## How SAML Support Works in the AWS Tools for PowerShell<a name="saml-pst-overview"></a>

This section describes how AWS Tools for PowerShell cmdlets enable configuration of SAML\-based identity federation for users\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/Powershell_SamlAuth_using_vsd.png)

1. AWS Tools for PowerShell authenticates against AD FS by using the Windows user's current credentials, or interactively, when the user tries to run a cmdlet that requires credentials to call into AWS\.

1. AD FS authenticates the user\.

1. AD FS generates a SAML 2\.0 authentication response that includes an assertion; the purpose of the assertion is to identify and provide information about the user\. AWS Tools for PowerShell extracts the list of the user's authorized roles from the SAML assertion\.

1. AWS Tools for PowerShell forwards the SAML request, including the requested role's Amazon Resource Names \(ARN\), to STS by making the `AssumeRoleWithSAMLRequest` API call\.

1. If the SAML request is valid, STS returns a response that contains the AWS `AccessKeyId`, `SecretAccessKey`, and `SessionToken`\. These credentials last for 3,600 seconds \(1 hour\)\.

1. The user now has valid credentials to work with any AWS service APIs that the user's role is authorized to access\. AWS Tools for PowerShell automatically applies these credentials for any subsequent AWS API calls, and renews them automatically when they expire\.
**Note**  
When the credentials expire, and new credentials are required, AWS Tools for PowerShell automatically reauthenticates with AD FS, and obtains new credentials for a subsequent hour\. For users of domain\-joined accounts, this process occurs silently\. For accounts that are not domain\-joined, AWS Tools for PowerShell prompts users to enter their credentials before they can reauthenticate\.

## How to Use the PowerShell SAML Configuration Cmdlets<a name="saml-pst-config-cmdlets"></a>

AWS Tools for PowerShell includes two new cmdlets that provide SAML support\.
+  `Set-AWSSamlEndpoint` configures your AD FS endpoint, assigns a friendly name to the endpoint, and optionally describes the authentication type of the endpoint\.
+  `Set-AWSSamlRoleProfile` creates or edits a user account profile that you want to associate with an AD FS endpoint, identified by specifying the friendly name you provided to the `Set-AWSSamlEndpoint` cmdlet\. Each role profile maps to a single role that a user is authorized to perform\.

  Just as with AWS credential profiles, you assign a friendly name to the role profile\. You can use the same friendly name with the `Set-AWSCredential` cmdlet, or as the value of the `-ProfileName` parameter for any cmdlet that invokes AWS service APIs\.

Open a new AWS Tools for PowerShell session\. If you are running PowerShell 3\.0 or newer, the AWS Tools for PowerShell module is automatically imported when you run any of its cmdlets\. If you are running PowerShell 2\.0, you must import the module manually by running the ``Import\-Module`` cmdlet, as shown in the following example\.

```
PS > Import-Module "C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowerShell\AWSPowerShell.psd1"
```

### How to Run the `Set-AWSSamlEndpoint` and `Set-AWSSamlRoleProfile` Cmdlets<a name="how-to-run-the-set-awssamlendpoint-and-set-awssamlroleprofile-cmdlets"></a>

1. First, configure the endpoint settings for the AD FS system\. The simplest way to do this is to store the endpoint in a variable, as shown in this step\. Be sure to replace the placeholder account IDs and AD FS host name with your own account IDs and AD FS host name\. Specify the AD FS host name in the `Endpoint` parameter\.

   ```
   PS > $endpoint = "https://adfs.example.com/adfs/ls/IdpInitiatedSignOn.aspx?loginToRp=urn:amazon:webservices"
   ```

1. To create the endpoint settings, run the `Set-AWSSamlEndpoint` cmdlet, specifying the correct value for the `AuthenticationType` parameter\. Valid values include `Basic`, `Digest`, `Kerberos`, `Negotiate`, and `NTLM`\. If you do not specify this parameter, the default value is `Kerberos`\.

   ```
   PS > $epName = Set-AWSSamlEndpoint -Endpoint $endpoint -StoreAs ADFS-Demo -AuthenticationType NTLM
   ```

   The cmdlet returns the friendly name you assigned by using the `-StoreAs` parameter, so you can use it when you run `Set-AWSSamlRoleProfile` in the next line\.

1. Now, run the `Set-AWSSamlRoleProfile` cmdlet to authenticate with the AD FS identity provider and get the set of roles \(in the SAML assertion\) that the user is authorized to perform\.

   The `Set-AWSSamlRoleProfile` cmdlet uses the returned set of roles to either prompt the user to select a role to associate with the specified profile, or validate that role data provided in parameters is present \(if not, the user is prompted to choose\)\. If the user is authorized for only one role, the cmdlet associates the role with the profile automatically, without prompting the user\. There is no need to provide a credential to set up a profile for domain\-joined usage\.

   ```
   PS > Set-AWSSamlRoleProfile -StoreAs SAMLDemoProfile -EndpointName $epName
   ```

   Alternatively, for non\-domain\-joined accounts, you can provide Active Directory credentials, and then select an AWS role to which the user has access, as shown in the following line\. This is useful if you have different Active Directory user accounts to differentiate roles within your organization \(for example, administration functions\)\.

   ```
   PS > $credential = Get-Credential -Message "Enter the domain credentials for the endpoint"
   PS > Set-AWSSamlRoleProfile -EndpointName $epName -NetworkCredential $credential -StoreAs SAMLDemoProfile
   ```

1. In either case, the `Set-AWSSamlRoleProfile` cmdlet prompts you to choose which role should be stored in the profile\. The following example shows two available roles: `ADFS-Dev`, and `ADFS-Production`\. The IAM roles are associated with your AD login credentials by the AD FS administrator\.

   ```
   Select Role
   Select the role to be assumed when this profile is active
   [1] 1 - ADFS-Dev  [2] 2 - ADFS-Production  [?] Help (default is "1"):
   ```

   Alternatively, you can specify a role without the prompt, by entering the `RoleARN`, `PrincipalARN`, and optional `NetworkCredential` parameters\. If the specified role is not listed in the assertion returned by authentication, the user is prompted to choose from available roles\.

   ```
   PS > $params = @{ "NetworkCredential"=$credential, "PrincipalARN"="{arn:aws:iam::012345678912:saml-provider/ADFS}", "RoleARN"="{arn:aws:iam::012345678912:role/ADFS-Dev}"
   }
   PS > $epName | Set-AWSSamlRoleProfile @params -StoreAs SAMLDemoProfile1 -Verbose
   ```

1. You can create profiles for all roles in a single command by adding the `StoreAllRoles` parameter, as shown in the following code\. Note that the role name is used as the profile name\.

   ```
   PS > Set-AWSSamlRoleProfile -EndpointName $epName -StoreAllRoles
   ADFS-Dev
   ADFS-Production
   ```

### How to Use Role Profiles to Run Cmdlets that Require AWS Credentials<a name="how-to-use-role-profiles-to-run-cmdlets-that-require-aws-credentials"></a>

To run cmdlets that require AWS credentials, you can use role profiles defined in the AWS shared credential file\. Provide the name of a role profile to `Set-AWSCredential` \(or as the value for any `ProfileName` parameter in the AWS Tools for PowerShell\) to get temporary AWS credentials automatically for the role that is described in the profile\.

Although you use only one role profile at a time, you can switch between profiles within a shell session\. The `Set-AWSCredential` cmdlet does not authenticate and get credentials when you run it by itself; the cmdlet records that you want to use a specified role profile\. Until you run a cmdlet that requires AWS credentials, no authentication or request for credentials occurs\.

You can now use the temporary AWS credentials that you obtained with the `SAMLDemoProfile` profile to work with AWS service APIs\. The following sections show examples of how to use role profiles\.

### Example 1: Set a Default Role with `Set-AWSCredential`<a name="example-1-set-a-default-role-with-set-awscredential"></a>

This example sets a default role for a AWS Tools for PowerShell session by using `Set-AWSCredential`\. Then, you can run cmdlets that require credentials, and are authorized by the specified role\. This example lists all Amazon Elastic Compute Cloud instances in the US West \(Oregon\) Region that are associated with the profile you specified with the `Set-AWSCredential` cmdlet\.

```
PS > Set-AWSCredential -ProfileName SAMLDemoProfile
PS > Get-EC2Instance -Region us-west-2 | Format-Table -Property Instances,GroupNames

Instances                                                   GroupNames
---------                                                   ----------
{TestInstance1}                                             {default}
{TestInstance2}                                             {}
{TestInstance3}                                             {launch-wizard-6}
{TestInstance4}                                             {default}
{TestInstance5}                                             {}
{TestInstance6}                                             {AWS-OpsWorks-Default-Server}
```

### Example 2: Change Role Profiles During a PowerShell Session<a name="example-2-change-role-profiles-during-a-powershell-session"></a>

This example lists all available Amazon S3 buckets in the AWS account of the role associated with the `SAMLDemoProfile` profile\. The example shows that although you might have been using another profile earlier in your AWS Tools for PowerShell session, you can change profiles by specifying a different value for the `-ProfileName` parameter with cmdlets that support it\. This is a common task for administrators who manage Amazon S3 from the PowerShell command line\.

```
PS > Get-S3Bucket -ProfileName SAMLDemoProfile

CreationDate                                                BucketName
------------                                                ----------
7/25/2013 3:16:56 AM                                        mybucket1
4/15/2015 12:46:50 AM                                       mybucket2
4/15/2015 6:15:53 AM                                        mybucket3
1/12/2015 11:20:16 PM                                       mybucket4
```

Note that the `Get-S3Bucket` cmdlet specifies the name of the profile created by running the `Set-AWSSamlRoleProfile` cmdlet\. This command could be useful if you had set a role profile earlier in your session \(for example, by running the `Set-AWSCredential` cmdlet\) and wanted to use a different role profile for the `Get-S3Bucket` cmdlet\. The profile manager makes temporary credentials available to the `Get-S3Bucket` cmdlet\.

Although the credentials expire after 1 hour \(a limit enforced by STS\), AWS Tools for PowerShell automatically refreshes the credentials by requesting a new SAML assertion when the tool detects that the current credentials have expired\.

For domain\-joined users, this process occurs without interruption, because the current user's Windows identity is used during authentication\. For non\-domain\-joined user accounts, AWS Tools for PowerShell shows a PowerShell credential prompt requesting the user password\. The user provides credentials that are used to reauthenticate the user and get a new assertion\.

### Example 3: Get Instances in a Region<a name="example-3-get-instances-in-a-region"></a>

The following example lists all Amazon EC2 instances in the Asia Pacific \(Sydney\) Region that are associated with the account used by the `ADFS-Production` profile\. This is a useful command for returning all Amazon EC2 instances in a region\.

```
PS > (Get-Ec2Instance -ProfileName ADFS-Production -Region ap-southeast-2).Instances | Select InstanceType, @{Name="Servername";Expression={$_.tags | where key -eq "Name" | Select Value -Expand Value}}

 InstanceType                                                Servername
 ------------                                                ----------
 t2.small                                                    DC2
 t1.micro                                                    NAT1
 t1.micro                                                    RDGW1
 t1.micro                                                    RDGW2
 t1.micro                                                    NAT2
 t2.small                                                    DC1
 t2.micro                                                    BUILD
```

## Additional Reading<a name="saml-pst-reading"></a>

For general information about how to implement federated API access, see [How to Implement a General Solution for Federated API/CLI Access Using SAML 2\.0](http://aws.amazon.com/blogs/security/how-to-implement-a-general-solution-for-federated-apicli-access-using-saml-2-0/)\.

For support questions or comments, visit the AWS Developer Forums for [PowerShell Scripting](https://forums.aws.amazon.com/forum.jspa?forumID=149) or [\.NET Development](https://forums.aws.amazon.com/forum.jspa?forumID=61)\.