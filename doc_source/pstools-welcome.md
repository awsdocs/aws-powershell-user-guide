# What Are the AWS Tools for PowerShell?<a name="pstools-welcome"></a>

The AWS Tools for PowerShell are a set of PowerShell modules that are built on the functionality exposed by the AWS SDK for \.NET\. The AWS Tools for PowerShell enable you to script operations on your AWS resources from the PowerShell command line\. 

The cmdlets provide an idiomatic PowerShell experience for specifying parameters and handling results even though they are implemented using the various AWS service HTTP query APIs\. For example, the cmdlets for the AWS Tools for PowerShell support PowerShell pipelining—that is, you can pipe PowerShell objects in and out of the cmdlets\.

The AWS Tools for PowerShell are flexible in how they enable you to handle credentials, including support for the AWS Identity and Access Management \(IAM\) infrastructure\. You can use the tools with IAM user credentials, temporary security tokens, and IAM roles\.

The AWS Tools for PowerShell support the same set of services and AWS Regions that are supported by the SDK\. You can install the AWS Tools for PowerShell on computers running Windows, Linux, or macOS operating systems\.

**Note**  
AWS Tools for PowerShell version 4 is the latest major release, and is a backward\-compatible update to AWS Tools for PowerShell version 3\.3\. It adds significant improvements while maintaining existing cmdlet behavior\. Your existing scripts should continue to work after upgrading to the new version, but we do recommend that you test them thoroughly before upgrading\. For more information about the changes in version 4, see [Migrating from AWS Tools for PowerShell Version 3\.3 to Version 4](v4migration.md)\.

The AWS Tools for PowerShell are available as the following three distinct packages:
+ [AWS\.Tools](#pwsh_structure_pstools)
+ [AWSPowerShell\.NetCore](#pwsh_structure_pscore)
+ [AWSPowerShell](#pwsh_structure_psoldwin)

## AWS\.Tools \- A Modularized Version of AWS Tools for PowerShell<a name="pwsh_structure_pstools"></a>

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell%20Gallery-AWS.Tools-blue.png) ](https://www.powershellgallery.com/packages/AWS.Tools.Common)

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/ZIP%20Archive-AWS.Tools-yellow.png) ](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWS.Tools.zip)

This version of AWS Tools for PowerShell is the recommended version for any computer running PowerShell in a production environment\. Because it's modularized, you need to download and load only the modules for the services you want to use\. This reduces download times, memory usage, and enables auto\-importing of AWS\.Tools cmdlets with the need to manually call `Import-Module` first\.

This is the latest version of AWS Tools for PowerShell and runs on all supported operating systems, including Windows, Linux, and macOS\. This package provides one installation module, `AWS.Tools.Installer`, one common module, `AWS.Tools.Common`, and one module for each AWS service, for example, `AWS.Tools.EC2`, `AWS.Tools.IAM`, `AWS.Tools.S3`, and so on\.

The `AWS.Tools.Installer` module provides cmdlets that enable you to install, update, and remove the modules for each of the AWS services\. The cmdlets in this module automatically ensure that you have all the dependent modules required to support the modules you want to use\.

The `AWS.Tools.Common` module provides cmdlets for configuration and authentication that are not service specific\. To use the cmdlets for an AWS service, you just run the command\. PowerShell automatically imports the `AWS.Tools.Common` module and the module for the AWS service whose cmdlet you want to run\. This module is automatically installed if you use the `AWS.Tools.Installer` module to install the service modules\.

You can install this version of AWS Tools for PowerShell on computers that are running:
+ PowerShell Core 6\.0 or later on Windows, Linux, or macOS\.
+ Windows PowerShell 5\.1 or later on Windows with the \.NET Framework 4\.7\.2 or later\.

Throughout this guide, when we need to specify this version only, we refer to it by its module name: *AWS\.Tools*\.

## AWSPowerShell\.NetCore \- A Single\-Module Version of AWS Tools for PowerShell<a name="pwsh_structure_pscore"></a>

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell-Gallery-AWSPowerShell.NetCore-blue.png) ](https://www.powershellgallery.com/packages/AWSPowerShell.NetCore/)

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/ZIP%20Archive-AWSPowerShell.NetCore-yellow.png) ](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWSPowerShell.NetCore.zip)

This version consists of a single, large module that contains support for all AWS services\. Before you can use this module, you must manually import it\.

You can install this version of AWS Tools for PowerShell on computers that are running:
+ PowerShell Core 6\.0 or later on Windows, Linux, or macOS\.
+ Windows PowerShell 3\.0 or later on Windows with the \.NET Framework 4\.7\.2 or later\.

Throughout this guide, when we need to specify this version only, we refer to it by its module name: *AWSPowerShell\.NetCore*\.

## AWSPowerShell \- A Single\-Module Version for Windows PowerShell<a name="pwsh_structure_psoldwin"></a>

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell-Gallery-AWSPowerShell-blue.png) ](https://www.powershellgallery.com/packages/AWSPowerShell/)

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/ZIP%20Archive-AWSPowerShell-yellow.png) ](https://sdk-for-net.amazonwebservices.com/ps/v4/latest/AWSPowerShell.zip)

This version of AWS Tools for PowerShell is compatible with and installable on only Windows computers that are running Windows PowerShell versions 2\.0 through 5\.1\. It is not compatible with PowerShell Core 6\.0 or later, or any other operating system \(Linux or macOS\)\. This version consists of a single, large module that contains support for all AWS services\.

Throughout this guide, when we need to specify this version only, we refer to it by its module name: *AWSPowerShell*\.

## How to Use This Guide<a name="how-to-use-this-guide"></a>

The guide is divided into the following major sections\.

** [Installing the AWS Tools for PowerShell](pstools-getting-set-up.md) **  
This section explains how to install the AWS Tools for PowerShell\. It includes how to sign up for AWS if you don't already have an account, and how to create an IAM user that you can use to run the cmdlets\.

** [Getting Started with the AWS Tools for Windows PowerShell](pstools-getting-started.md) **  
This section describes the fundamentals of using the AWS Tools for PowerShell, such as specifying credentials and AWS Regions, finding cmdlets for a particular service, and using aliases for cmdlets\.

** [Using the AWS Tools for Windows PowerShell](pstools-using.md) **  
This section includes information about using the AWS Tools for PowerShell to perform some of the most common AWS tasks\.