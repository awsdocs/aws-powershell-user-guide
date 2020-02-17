# Installing AWS Tools for PowerShell on Linux or macOS<a name="pstools-getting-set-up-linux-mac"></a>

This topic provides instructions on how to install the AWS Tools for PowerShell on Linux or macOS\.

## Overview of Setup<a name="pstools-installing-core-prerequisites"></a>

To install AWS Tools for PowerShell on a Linux or macOS computer, you can choose from two package options:
+ [**AWS\.Tools**](#install-aws.tools-on-linux-macos) – The modularized version of AWS Tools for PowerShell\. Each AWS service is supported by its own individual, small module, with shared support modules `AWS.Tools.Common`\.
+ [**AWSPowerShell\.NetCore**](#install-netcore-on-linux-macos) – The single, large\-module version of AWS Tools for PowerShell\. All AWS services are supported by this single, large module\.

 Setting either of these up on a computer running Linux or macOS involves the following tasks, described in detail later in this topic:

1. Install PowerShell Core 6\.0 or later on a supported system\.

1. After installing PowerShell Core, start PowerShell by running `pwsh` in your system shell\.

1. Install either AWS\.Tools or AWSPowerShell\.NetCore\.

1. Run the appropriate `Import-Module` cmdlet to import the module into your PowerShell session\.

1. Run the [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) cmdlet to provide your AWS credentials\.

## Prerequisites<a name="prerequisites"></a>

Ensure that you meet the requirements listed on [Prerequisites for Setting up the AWS Tools for PowerShell](pstools-getting-set-up-prereq.md)\.

To run the AWS Tools for PowerShell Core, your computer must be running PowerShell Core 6\.0 or later\. 
+ For a list of the supported Linux versions and for information about how to install PowerShell Core 6\.0 or later on a Linux\-based computer, see [Installing PowerShell Core on Linux](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux?view=powershell-6) on Microsoft's website\. Some Linux\-based operating systems, such as Arch, Kali, and Raspbian, are not officially supported, but have varying levels of community support\.
+ For a list of supported macOS versions and for information about how to install PowerShell Core 6\.0 on macOS 10\.12 or later, see [Installing PowerShell Core on macOS](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-macos) on Microsoft's website\.

## Install AWS\.Tools on Linux or macOS<a name="install-aws.tools-on-linux-macos"></a>

You can install the modularized version of AWS Tools for PowerShell on computers that are running PowerShell Core 6\.0 or later\. For information about how to install PowerShell Core, see [https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell](https://docs.microsoft.com/en-us/powershell/scripting/install/installing-powershell)\. 

You can install AWS\.Tools in one of three ways:
+ Using the cmdlets in the `AWS.Tools.Installer` module\. The `AWS.Tools.Installer` module simplifies the installation and update of other AWS\.Tools modules\. `AWS.Tools.Installer` requires, and automatically downloads and installs, an updated version of `PowerShellGet`\. The `AWS.Tools.Installer` module also automatically keeps your module versions in sync\. When you install or update to a newer version of one module, the cmdlets in the `AWS.Tools.Installer` automatically update all of your other AWS\.Tools modules to the same version\.
+ Installing each service module from PowerShell Gallery using the `Install-Module` cmdlet\.
+ Downloading the modules as a [ZIP archive](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWS.Tools.zip) and extracting them in one of the module directories\. You can discover your module directories by printing the value of the `$Env:PSModulePath` variable.

**To install AWS\.Tools on Linux or macOS**

1. Start a PowerShell Core session by running the following command\.

   ```
   $ pwsh
   ```
**Note**  
We recommend that you *don't* run PowerShell as an administrator with elevated permissions except when required by the task at hand\. This is because of the potential security risk and is inconsistent with the principle of least privilege\.

1. To install the modularized AWS\.Tools package using the AWS\.Tools\.Installer module, run the following command\.

   ```
   PS > Install-Module -Name AWS.Tools.Installer
   
   Untrusted repository
   You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
    you want to install the modules from 'PSGallery'?
   [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
   ```

   If you are notified that the repository is "untrusted", you're asked if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. To avoid the prompt and install the module without trusting the repository, you can run the following command\.

   ```
   PS > Install-Module -Name AWS.Tools.Installer -Force
   ```

1. You can now install the module for each service that you want to use\. For example, the following command installs the IAM module\. This command also installs any dependent modules that are required for the specified module to work\. For example, when you install your first AWS\.Tools service module, it also installs `AWS.Tools.Common`\. This is a shared module required by all AWS service modules\. It also removes older versions of the modules, and updates other modules to the same newer version\.

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
The `Install-AWSToolsModule` cmdlet downloads all requested modules from the `PSRepository` named `PSGallery` \([https://www\.powershellgallery\.com/](https://www.powershellgallery.com/)\) and considers the repository as a trusted source\. Use the command `Get-PSRepository -Name PSGallery` for more information about this `PSRepository`\.

   By default, this installs modules into the `$home\Documents\PowerShell\Modules` folder\. To install the AWS\.Tools module for all users of a computer, you must run the following command in a PowerShell session that you started as an administrator\. This installs modules to the `$env:ProgramFiles\PowerShell\Modules` folder that is accessible by all users\.

   ```
   PS > Install-AWSToolsModule -Name AWS.Tools.IdentityManagement -Scope AllUsers
   ```

## Install AWSPowerShell\.NetCore on Linux or macOS<a name="install-netcore-on-linux-macos"></a>

You can install AWSPowerShell\.NetCore in one of two ways:
+ Installing from PowerShell Gallery using the `Install-Module` cmdlet\.
+ Downloading the module as a [ZIP archive](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWSPowerShell.NetCore.zip) and extracting it in one of the module directories\. You can discover your module directories by printing the value of the `$Env:PSModulePath` variable.

To upgrade to a newer release of AWSPowerShell\.NetCore, follow the instructions in [Updating the AWS Tools for PowerShell on Linux or macOS](#pstools-updating-linux)\. Uninstall earlier versions of AWSPowerShell\.NetCore first\.

Start a PowerShell Core session by running the following command\.

```
$ pwsh
```

**Note**  
We recommend that you *don't* start PowerShell by running `sudo pwsh` to run PowerShell with elevated, administrator rights\. This is because of the potential security risk and is inconsistent with the principle of least privilege\.

To install the AWSPowerShell\.NetCore single\-module package from PowerShell Gallery, run the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore

Untrusted repository
You are installing the modules from an untrusted repository. If you trust this repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet. Are you sure
 you want to install the modules from 'PSGallery'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
```

If you are notified that the repository is "untrusted", you're asked if you want to install anyway\. Enter **y** to allow PowerShell to install the module\. To avoid the prompt without trusting the repository, you can run the following command\.

```
PS > Install-Module -Name AWSPowerShell.NetCore -Force
```

You don't have to run this command as root, unless you want to install the AWS Tools for PowerShell for all users of a computer\. To do this, run the following command in a PowerShell session that you have started with `sudo pwsh`\.

```
PS > Install-Module -Scope AllUsers -Name AWSPowerShell.NetCore -Force
```

## Script Execution<a name="enable-script-execution"></a>

The `Set-ExecutionPolicy` command isn't available on non\-Windows systems\. You can run `Get-ExecutionPolicy`, which shows that the default execution policy setting in PowerShell Core running on non\-Windows systems is `Unrestricted`\. For more information, see [About Execution Policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-5.1) on the Microsoft Technet website\.

 Because the `PSModulePath` includes the location of the AWS module's directory, the `Get-Module -ListAvailable` cmdlet shows the module that you installed\.

**AWS\.Tools**

```
PS > Get-Module -ListAvailable

    Directory: /Users/username/.local/share/powershell/Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Binary     3.3.563.1  AWS.Tools.Common                    Desk      {Clear-AWSHistory, Set-AWSHistoryConfiguration, Initialize-AWSDefaultConfiguration, Clear-AWSDefaultConfigurat…
```

**AWSPowerShell\.NetCore**

```
PS > Get-Module -ListAvailable

Directory: /Users/username/.local/share/powershell/Modules

ModuleType Version    Name                                ExportedCommands
---------- -------    ----                                ----------------
Binary     3.3.563.1  AWSPowerShell.NetCore
```

## Configure a PowerShell Console to Use the AWS Tools for PowerShell Core \(AWSPowerShell\.NetCore Only\)<a name="pstools-config-ps-window"></a>

PowerShell Core typically loads modules automatically whenever you run a cmdlet in the module\. But this doesn't work for AWSPowerShell\.NetCore because of its large size\. To start running AWSPowerShell\.NetCore cmdlets, you must first run the `Import-Module AWSPowerShell.NetCore` command\. This isn't required for cmdlets in AWS\.Tools modules\.

## Initialize Your PowerShell Session<a name="linux-config-init"></a>

When you start PowerShell on a Linux\-based or macOS\-based system after you have installed the AWS Tools for PowerShell, you must run [Initialize\-AWSDefaultConfiguration](https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html) to specify which AWS access key to use\. For more information about `Initialize-AWSDefaultConfiguration`, see [Using AWS Credentials](specifying-your-aws-credentials.md)\. 

**Note**  
In earlier \(before 3\.3\.96\.0\) releases of the AWS Tools for PowerShell, this cmdlet was named `Initialize-AWSDefaults`\.

## Versioning<a name="pstools-versioning"></a>

AWS releases new versions of the AWS Tools for PowerShell periodically to support new AWS services and features\. To determine the version of the AWS Tools for PowerShell that you have installed, run the [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet\.

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

To see a list of the supported AWS services in the current version of the tools, add the `-ListServiceVersionInfo` parameter to a [Get\-AWSPowerShellVersion](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html) cmdlet\.

```
PS > Get-AWSPowerShellVersion -ListServiceVersionInfo

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

To determine the version of PowerShell that you are running, enter `$PSVersionTable` to view the contents of the `$PSVersionTable` [automatic variable](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6)\.

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

### Update the Modularized AWS\.Tools\.\*<a name="update-aws.tools-all-systems"></a>

To upgrade your AWS\.Tools modules to the latest version, run the following command\.

```
PS > Update-AWSToolsModule -CleanUp
```

This command updates all of the currently installed `AWS.Tools` modules and, for those modules that were successfully updated, removes the earlier versions\.

**Note**  
The `Update-AWSToolsModule` cmdlet downloads all modules from the `PSRepository` named `PSGallery` \([https://www\.powershellgallery\.com/](https://www.powershellgallery.com/)\) and considers it a trusted source\. Use the command `Get-PSRepository -Name PSGallery` for more information about this `PSRepository`\.

### Update the Tools for PowerShell Core<a name="update-netcore-all-systems"></a>

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

## Related Information<a name="pstools-seealso-setup"></a>
+  [Getting Started with the AWS Tools for Windows PowerShell](pstools-getting-started.md) 
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [AWS Account and Access Keys](pstools-appendix-sign-up.md) 