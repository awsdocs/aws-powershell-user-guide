# Installing the AWS Tools for PowerShell on Windows<a name="pstools-getting-set-up-windows"></a>

## Overview of Setup<a name="pstools-installing-windows-prerequisites"></a>

A Windows\-based computer can run any of the AWS Tools for PowerShell package options: 
+ The AWSPowerShell module
+ The AWSPowerShell\.NetCore module
+ The AWS\.Tools module

The package you choose depends on the release and edition of Windows that you're running\. 

**Note**  
The Tools for Windows PowerShell \(AWSPowerShell module\) are installed by default on all Windows\-based Amazon Machine Images \(AMIs\)\.

Setting up the AWS Tools for PowerShell involves the following high\-level tasks, described in detail in this topic\.

1. Install the AWS Tools for PowerShell package option that's appropriate for your environment\.

1. Verify that script execution is enabled by running the `Get-ExecutionPolicy` cmdlet\.

1. Import the AWS Tools for PowerShell module into your PowerShell session\.

## Prerequisites<a name="prerequisites"></a>

Ensure that you meet the requirements listed in [Prerequisites for Setting up the AWS Tools for PowerShell](pstools-getting-set-up-prereq.md)\.

For a list of PowerShell versions and which versions of Windows each was introduced in, see [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\.

Newer versions of PowerShell, including PowerShell Core, are available as downloads from Microsoft at [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\.

## Install the Prerelease Version of AWS\.Tools on Windows<a name="pstools-installing-download"></a>

Depending on the version of PowerShell you install, you can choose from three package options:
+ **AWS\.Tools** \- The modularized version of AWS Tools for PowerShell that runs on either of the following configurations:
  + PowerShell Core 6\.0 or later\.
  + Windows PowerShell 3\.0 or later, as long as \.NET Framework 4\.7\.2 or later is also installed\.
**Prerelease Evaluation Software**  
The modularized AWS\.Tools version of the product is provided as prerelease software for testing and evaluation\. Because it's prerelease software, we recommend that you don't use this in a production environment\. For production environments, we recommend that you use the generally available AWSPowerShell\.NetCore\.
+ [**AWSPowerShell\.NetCore**](#ps-installing-awspowershellnetcore) \- The monolithic version of AWS Tools for PowerShell that runs on either of the following configurations:
  + PowerShell Core 6\.0 or later\.
  + Windows PowerShell 3\.0 or later, as long as \.NET Framework 4\.7\.2 or later is also installed\.
+ [**AWSPowerShell**](#ps-installing-awswindowspowershell) \- The monolithic version of AWS Tools for PowerShell that runs only on Windows, and is compatible with PowerShell 2\.0 through 5\.1\.

If you already have a version of AWS Tools for PowerShell installed, we recommend that you uninstall the earlier version of AWS Tools for PowerShell first\. Follow the instructions in [Updating the AWS Tools for PowerShell on Windows](#pstools-updating)\. 

## Install AWS\.Tools on Windows<a name="ps-installing-awstools"></a>

You can install the modularized version of AWS Tools for PowerShell on computers that are running Windows with Windows PowerShell versions 3 through 5\.1, or PowerShell Core 6\.0 or later\. For information about how to install PowerShell Core, see [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\.

To install the prerelease version of the modularized AWS\.Tools package, run the following command\.

```
PS > Install-Module -Name AWS.Tools.Common

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
 you want to install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
```

If you are notified that the repository is "untrusted", you're asked if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. Alternatively, to avoid the prompt without trusting the repository, you can run the following command\.

```
PS > Install-Module -Name AWS.Tools.Common -Force
```

You can now install the module for each service that you want to use\. For example, the following command installs the IAM module, again ignoring any prompt to trust the repository\.

```
PS > Install-Module -Name AWS.Tools.IdentityManagement -Force
```

## Install AWSPowerShell\.NetCore on Windows<a name="ps-installing-awspowershellnetcore"></a>

You can install the AWS Tools for PowerShell Core on computers that are running Windows with PowerShell version 3 through 5\.1, or PowerShell Core 6\.0 or later\. For information about how to install PowerShell Core, see [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\.

To install the AWS Tools for PowerShell Core, your computer must be running PowerShell 5\.0 or later, or running [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) on PowerShell 3 or later\. Run the following command\.

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

You can install the AWS Tools for Windows PowerShell in one of two ways, depending on which version of Windows PowerShell you have installed:
+ If you're running PowerShell 5\.0 or later, or have installed [PowerShellGet](https://www.powershellgallery.com/packages/PowerShellGet) on PowerShell 3 or later, you can install and update the Tools for Windows PowerShell from Microsoft's [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) website by running the following command\.

  ```
  PS > Install-Module -Name AWSPowerShell
  ```
+ If you're running a version of PowerShell earlier than 5\.0, you can run the Tools for Windows PowerShell installer `.msi`\. To download the installer, navigate to [http://aws.amazon.com/powershell/](http://aws.amazon.com/powershell/), and then choose **AWS Tools for Windows**\.
**Note**  
You can run the installer `.msi` if you're running Windows PowerShell 5\.0 or later\. But we recommend that you use the Install\-Module cmdlet instead so that you have easier support for updating or removing the module\.

  The installer includes the most recent versions of the AWS SDK for \.NET assemblies for the \.NET 3\.5 and 4\.5 Frameworks\. If you have Visual Studio 2013 or 2015 or later installed, the installer can also install the [AWS Toolkit for Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/welcome.html)\.

To load the AWSPowerShell module into a PowerShell session automatically, add the previous `import-module` cmdlet to your PowerShell profile\. For more information about editing your PowerShell profile, see [About Profiles](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-6) in the PowerShell documentation\.

**Note**  
The Tools for Windows PowerShell are installed by default on all Windows\-based Amazon Machine Images \(AMIs\)\.

## Enable Script Execution<a name="enable-script-execution"></a>

To load the AWS Tools for PowerShell modules, you must enable PowerShell script execution\. To enable script execution, run the `Set-ExecutionPolicy` cmdlet to set a policy of `RemoteSigned`\. For more information about execution policies, see [About Execution Policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies) on the Microsoft Technet website\.

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
Version 3.3.563.1
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

Run the `Get-AWSPowerShellVersion` cmdlet to determine the version that you are running, and compare that with the version of Tools for Windows PowerShell that is available on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) website\. We suggest you check every two to three weeks\. Support for new commands and AWS services is available only after you update to a version with that support\.

### Update the Modularized Prerelease Version of AWS\.Tools<a name="update-the-tools-for-powershell"></a>

Before you install a newer release of the AWS\.Tools, uninstall any existing modules\. Close any open PowerShell sessions before you uninstall the existing AWS\.Tools package\. Run the following commands to uninstall the package\.

```
PS > Get-Module -Name AWS.Tools.* | Uninstall-Module
```

If you see an error stating that a module can't be removed because of a dependency, re\-run the command until you no longer get the error\. This can occur if PowerShell tries to uninstall `AWS.Tools.Common` before all of the other AWS service modules are uninstalled\.

When uninstallation is finished, you can install a newer version by running the following command\.

```
PS > Install-Module -Name AWS.Tools.Common
```

You must also reinstall any of the other `AWS.Tools.*` modules that you were using\.

### Update the Tools for PowerShell Core<a name="update-the-tools-for-powershell-core"></a>

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