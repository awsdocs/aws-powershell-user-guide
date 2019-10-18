# Installing AWS Tools for PowerShell on Linux or macOS<a name="pstools-getting-set-up-linux-mac"></a>

This topic provides instructions on how to install the AWS Tools for PowerShell on Linux or macOS\.

## Overview of Setup<a name="pstools-installing-core-prerequisites"></a>

When you want to install AWS Tools for PowerShell on a Linux or macOS computer, you can choose from two package options:
+ **AWS\.Tools** \- The modularized version of AWS Tools for PowerShell\.
**Prerelease Evaluation Software**  
The modularized AWS\.Tools version of the product is provided as prerelease software for testing and evaluation\. Because it's prerelease software, we recommend that you don't use this in a production environment\. For production environments, we recommend that you instead use the generally available AWSPowerShell\.NetCore\.
+ [**AWSPowerShell\.NetCore**](pstools-getting-set-up-windows.md#ps-installing-awspowershellnetcore) \- The single, large\-module version of AWS Tools for PowerShell\.

 Setting either of these up on a computer running Linux or macOS involves the following tasks, described in detail later in this topic:

1. Install PowerShell Core 6\.0 or later on a supported system\.

1. After installing PowerShell Core, start PowerShell by running `pwsh` in your system shell\.

1. Install the either AWS\.Tools or AWSPowerShell\.NetCore\.

1. Run the appropriate `Import-Module` cmdlet to import the module into your PowerShell session\.

1. Run the [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) cmdlet to provide your AWS credentials\.

## Prerequisites<a name="prerequisites"></a>

Ensure that you meet the requirements listed on [Prerequisites for Setting up the AWS Tools for PowerShell](pstools-getting-set-up-prereq.md)\.

To run the AWS Tools for PowerShell Core, your computer must be running PowerShell Core 6\.0 or later\. 
+ For a list of the supported Linux versions and for information about how to install PowerShell Core 6\.0 or later on a Linux\-based computer, see [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux?view=powershell-6) on Microsoft's website\. Some Linux\-based operating systems, such as Arch, Kali, and Raspbian, are not officially supported, but have varying levels of community support\.
+ For a list of supported macOS versions and for information about how to install PowerShell Core 6\.0 on macOS 10\.12 or later, see [Installing PowerShell Core on macOS](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-macos) on Microsoft's website\.

## Install the Prerelease Version of AWS\.Tools on a Linux or macOS<a name="install-aws.tools-on-linux-macos"></a>

To upgrade to a later release of the AWS\.Tools, follow the instructions in [Updating the AWS Tools for PowerShell on Linux or macOS](#pstools-updating-linux)\. Uninstall earlier versions of the AWS Tools for PowerShell first\.

Start a PowerShell Core session by running the following command\.

```
$ pwsh
```

**Note**  
We do not recommend that you start PowerShell by running `sudo pwsh` to run PowerShell with elevated, administrator rights, because of the potential security risk and inconsistency with the principle of least privilege\.

To install the prerelease version of the modularized AWS\.Tools package, run the following command\.

```
PS > Install-Module -Name AWS.Tools.Common

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
 you want to install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
```

If you are notified that the repository is "untrusted", you're asked you if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. Alternatively, to avoid the prompt without trusting the repository, you can run the following command\.

```
PS > Install-Module -Name AWS.Tools.Common -Force
```

You can now install the module for each service that you want to use\. For example, the following command installs the IAM module, again ignoring any prompt to trust the repository\.

```
PS > Install-Module -Name AWS.Tools.IdentityManagement -Force
```

You don't have to run this command as an administrator, unless you want to install the AWS Tools for PowerShell for all users of a computer\. To do this, run the following command in a PowerShell session that you have started with `sudo pwsh`\.

```
PS > Install-Module -Scope CurrentUser -Name AWS.Tools.Common -Force
```

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, [Introducing AWS Tools for PowerShell Core Edition](https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition)\.

## Install AWSPowerShell\.NetCore on Linux or macOS<a name="install-netcore-on-linux-macos"></a>

To upgrade to a newer release of AWSPowerShell\.NetCore, follow the instructions in [Updating the AWS Tools for PowerShell on Linux or macOS](#pstools-updating-linux)\. Uninstall earlier versions of PowerShell first\.

Start a PowerShell Core session by running the following command\.

```
$ pwsh
```

**Note**  
We do not recommend that you start PowerShell by running `sudo pwsh` to run PowerShell with elevated, administrator rights, because of the potential security risk and it inconsistency with the principle of least privilege\.

To install the AWSPowerShell\.NetCore single\-module package, run the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
 you want to install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
```

If you are notified that the repository is "untrusted", you're asked if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. Alternatively, to avoid the prompt without trusting the repository, you can run the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore -Force
```

You don't have to run this command as root, unless you want to install the AWS Tools for PowerShell for all users of a computer\. To do this, run the following command in a PowerShell session that you have started with `sudo pwsh`\.

```
PS > Install-Module -Scope AllUsers -Name AWSPowerShell.NetCore -Force
```

## Script Execution<a name="enable-script-execution"></a>

The `Set-ExecutionPolicy` command is not available on non\-Windows systems\. You can run `Get-ExecutionPolicy`, which shows that the default execution policy setting in PowerShell Core running on non\-Windows systems is `Unrestricted`\. For more information about execution policies, see [About Execution Policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-5.1) on the Microsoft Technet website\.

 Because the `PSModulePath` includes the location of the AWS module's directory, the `Get-Module -ListAvailable` cmdlet shows the module that you installed\.

**AWS\.Tools:**

```
PS > Get-Module -ListAvailable

    Directory: /Users/username/.local/share/powershell/Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Binary     3.3.563.1  AWS.Tools.Common                    Desk      {Clear-AWSHistory, Set-AWSHistoryConfiguration, Initialize-AWSDefaultConfiguration, Clear-AWSDefaultConfigurat…
```

**AWSPowerShell\.NetCore:**

```
PS > Get-Module -ListAvailable

Directory: /Users/username/.local/share/powershell/Modules

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Binary     3.3.563.1  AWSPowerShell.NetCore
```

## Configure a PowerShell Console to Use the AWS Tools for PowerShell Core \(AWSPowerShell\.NetCore\)<a name="pstools-config-ps-window"></a>

PowerShell Core typically loads modules automatically whenever you run a cmdlet in the module\. But this doesn't work for AWSPowerShell\.NetCore because of its large size\. To start running AWS Tools for PowerShell cmdlets, you must first run the `Import-Module AWSPowerShell.NetCore` command\.

## Initialize Your PowerShell Session<a name="linux-config-init"></a>

When you start PowerShell on a Linux\-based or macOS\-based system after you have installed the AWS Tools for PowerShell Core, you must run [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) to specify which AWS access key to use\. For more information about `Initialize-AWSDefaultConfiguration`, see [Using AWS Credentials](specifying-your-aws-credentials.md)\. 

**Note**  
In earlier \(before 3\.3\.96\.0\) releases of the AWS Tools for PowerShell, this cmdlet was named `Initialize-AWSDefaults`\.

## Versioning<a name="pstools-versioning"></a>

AWS releases new versions of the AWS Tools for PowerShell periodically to support new AWS services and features\. To determine the version of the AWS Tools for PowerShell that you have installed, run the [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet\.

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

You can also add the `-ListServiceVersionInfo` parameter to a [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet to see a list of the supported AWS services in the current version of the tools\.

```
PS > Get-AWSPowerShellVersion -ListServiceVersionInfo

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


Service                                               Noun Prefix API Version
-------                                               ----------- -----------
AWS Amplify                                           AMP         2017-07-25
AWS App Mesh                                          AMSH        2019-01-25
AWS AppStream                                         APS         2016-12-01
AWS AppSync                                           ASYN        2017-07-25
AWS Auto Scaling Plans                                ASP         2018-01-06
AWS Batch                                             BAT         2016-08-10
AWS Budgets                                           BGT         2016-10-20
AWS Certificate Manager                               ACM         2015-12-08
AWS Certificate Manager Private Certificate Authority PCA         2017-08-22
AWS Cloud Directory                                   CDIR        2017-01-11
AWS Cloud HSM                                         HSM         2014-05-30
AWS Cloud HSM V2                                      HSM2        2017-04-28
AWS Cloud9                                            C9          2017-09-23
AWS CloudFormation                                    CFN         2010-05-15
AWS CloudTrail                                        CT          2013-11-01
AWS CodeBuild                                         CB          2016-10-06
AWS CodeCommit                                        CC          2015-04-13

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

## Updating the AWS Tools for PowerShell on Linux or macOS<a name="pstools-updating-linux"></a>

Periodically, as updated versions of the AWS Tools for PowerShell are released, you should update the version that you're running locally\. 

Run the `Get-AWSPowerShellVersion` cmdlet to determine the version that you are running, and compare that with the version of Tools for Windows PowerShell that is available on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell) website\. We suggest you check every two to three weeks\. Support for new commands and AWS services is available only after you update to a version with that support\.

### Update the Modularized Prerelease Version of AWS\.Tools\.\*<a name="update-aws.tools-all-systems"></a>

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

After installation, run the command `Import-Module AWS.Tools.Common` to load the updated module into your PowerShell session\.

### Update the Tools for PowerShell Core<a name="update-netcore-all-systems"></a>

Before you install a newer release of AWSPowerShell\.NetCore, uninstall the existing module\. Close any open PowerShell sessions before you uninstall the existing package\. Run the following command to uninstall the package\.

```
PS > Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions
```

When uninstallation is finished, install the updated module by running the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore
```

After installation, run the command `Import-Module AWSPowerShell.NetCore` to load the updated cmdlets into your PowerShell session\.

## Related Information<a name="pstools-seealso-setup"></a>
+  [Getting Started with the AWS Tools for Windows PowerShell](pstools-getting-started.md) 
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [AWS Account and Access Keys](pstools-appendix-sign-up.md) 