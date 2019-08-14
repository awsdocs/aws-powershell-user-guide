# Setting up the AWS Tools for PowerShell Core on Linux or macOS X<a name="pstools-getting-set-up-linux-mac"></a>

## Overview of Setup<a name="pstools-installing-core-prerequisites"></a>

**Note**  
You can skip installing AWS Tools for PowerShell Core installation on the \.NET Core with Ubuntu Server 16\.04 and \.NET Core with Amazon Linux 2 LTS Candidate Amazon Machine Images \(AMIs\)\. These AMIs are preconfigured with \.NET Core 2\.0, PowerShell Core 6\.0, the AWS Tools for PowerShell Core, and the AWS CLI\. You must still run `Import-Module AWSPowerShell.NetCore` in a PowerShell session to load the AWS Tools for PowerShell Core module\.

A non\-Windows\-based computer can run only the AWS Tools for PowerShell Core \(AWSPowerShell\.NetCore\)\. Setting up the AWS Tools for PowerShell Core involves the following tasks, described in this topic\.

1. Install Microsoft PowerShell Core 6\.0 or newer on a supported non\-Windows system\.

1. After installing Microsoft PowerShell Core, start PowerShell by running `pwsh` in your system shell\.

1. Install the AWS Tools for PowerShell Core\.

1. Run `Import-Module AWSPowerShell.NetCore` to import the AWS Tools for PowerShell Core module into your PowerShell session\.

1. Run the [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) cmdlet to provide your AWS credentials\.

## Prerequisites<a name="prerequisites"></a>

To use the the AWS Tools for PowerShell Core, you must have an AWS account\. If you do not yet have an AWS account, see [AWS Account and Access Keys](pstools-appendix-sign-up.md) for instructions\.

To run the AWS Tools for PowerShell Core, your system must be running Microsoft PowerShell Core 6\.0 or newer\. For more information about how to install PowerShell Core 6\.0 or newer on a Linux\-based computer, see [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux?view=powershell-6)\. For information about how to install PowerShell Core 6\.0 on macOS 10\.12 or higher, see [Installing PowerShell Core on macOS](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-macos?view=powershell-6)\.

## Install the AWS Tools for PowerShell Core on Linux, macOS X, and Other Non\-Windows Systems<a name="install-the-tpclong-on-linux-macos-x-and-other-non-windows-systems"></a>

