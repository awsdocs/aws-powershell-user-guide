# Installing the AWS Tools for PowerShell on Windows<a name="pstools-getting-set-up-windows"></a>

A Windows\-based computer can run any of the AWS Tools for PowerShell package options: 
+ [**AWS\.Tools**](#ps-installing-awstools) \- The modularized version of AWS Tools for PowerShell\. Each AWS service is supported by its own individual, small module, with shared support modules `AWS.Tools.Common` and `AWS.Tools.Installer`\.
+ [**AWSPowerShell\.NetCore**](#ps-installing-awspowershellnetcore) \- The single, large\-module version of AWS Tools for PowerShell\. All AWS services are supported by this single, large module\.
+ [**AWSPowerShell**](#ps-installing-awswindowspowershell) \- The legacy Windows\-specific, single, large\-module version of AWS Tools for PowerShell\. All AWS services are supported by this single, large module\.

The package you choose depends on the release and edition of Windows that you're running\. 

**Note**  
The Tools for Windows PowerShell \(AWSPowerShell module\) are installed by default on all Windows\-based Amazon Machine Images \(AMIs\)\.

Setting up the AWS Tools for PowerShell involves the following high\-level tasks, described in detail in this topic\.

1. Install the AWS Tools for PowerShell package option that's appropriate for your environment\.

1. Verify that script execution is enabled by running the `Get-ExecutionPolicy` cmdlet\.

1. Import the AWS Tools for PowerShell module into your PowerShell session\.

## Prerequisites<a name="prerequisites"></a>

Ensure that you meet the requirements listed in [Prerequisites for Setting up the AWS Tools for PowerShell](pstools-getting-set-up-prereq.md)\.

Newer versions of PowerShell, including PowerShell Core, are available as downloads from Microsoft at [Installing various versions of PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell) on Microsoft's Web site\.

## Install AWS\.Tools on Windows<a name="ps-installing-awstools"></a>

You can install the modularized version of AWS Tools for PowerShell on computers that are running Windows with Windows PowerShell 5\.1, or PowerShell Core 6\.0 or later\. For information about how to install PowerShell Core, see [Installing various versions of PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell) on Microsoft's Web site\.

You can install AWS\.Tools in one of three ways:
+ Using the cmdlets in the `AWS.Tools.Installer` module\. The `AWS.Tools.Installer` module simplifies the installation and update of other AWS\.Tools modules\. `AWS.Tools.Installer` requires, and automatically downloads and installs, an updated version of `PowerShellGet`\. The `AWS.Tools.Installer` module also automatically keeps your module versions in sync\. When you install or update to a newer version of one module, the cmdlets in the `AWS.Tools.Installer` automatically update all of your other AWS\.Tools modules to the same version\.
+ Installing each service module from PowerShell Gallery using the `Install-Module` cmdlet\.
+ Downloading the modules as a [ZIP archive](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWS.Tools.zip) and extracting them in one of the module folders\. You can discover your module folders by printing the value of the `$Env:PSModulePath` variable.

**To install AWS\.Tools on Windows**

1. Start a PowerShell session\.
**Note**  
We recommend that you *don't* run PowerShell as an administrator with elevated permissions except when required by the task at hand\. This is because of the potential security risk and is inconsistent with the principle of least privilege\.

1. To install the modularized AWS\.Tools package, run the following command\.

   ```
   PS > Install-Module -Name AWS.Tools.Installer
   
   Untrusted repository
   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
    you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
   ```

   If you are notified that the repository is "untrusted", it asks you if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. To avoid the prompt and install the module without trusting the repository, you can run the command with the `-Force` parameter\.

   ```
   PS > Install-Module -Name AWS.Tools.Installer -Force
   ```

1. You can now install the module for each AWS service that you want to use by using the `Install-AWSToolsModule` cmdlet\. For example, the following command installs the IAM module\. This command also installs any dependent modules that are required for the specified module to work\. For example, when you install your first AWS\.Tools service module, it also installs `AWS.Tools.Common`\. This is a shared module required by all AWS service modules\. It also removes older versions of the modules, and updates other modules to the same newer version\.

   ```
   PS > Install-AWSToolsModule AWS.Tools.EC2,AWS.Tools.S3 -CleanUp
     Confirm
     Are you sure you want to perform this action?
     Performing the operation "Install-AWSToolsModule" on target "AWS Tools version 4.0.0.0".
     [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
   
     Installing module AWS.Tools.Common version 4.0.0.0
     Installing module AWS.Tools.EC2 version 4.0.0.0
     Installing module AWS.Tools.Glacier version 4.0.0.0
     Installing module AWS.Tools.S3 version 4.0.0.0
   
     Uninstalling AWS.Tools version 3.3.618.0
     Uninstalling module AWS.Tools.Glacier
     Uninstalling module AWS.Tools.S3
     Uninstalling module AWS.Tools.SimpleNotificationService
     Uninstalling module AWS.Tools.SQS
     Uninstalling module AWS.Tools.Common
   ```
**Note**  
The `Install-AWSToolsModule` cmdlet downloads all requested modules from the `PSRepository` named `PSGallery` \([https://www\.powershellgallery\.com/](https://www.powershellgallery.com/)\) and considers it a trusted source\. Use the command `Get-PSRepository -Name PSGallery` for more information about this `PSRepository`\.

   By default, this command installs modules into the `$home\Documents\PowerShell\Modules` folder\. To install the AWS Tools for PowerShell for all users of a computer, you must run the following command in a PowerShell session that you started as an administrator\. This installs modules to the `$env:ProgramFiles\PowerShell\Modules` folder that is accessible by all users\.

   ```
   PS > Install-AWSToolsModule AWS.Tools.IdentityManagement -Scope AllUsers
   ```

## Install AWSPowerShell\.NetCore on Windows<a name="ps-installing-awspowershellnetcore"></a>

You can install the AWSPowerShell\.NetCore on computers that are running Windows with PowerShell version 3 through 5\.1, or PowerShell Core 6\.0 or later\. For information about how to install PowerShell Core, see [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\.

You can install AWSPowerShell\.NetCore in one of two ways:
+ Installing from PowerShell Gallery using the `Install-Module` cmdlet\.
+ Downloading the module as a [ZIP archive](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWSPowerShell.NetCore.zip) and extracting it in one of the module directories\. You can discover your module directories by printing the value of the `$Env:PSModulePath` variable.

To install the AWSPowerShell\.NetCore from the PowerShell Gallery, your computer must be running PowerShell 5\.0 or later, or running [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) on PowerShell 3 or later\. Run the following command\.

```
PS > Install-Module -name AWSPowerShell.NetCore
```

If you're running PowerShell as administrator, the previous command installs AWS Tools for PowerShell for all users on the computer\. If you're running PowerShell as a standard user without administrator permissions, that same command installs AWS Tools for PowerShell for only the current user\.

To install for only the current user when that user has administrator permissions, run the command with the `-Scope CurrentUser` parameter set, as follows\.

```
PS > Install-Module -name AWSPowerShell.NetCore -Scope CurrentUser
```

Although PowerShell 3\.0 and later releases typically load modules into your PowerShell session the first time you run a cmdlet in the module, the AWSPowerShell\.NetCore module is too large to support this functionality\. You must instead explicitly load the AWSPowerShell\.NetCore Core module into your PowerShell session by running the following command\.

```
PS > Import-Module AWSPowerShell.NetCore
```

To load the AWSPowerShell\.NetCore module into a PowerShell session automatically, add that command to your PowerShell profile\. For more information about editing your PowerShell profile, see [About Profiles](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_profiles) in the PowerShell documentation\.

## Install AWSPowerShell on Windows PowerShell<a name="ps-installing-awswindowspowershell"></a>

You can install AWSPowerShell in one of three ways:
+ Installing from PowerShell Gallery using the `Install-Module` cmdlet\.
+ Downloading the module as a [ZIP archive](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWSPowerShell.zip) and extracting it in one of the module directories\. You can discover your module directories by printing the value of the `$Env:PSModulePath` variable.
+ Running the [Tools for Windows PowerShell installer](https://sdk-for-net.amazonwebservices.com/latest/AWSToolsAndSDKForNet.msi). This method of installing AWSPowerShell is deprecated and we recommend that you use `Install-Module` instead.

You can install the AWSPowerShell from the PowerShell Gallery if you're running PowerShell 5\.0 or later, or have installed [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) on PowerShell 3 or later\. You can install and update AWSPowerShell from Microsoft's [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) by running the following command\.

  ```
  PS > Install-Module -Name AWSPowerShell
  ```
To load the AWSPowerShell module into a PowerShell session automatically, add the previous `import-module` cmdlet to your PowerShell profile\. For more information about editing your PowerShell profile, see [About Profiles](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-6) in the PowerShell documentation\.

**Note**  
The Tools for Windows PowerShell are installed by default on all Windows\-based Amazon Machine Images \(AMIs\)\.

## Enable Script Execution<a name="enable-script-execution"></a>

To load the AWS Tools for PowerShell modules, you must enable PowerShell script execution\. To enable script execution, run the `Set-ExecutionPolicy` cmdlet to set a policy of `RemoteSigned`\. For more information, see [About Execution Policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies) on the Microsoft Technet website\.

**Note**  
This is a requirement only for computers that are running Windows\. The `ExecutionPolicy` security restriction is not present on other operating systems\.

 **To enable script execution** 

1. Administrator rights are required to set the execution policy\. If you are not logged in as a user with administrator rights, open a PowerShell session as Administrator\. Choose **Start**, and then choose **All Programs**\. Choose **Accessories**, and then choose **Windows PowerShell**\. Right\-click **Windows PowerShell**, and on the context menu, choose **Run as administrator**\.

1. At the command prompt, enter the following\.

   ```
   PS > Set-ExecutionPolicy RemoteSigned 
   ```

**Note**  
On a 64\-bit system, you must do this separately for the 32\-bit version of PowerShell, **Windows PowerShell \(x86\)**\.

If you don't have the execution policy set correctly, PowerShell shows the following error whenever you try to run a script, such as your profile\.

```
File C:\Users\username\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 cannot be loaded because the execution
 of scripts is disabled on this system. Please see "get-help about_signing" for more details.
At line:1 char:2
+ . <<<<  'C:\Users\username\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'
    + CategoryInfo          : NotSpecified: (:) [], PSSecurityException
    + FullyQualifiedErrorId : RuntimeException
```

The Tools for Windows PowerShell installer automatically updates the [PSModulePath](http://msdn.microsoft.com/en-us/library/windows/desktop/dd878326.aspx) to include the location of the directory that contains the `AWSPowerShell` module\. 

Because the `PSModulePath` includes the location of the AWS module's directory, the `Get-Module -ListAvailable` cmdlet shows the module\.

```
PS > Get-Module -ListAvailable

ModuleType Name                      ExportedCommands
---------- ----                      ----------------
Manifest   AppLocker                 {}
Manifest   BitsTransfer              {}
Manifest   PSDiagnostics             {}
Manifest   TroubleshootingPack       {}
Manifest   AWSPowerShell             {Update-EBApplicationVersion, Set-DPStatus, Remove-IAMGroupPol...
```

## Versioning<a name="pstools-versioning"></a>

AWS releases new versions of the AWS Tools for PowerShell periodically to support new AWS services and features\. To determine the version of the Tools that you have installed, run the [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet\.

```
PS > Get-AWSPowerShellVersion

AWS Tools for PowerShell
Version 4.0.123.0
Copyright 2012-2019 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Amazon Web Services SDK for .NET
Core Runtime Version 3.3.103.22
Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Release notes: https://github.com/aws/aws-tools-for-powershell/blob/master/CHANGELOG.md

This software includes third party software subject to the following copyrights:
- Logging from log4net, Apache License
[http://logging.apache.org/log4net/license.html]
```

You can also add the `-ListServiceVersionInfo` parameter to a [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) command to see a list of the AWS services that are supported in the current version of the tools\. If you use the modularized `AWS.Tools.*` option, only the modules that you currently have imported are displayed\.

```
PS > Get-AWSPowerShellVersion -ListServiceVersionInfo

AWS Tools for Windows PowerShell
Version 3.3.96.0
Copyright 2012-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Amazon Web Services SDK for .NET
Core Runtime Version 3.3.14.0
Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.

Release notes: https://aws.amazon.com/releasenotes/PowerShell

This software includes third party software subject to the following copyrights:
- Logging from log4net, Apache License
[http://logging.apache.org/log4net/license.html]


Service                            Noun Prefix Version
-------                            ----------- -------
AWS AppStream                       APS         2016-12-01
AWS Batch                           BAT         2016-08-10
AWS Budgets                         BGT         2016-10-20
AWS Certificate Manager             ACM         2015-12-08
AWS Cloud Directory                 CDIR        2016-05-10
AWS Cloud HSM                       HSM         2014-05-30
AWS CloudFormation                  CFN         2010-05-15
AWS CloudTrail                      CT          2013-11-01
AWS CodeBuild                       CB          2016-10-06
AWS CodeCommit                      CC          2015-04-13
AWS CodeDeploy                      CD          2014-10-06
AWS CodePipeline                    CP          2015-07-09
AWS CodeStar                        CST         2017-04-19
AWS Config                          CFG         2014-11-12
AWS Cost and Usage Report           CUR         2017-01-06
AWS Data Pipeline                   DP          2012-10-29
AWS Database Migration Service      DMS         2016-01-01
AWS Device Farm                     DF          2015-06-23
AWS Direct Connect                  DC          2012-10-25
AWS Directory Service               DS          2015-04-16
AWS Elastic Beanstalk               EB          2010-12-01

...
```

To determine the version of PowerShell that you are running, enter `$PSVersionTable` to view the contents of the $PSVersionTable [automatic variable](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6)\.

```
PS > $PSVersionTable

Name                           Value
----                           -----
PSVersion                      6.2.2
PSEdition                      Core
GitCommitId                    6.2.2
OS                             Darwin 18.7.0 Darwin Kernel Version 18.7.0: Tue Aug 20 16:57:14 PDT 2019; root:xnu-4903.271.2~2/RELEASE_X86_64
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

## Updating the AWS Tools for PowerShell on Windows<a name="pstools-updating"></a>

Periodically, as updated versions of the AWS Tools for PowerShell are released, you should update the version that you are running locally\. 

### Update the Modularized AWS\.Tools<a name="update-the-tools-for-powershell"></a>

To upgrade your AWS\.Tools modules to the latest version, run the following command\.

```
PS > Update-AWSToolsModule -CleanUp
```

This command updates all of the currently installed `AWS.Tools` modules and, after a successful update, removes other installed versions\.

**Note**  
The `Update-AWSToolsModule` cmdlet downloads all modules from the `PSRepository` named `PSGallery` \([https://www\.powershellgallery\.com/](https://www.powershellgallery.com/)\) and considers it a trusted source\. Use the command: `Get-PSRepository -Name PSGallery` for more information on this `PSRepository`\.

### Update the Tools for PowerShell Core<a name="update-the-tools-for-powershell-core"></a>

Run the `Get-AWSPowerShellVersion` cmdlet to determine the version that you are running, and compare that with the version of Tools for Windows PowerShell that is available on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) website\. We suggest you check every two to three weeks\. Support for new commands and AWS services is available only after you update to a version with that support\.

Before you install a newer release of AWSPowerShell\.NetCore, uninstall the existing module\. Close any open PowerShell sessions before you uninstall the existing package\. Run the following command to uninstall the package\.

```
PS > Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions
```

After the package is uninstalled, install the updated module by running the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore
```

After installation, run the command `Import-Module AWSPowerShell.NetCore` to load the updated cmdlets into your PowerShell session\.

### Update the Tools for Windows PowerShell<a name="update-the-tools-for-windows-powershell"></a>

Run the `Get-AWSPowerShellVersion` cmdlet to determine the version that you are running, and compare that with the version of Tools for Windows PowerShell that is available on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) website\. We suggest you check every two to three weeks\. Support for new commands and AWS services is available only after you update to a version with that support\.
+ If you installed by using the `Install-Module` cmdlet, run the following commands\.

  ```
  PS > Uninstall-Module -Name AWSPowerShell -AllVersions
  PS > Install-Module -Name AWSPowerShell
  ```
+ If you installed by using the `.msi` package installer:

  1. Download the most recent version of the MSI package from [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/)\. Compare the package version number in the MSI file name with the version number you get when you run the `Get-AWSPowerShellVersion` cmdlet\.

  1. If the download version is a higher number than the version you have installed, close all Tools for Windows PowerShell consoles\.

  1. Install the newer version of the Tools for Windows PowerShell by running the MSI package you downloaded\.

After installation, run `Import-Module AWSPowerShell` to load the updated cmdlets into your PowerShell session\. Or run the custom AWS Tools for PowerShell console from your **Start** menu\.