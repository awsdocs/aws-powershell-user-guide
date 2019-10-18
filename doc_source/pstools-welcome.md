# What Are the AWS Tools for PowerShell?<a name="pstools-welcome"></a>

The AWS Tools for PowerShell are a set of PowerShell modules that are built on the functionality exposed by the AWS SDK for \.NET\. The AWS Tools for PowerShell enable you to script operations on your AWS resources from the PowerShell command line\. 

The cmdlets provide an idiomatic PowerShell experience for specifying parameters and handling results even though they are implemented using the various AWS service HTTP query APIs\. For example, the cmdlets for the AWS Tools for PowerShell support PowerShell pipelining—that is, you can pipe PowerShell objects in and out of the cmdlets\.

The AWS Tools for PowerShell are flexible in how they enable you to handle credentials, including support for the AWS Identity and Access Management \(IAM\) infrastructure\. You can use the tools with IAM user credentials, temporary security tokens, and IAM roles\.

The AWS Tools for PowerShell support the same set of services and AWS Regions that are supported by the SDK\. You can install the AWS Tools for PowerShell on computers running Windows, Linux, or macOS operating systems\.

The AWS Tools for PowerShell are available as the following three distinct packages:
+ [AWS\.Tools](#pwsh_structure_pstools)
+ [AWSPowerShell\.NetCore](#pwsh_structure_pscore)
+ [AWSPowerShell](#pwsh_structure_psoldwin)

## AWS\.Tools \- A Modularized Version of AWS Tools for PowerShell<a name="pwsh_structure_pstools"></a>

**Prerelease Evaluation Software**  
This version of the product is provided as prerelease software for testing and evaluation\. At this time, we recommend that you do not use this in a production environment\. For production environments, we recommend that you use the generally available AWSPowerShell\.NetCore\.  
You can provide feedback for this prerelease version at [https://github\.com/aws/aws\-tools\-for\-powershell/issues](https://github.com/aws/aws-tools-for-powershell/issues)\. 

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell%2520Gallery-AWS.Tools.Common-blue.svg) ](https://www.powershellgallery.com/packages/AWS.Tools.Common)

This is the latest version of AWS Tools for PowerShell and runs on all supported operating systems, including Windows, Linux, and macOS\. This package provides one common module, `AWS.Tools.Common`, and one module for each AWS service, for example: `AWS.Tools.EC2`, `AWS.Tools.IAM`, `AWS.Tools.S3`, and so on\.

The `AWS.Tools.Common` module provides cmdlets for configuration and authentication that are not service specific\. To use the cmdlets for an AWS service, you just run the command\. PowerShell automatically imports the `AWS.Tools.Common` module and the module for the AWS service whose cmdlet you want to run\.

You can install this version of AWS Tools for PowerShell on computers that are running:
+ PowerShell Core 6\.0 or later on Windows, Linux, or macOS\.
+ Windows PowerShell 3\.0 or later on Windows with the \.NET Framework 4\.7\.2 or later\.

Throughout this guide, when we need to specify this version only, we refer to it by its module name: *AWS\.Tools*\.

## AWSPowerShell\.NetCore \- A Single\-Module Version of AWS Tools for PowerShell<a name="pwsh_structure_pscore"></a>

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell%2520Gallery-AWSPowerShell.Netcore-blue.svg) ](https://www.powershellgallery.com/packages/AWSPowerShell.NetCore/)

This version of AWS Tools for PowerShell is the recommended version for any computer running PowerShell in a production environment\. This version consists of a single, large module that contains support for all AWS services\. Before you can use this module, you must manually import it\.

You can install this version of AWS Tools for PowerShell on computers that are running:
+ PowerShell Core 6\.0 or later on Windows, Linux, or macOS\.
+ Windows PowerShell 3\.0 or later on Windows with the \.NET Framework 4\.7\.2 or later\.

Throughout this guide, when we need to specify this version only, we refer to it by its module name: *AWSPowerShell\.NetCore*\.

## AWSPowerShell \- A Single\-Module Version for Windows PowerShell<a name="pwsh_structure_psoldwin"></a>

[ ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/powershell/latest/userguide/images/PowerShell%2520Gallery-AWSPowerShell-blue.svg) ](https://www.powershellgallery.com/packages/AWSPowerShell/)

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