To upgrade to a newer release of the AWS Tools for PowerShell Core, follow instructions in [pstools\-updating\-core](#pstools-updating-core)\. Uninstall older versions of PowerShell first\.

You can install the AWS Tools for PowerShell Core on computers that are running Microsoft PowerShell Core 6\.0 or newer\. Microsoft PowerShell Core 6\.0 is supported on the following non\-Windows\-based operating systems\.
+ Ubuntu 14\.04 LTS and newer
+ CentOS Linux 7
+ Arch Linux
+ Debian 8\.7 and newer
+ Red Hat Enterprise Linux 7
+ OpenSUSE 42\.3
+ Fedora 25 and 26
+ Snap
+ Raspbian Stretch
+ macOS 10\.12

Some Linux\-based operating systems, such as Arch, Kali, and Raspbian, are not officially supported, but have community support\.

After you install PowerShell Core, you can find the AWS Tools for PowerShell Core on Microsoft's [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell.NetCore) website\. The simplest way to install the AWS Tools for PowerShell Core is by running the `Install-Module` cmdlet\. First, start your PowerShell session by running `pwsh` in a shell\.

**Note**  
Although you can start PowerShell by running `sudo pwsh` to run PowerShell with elevated rights, be aware that this is a potential security risk, and not consistent with the principle of least privilege\.

Next, run `Install-Module` as shown in the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore -AllowClobber
```

It is not necessary to run this command as Administrator, unless you want to install the AWS Tools for PowerShell Core for all users of a computer\. To do this, run the following command in a PowerShell session that you have started with `sudo pwsh`:

```
PS > Install-Module -Scope CurrentUser -Name AWSPowerShell.NetCore -Force
```

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, [Introducing AWS Tools for PowerShell Core Edition](https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition)\.

## Installation Troubleshooting Tips<a name="installation-troubleshooting-tips"></a>

Some users have reported issues with the `Install-Module` cmdlet that is included with older releases of PowerShell Core, including errors related to semantic versioning \(see [https://github\.com/OneGet/oneget/issues/202](https://github.com/OneGet/oneget/issues/202)\)\. Using the NuGet provider appears to resolve the issue\. Newer versions of PowerShell Core have resolved this issue\.

To install AWS Tools for PowerShell Core by using NuGet, run the following command\. Specify an appropriate destination folder \(on Linux, try `-Destination ~/.local/share/powershell/Modules`\)\.

```
PS > Install-Package `
               -Name AWSPowerShell.NetCore `
               -Source https://www.powershellgallery.com/api/v2/ `
               -ProviderName NuGet `
               -ExcludeVersion `
               -Destination ~/.local/share/powershell/Modules
```

## Script Execution<a name="enable-script-execution"></a>

The `Set-ExecutionPolicy` command is not available on non\-Windows systems\. You can run `Get-ExecutionPolicy`, which shows that the default execution policy setting in PowerShell Core running on non\-Windows systems is `Unrestricted`\. For more information about execution policies, see [About Execution Policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-5.1) on the Microsoft Technet website\.

The AWS Tools installer updates the [PSModulePath](http://msdn.microsoft.com/en-us/library/windows/desktop/dd878326.aspx) to include the location of the directory that contains the AWSPowerShell\.NetCore module\.

Because the `PSModulePath` includes the location of the AWS module's directory, the `Get-Module -ListAvailable` cmdlet shows the module\.

```
PS > Get-Module -ListAvailable

Directory: /home/ubuntu/.local/share/powershell/Modules

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Binary     3.3.219.0  AWSPowerShell.NetCore               {Add-AASScalableTarget, Add-ACMCertificateTag, Add-ADSC...
```

## Configure a PowerShell Console to Use the AWS Tools for PowerShell Core<a name="pstools-config-ps-window"></a>

Although PowerShell 3\.0 and newer releases typically load modules automatically whenever you run a cmdlet in the module, this does not work for AWS Tools for PowerShell, because of its large size\. To start running AWS Tools for PowerShell cmdlets, run the command: `Import-Module AWSPowerShell.NetCore`\.

When you start PowerShell on a Linux\-based system after you have installed the AWS Tools for PowerShell Core, run [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) to specify your AWS access and secret keys\. For more information about `Initialize-AWSDefaultConfiguration`, see [Using AWS Credentials](specifying-your-aws-credentials.md)\. In older \(before 3\.3\.96\.0\) releases of the AWS Tools for PowerShell, this cmdlet was named `Initialize-AWSDefaults`\.

## Versioning<a name="pstools-versioning"></a>

AWS releases new versions of the AWS Tools for PowerShell and AWS Tools for PowerShell Core periodically to support new AWS services and features\. To determine the version of the Tools that you have installed, run the [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet:

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

You can also add the `-ListServiceVersionInfo` parameter to a [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) command to see a list of which AWS services are supported in the current version of the tools\.

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
...
```

To determine the version of PowerShell that you are running, type `$PSVersionTable` to view the contents of the $PSVersionTable [automatic variable](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6)\.

```
PS > $PSVersionTable

Name                           Value
----                           -----
PSVersion                      6.2.2
PSEdition                      Core
GitCommitId                    6.2.2
OS                             Darwin 18.7.0 Darwin Kernel Version 18.7.0: Thu Jun 20 18:42:21 PDT 2019; root:xnu-4903.270.47~4/RELEASE_X86_64
Platform                       Unix
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

## Updating the AWS Tools for PowerShell Core<a name="pstools-updating-core"></a>

Periodically, as updated versions of the AWS Tools for PowerShell Core are released, you should update the version that you are running locally\. Run the `Get-AWSPowerShellVersion` cmdlet to determine the version that you are running, and compare that with the version of AWS Tools for PowerShell Core that is available at [AWS Tools for Windows PowerShell](https://aws.amazon.com/powershell/) or on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell.NetCore) website\. A suggested time period for checking for an updated AWS Tools for PowerShell package is every two to three weeks\.

### Update the Tools for PowerShell Core \(All systems\)<a name="update-the-tpc-all-systems"></a>

Before you install a newer release of the AWS Tools for PowerShell Core, close any open PowerShell or AWS Tools for PowerShell Core sessions before you uninstall the existing Tools for PowerShell Core package\. You can exit a PowerShell session on a Linux\-based system by pressing **Ctrl\+D**\. Run the following command to uninstall the package\.

```
PS > Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions
```

When uninstallation is finished, install the updated module by running the following command\. By default, this command installs the latest version of the Tools for PowerShell Core\. This module is available on the [PowerShell Gallery](https://www.powershellgallery.com/packages/AWSPowerShell.NetCore), but the easiest method of installation is to run `Install-Module`\.

```
PS > Install-Module -Name AWSPowerShell.NetCore
```

After you install the module, run the command `Import-Module AWSPowerShell.NetCore` to load the AWS Tools for PowerShell Core cmdlets into your PowerShell session\.

## See Also<a name="pstools-seealso-setup"></a>
+  [Getting Started with the AWS Tools for Windows PowerShell](pstools-getting-started.md) 
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [AWS Account and Access Keys](pstools-appendix-sign-up.md) 