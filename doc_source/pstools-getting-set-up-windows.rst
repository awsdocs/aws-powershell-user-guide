.. _pstools-getting-set-up-windows:

###################################################################
Setting up the AWS Tools for PowerShell on a Windows-based Computer
###################################################################

.. _pstools-installing-windows-prerequisites:

Overview of Setup
=================

A Windows-based computer can run the AWS Tools for PowerShell, the AWS Tools for PowerShell Core, or both, depending on the release and edition of Windows that you are running. Setting up the AWS Tools for PowerShell or AWS Tools for PowerShell Core involves the following tasks, described in this topic.

#. Installing Windows PowerShell 2.0 or newer (Microsoft PowerShell Core 6.0 or newer if you are installing the AWS Tools for PowerShell Core).
#. After installing PowerShell, either downloading and running the |TWPlong| MSI installer, or starting PowerShell.
#. If you did not run the MSI installer, running :code:`Install-Module` in a PowerShell session to install the AWS Tools for PowerShell or PowerShell Core.
#. Verifying that script execution is enabled by running the :code:`Get-ExecutionPolicy` cmdlet.
#. If you are not running the custom AWS Tools for PowerShell console, or running Windows PowerShell 3.0 or newer, explicitly loading the AWS Tools for PowerShell module into your PowerShell session by running an :code:`Import-Module AWSPowerShell` command.

Prerequisites
=============


To use the |TWPlong| or the AWS Tools for PowerShell Core, you must have an AWS account. If you do not yet have an AWS account, see
:ref:`pstools-appendix-sign-up` for instructions.

To use the |TWPlong|, your system must meet the following prerequisites.

* Microsoft Windows XP or later

* Windows PowerShell 2.0 or later (PowerShell Core 6.0 or later for the Tools for PowerShell Core).

The following table shows the versions of PowerShell that are installed on Windows releases by default.

+----------------------------------------+------------------------------+
| Windows Release                        | Included PowerShell Release  |
+========================================+==============================+
| Windows 7 and Windows Server 2008 R2   | Windows PowerShell 2.0       |
+----------------------------------------+------------------------------+
| Windows 8 and Windows Server 2012      | Windows PowerShell 3.0       |
+----------------------------------------+------------------------------+
| Windows 8.1 and Windows Server 2012 R2 | Windows PowerShell 4.0       |
+----------------------------------------+------------------------------+
| Windows 10 and Windows Server 2016     | Windows PowerShell 5.0       |
+----------------------------------------+------------------------------+
| Windows 10 and Windows Server 2016     |                              |
| January 2017 Anniversary Update        | Windows PowerShell 5.1       |
+----------------------------------------+------------------------------+

Server Core installation options for the preceding server releases include PowerShell.
The Nano Server installation option of Windows Server 2016 includes PowerShell Core.
For earlier releases of Windows, such as Windows XP, Windows Vista, Windows Server 2003, and Windows Server 2008, 
you can get PowerShell 2.0 by installing the Windows Management Framework.

* `Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0, and BITS 4.0)
  <http://support.microsoft.com/kb/968929>`_
  

.. _pstools-installing-download:

Install the AWS Tools for PowerShell on a Windows-based Computer
================================================================

To upgrade to a newer release of the AWS Tools for PowerShell, follow instructions in pstools-updating_. Uninstall older versions of PowerShell first.

The |TWPlong| is one of the optional components that you can install by running the AWS Tools for
Windows installer :file:`.msi`. Download the installer by opening the following webpage, and then
choosing :guilabel:`AWS Tools for Windows`.

* http://aws.amazon.com/powershell/

The installer for the |TWP| installs the most recent versions of the |sdk-net| assemblies for the .NET 3.5 and 4.5 Frameworks. 
If you have Microsoft Visual Studio 2013 or 2015 installed, the installer can also install the :tvs-ug:`AWS Toolkit for Visual Studio <welcome>`.

Users who are running PowerShell 5.0 or newer can also install and update the |TWP| from Microsoft's `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell>`_ website by running the following command.

.. code-block:: none

    PS C:\> Install-Module -Name AWSPowerShell
    

If you are running PowerShell 3.0 or newer, and add the module installation path to the value of the 
:code:`PSModulePath` environment variable, the |TWP| are automatically loaded into your session when you run any 
|TWP| cmdlet or function.

The |TWP| are installed by default on all Windows Amazon Machine Images (AMIs).

Install the AWS Tools for PowerShell Core on a Windows-based Computer
=====================================================================

You can install the AWS Tools for PowerShell Core on computers that are running Microsoft PowerShell Core 6.0 or newer.
AWS Tools for PowerShell Core is supported on the following Windows-based operating systems.

* Windows 8.1 Enterprise
* Windows Server 2012 R2
* Windows 10 for Business or Windows 10 Pro
* Windows Server 2016


For more information about how to install PowerShell Core on computers that run Windows 8.1 or Windows 10, see `Package installation instructions (Windows) 
<https://github.com/PowerShell/PowerShell/blob/master/docs/installation/windows.md>`_, also in the GitHub repository for PowerShell.

After you install PowerShell Core, you can find the AWS Tools for PowerShell Core on 
Microsoft's `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website.
The simplest way to install the Tools for PowerShell Core is by running the :code:`Install-Module` cmdlet.

.. code-block:: none

    PS C:\> Install-Module -Name AWSPowerShell.NetCore -AllowClobber

It is not necessary to run this command as Administrator, unless you want to install the AWS Tools for PowerShell Core for all users of a computer. To do this, run the following command in a PowerShell session that is running as Administrator:

.. code-block:: none

    PS C:\> Install-Module -Scope CurrentUser -Name AWSPowerShell.NetCore -Force

To install both AWSPowerShell and AWSPowerShell.NetCore on a Windows-based computer, add :code:`-AllowClobber` to the second installation command, because the modules have cmdlets with the same names. 

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, `Introducing AWS Tools for PowerShell Core Edition <https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition>`_.

Installation Troubleshooting Tips
=================================

Some users have reported issues with the Install-Module cmdlet that is included with older releases of PowerShell Core, including errors 
related to semantic versioning (see https://github.com/OneGet/oneget/issues/202). Using the NuGet provider appears to 
resolve the issue. Newer versions of PowerShell Core have resolved this issue.

To install AWS Tools for PowerShell Core by using NuGet, run the following command. Specify an appropriate destination folder (on Linux, try -Destination ~/.local/share/powershell/Modules):

.. code-block:: none

    PS C:\> Install-Package -Name AWSPowerShell.NetCore -Source
    https://www.powershellgallery.com/api/v2/ -ProviderName NuGet -ExcludeVersion
    -Destination <path to destination folder>


.. _enable-script-execution:

Enable Script Execution
=======================

To load the |TWPlong| or AWS Tools for PowerShell Core modules, enable PowerShell script execution if you have not already done so. To
enable script execution, run the :code:`Set-ExecutionPolicy` cmdlet to set a policy of
:code:`RemoteSigned`. By default, PowerShell script execution policy is set to :code:`Restricted`. For more
information about execution policies, see `About Execution Policies
<https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-5.1>`_ on the Microsoft Technet website.

**To enable script execution**

1. Administrator rights are required to set the execution policy. If you are not logged on as a user
   with administrator rights, open a PowerShell session as Administrator by doing the following: Click :guilabel:`Start`
   and then click :guilabel:`All Programs`. Click :guilabel:`Accessories`, and then click
   :guilabel:`Windows PowerShell`. Right-click :guilabel:`Windows PowerShell`, and then choose
   :guilabel:`Run as administrator` from the context menu.

2. At the command prompt, type: :code:`Set-ExecutionPolicy RemoteSigned`

.. note:: On a 64-bit system, you must also do this for the 32-bit version of PowerShell,
   **Windows PowerShell (x86)**.

If you do not have the execution policy set correctly, PowerShell shows the following error.

.. code-block:: none

    File C:\Users\username\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 cannot be loaded because the execution
     of scripts is disabled on this system. Please see "get-help about_signing" for more details.
    At line:1 char:2
    + . <<<<  'C:\Users\username\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'
        + CategoryInfo          : NotSpecified: (:) [], PSSecurityException
        + FullyQualifiedErrorId : RuntimeException

The |TWP| installer updates the `PSModulePath
<http://msdn.microsoft.com/en-us/library/windows/desktop/dd878326.aspx>`_ to include the location of
the directory that contains the AWSPowerShell module. If you are running PowerShell
3.0 or newer, the AWSPowerShell module is loaded automatically whenever you run one of the AWS cmdlets. This
lets you use the AWS cmdlets even if the execution policy on your system is set to
disallow script execution.

Because the :code:`PSModulePath` includes the location of the AWS module's directory, the
:code:`Get-Module -ListAvailable` cmdlet shows the module.

.. code-block:: none

    PS C:\> Get-Module -ListAvailable

    ModuleType Name                      ExportedCommands
    ---------- ----                      ----------------
    Manifest   AppLocker                 {}
    Manifest   BitsTransfer              {}
    Manifest   PSDiagnostics             {}
    Manifest   TroubleshootingPack       {}
    Manifest   AWSPowerShell             {Update-EBApplicationVersion, Set-DPStatus, Remove-IAMGroupPol...


.. _pstools-config-ps-window:

Configure a PowerShell Console to Use the |TWPlong|
===================================================

The installer creates a :guilabel:`Start Menu` group called :guilabel:`Amazon Web Services`, which
contains a shortcut called :guilabel:`Windows PowerShell for AWS`. In PowerShell 2.0, this shortcut
automatically imports the AWSPowerShell module and runs the :code:`Initialize-AWSDefaultConfiguration`
cmdlet for you. Because PowerShell 3.0 and newer automatically load the AWSPowerShell module whenever you run an AWS
cmdlet, in PowerShell 3.0 and newer, the shortcut created by the AWS Tools for PowerShell installer runs only the
:code:`Initialize-AWSDefaultConfiguration` cmdlet. For more information about :code:`Initialize-AWSDefaultConfiguration`,
see :ref:`specifying-your-aws-credentials`. In older (before 3.3.96.0) releases of the |TWP|, this cmdlet was named
:code:`Initialize-AWSDefaults`.

The installer creates another shortcut titled :guilabel:`AWS Tools for Windows`, which
opens a visual display of AWS resources for Windows developers.

If you run PowerShell 3.0 or newer, or if you only use the custom-console shortcut that is installed by the installer, there is no 
need to configure a PowerShell window to use the |TWPlong|. But if you run 
PowerShell 2.0 with a specially-configured PowerShell console, and you want to add support for the
AWS Tools for PowerShell, you must load the AWS module manually by running :code:`Import-Module` as described in the following sections.

.. _pstools-installing-integration:

How to Load the |TWPlong| Module (PowerShell 2.0)
-------------------------------------------------

**To load the Powershell Tools module into your current session**

1. Open a PowerShell session, type the following command, and press Enter.

    .. code-block:: none

        PS C:\> Import-Module "C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowerShell\AWSPowerShell.psd1"

    .. note:: In PowerShell 4.0 and later, Import-Module also searches the Program Files folder for
       installed modules, so it is not necessary to provide the full path to the module. You can
       run the following command to import the AWSPowerShell module. In PowerShell 3.0 and later,
       running a cmdlet in the module also automatically imports a module into your session.

        .. code-block:: none

            PS C:\> Import-Module AWSPowerShell

2. To verify that the module was loaded, type the following command:

   .. code-block:: none

      PS C:\> Get-Module

   Look for an entry in the list named **AWSPowerShell** to verify that the |TWP| module was loaded
   successfully.

    .. code-block:: none

       ModuleType Version   Name           ExportedCommands
       ---------- -------   ----           ----------------
       Binary     3.3.96.0  AWSPowerShell  {Add-AASScalableTarget, Add-ACMCertificateTag, Add-ADSConfigurationItemsToApplication, Add-ASAAttachmentsToSet...}
       ...


.. _pstools-installing-integration-profile:

Load the |TWPlong| Module into Every Session (PowerShell 2.0)
-------------------------------------------------------------

To load the AWSPowerShell module automatically every time you start a PowerShell session, add it to
your PowerShell profile. Note, however, that adding commands to your PowerShell profile can slow
the startup of PowerShell.

The PowerShell :code:`$profile` variable stores the full path to the text file containing your
PowerShell profile. This variable is available only in a PowerShell session; it is not a Windows
environment variable. To view the value of this variable, run :code:`echo`.

.. code-block:: none

   echo $profile C:\Users\{username}\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

You can edit this file with any text editor, such as notepad.exe.

.. code-block:: none

   notepad $profile

You might need to create both the profile directory and the profile itself, if they do not already
exist.



.. _pstools-versioning:

Versioning
==========

AWS releases new versions of the AWS Tools for PowerShell and AWS Tools for PowerShell Core periodically to support new AWS services and features. To determine 
the version of the Tools that you have installed, run the `Get-AWSPowerShellVersion
<http://docs.aws.amazon.com/powershell/latest/reference/Index.html>`_ cmdlet:

.. code-block:: none

    PS C:\> Get-AWSPowerShellVersion

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

You can also add the :code:`-ListServiceVersionInfo` parameter to a `Get-AWSPowerShellVersion
<http://docs.aws.amazon.com/powershell/latest/reference/Index.html>`_ command to see a list of which AWS
services are supported in the current version of the tools.

.. code-block:: none

    PS C:\> Get-AWSPowerShellVersion -ListServiceVersionInfo

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
    AWS Health                          HLTH        2016-08-04
    AWS Identity and Access Management  IAM         2010-05-08
    AWS Import/Export                   IE          2010-06-01
    AWS Import/Export Snowball          SNOW        2016-06-30
    AWS IoT                             IOT         2015-05-28
    AWS Key Management Service          KMS         2014-11-01
    AWS Marketplace Commerce Analytics  MCA         2015-07-01
    AWS Marketplace Entitlement Service MES         2017-01-11
    AWS Marketplace Metering            MM          2016-01-14
    AWS OpsWorks                        OPS         2013-02-18
    AWS OpsWorksCM                      OWCM        2016-11-01
    AWS Organizations                   ORG         2016-11-28
    AWS Resource Groups Tagging API     RGT         2017-01-26
    AWS Security Token Service          STS         2011-06-15
    AWS Service Catalog                 SC          2015-12-10
    AWS Shield                          SHLD        2016-06-02
    AWS Storage Gateway                 SG          2013-06-30
    AWS Support API                     ASA         2013-04-15
    AWS WAF                             WAF         2015-08-24
    AWS WAF Regional                    WAFR        2016-11-28
    AWS X-Ray                           XR          2016-04-12
    Amazon API Gateway                  AG          2015-07-09
    Amazon Athena                       ATH         2017-05-18
    Amazon CloudFront                   CF          2017-03-25
    Amazon CloudSearch                  CS          2013-01-01
    Amazon CloudSearchDomain            CSD         2013-01-01
    Amazon CloudWatch                   CW          2010-08-01
    Amazon CloudWatch Events            CWE         2015-10-07
    Amazon CloudWatch Logs              CWL         2014-03-28
    Amazon Cognito Identity             CGI         2014-06-30
    Amazon Cognito Identity Provider    CGIP        2016-04-18
    Amazon DynamoDB                     DDB         2012-08-10
    Amazon EC2 Container Registry       ECR         2015-09-21
    Amazon EC2 Container Service        ECS         2014-11-13
    Amazon ElastiCache                  EC          2015-02-02
    Amazon Elastic Compute Cloud        EC2         2016-11-15
    Amazon Elastic File System          EFS         2015-02-01
    Amazon Elastic MapReduce            EMR         2009-03-31
    Amazon Elastic Transcoder           ETS         2012-09-25
    Amazon Elasticsearch                ES          2015-01-01
    Amazon GameLift Service             GML         2015-10-01
    Amazon Inspector                    INS         2016-02-16
    Amazon Kinesis                      KIN         2013-12-02
    Amazon Kinesis Analytics            KINA        2015-08-14
    Amazon Kinesis Firehose             KINF        2015-08-04
    Amazon Lambda                       LM          2015-03-31
    Amazon Lex                          LEX         2016-11-28
    Amazon Lex Model Building Service   LMB         2017-04-19
    Amazon Lightsail                    LS          2016-11-28
    Amazon MTurk Service                MTR         2017-01-17
    Amazon Machine Learning             ML          2014-12-12
    Amazon Pinpoint                     PIN         2016-12-01
    Amazon Polly                        POL         2016-06-10
    Amazon Redshift                     RS          2012-12-01
    Amazon Rekognition                  REK         2016-06-27
    Amazon Relational Database Service  RDS         2014-10-31
    Amazon Route 53                     R53         2013-04-01
    Amazon Route 53 Domains             R53D        2014-05-15
    Amazon Server Migration Service     SMS         2016-10-24
    Amazon Simple Email Service         SES         2010-12-01
    Amazon Simple Notification Service  SNS         2010-03-31
    Amazon Simple Queue Service         SQS         2012-11-05
    Amazon Simple Storage Service       S3          2006-03-01
    Amazon Simple Systems Management    SSM         2014-11-06
    Amazon Step Functions               SFN         2016-11-23
    Amazon WorkDocs                     WD          2016-05-01
    Amazon WorkSpaces                   WKS         2015-04-08
    Application Auto Scaling            AAS         2016-02-06
    Application Discovery Service       ADS         2015-11-01
    Auto Scaling                        AS          2011-01-01
    Elastic Load Balancing              ELB         2012-06-01
    Elastic Load Balancing V2           ELB2        2015-12-01

To determine the version of PowerShell that you are running, enter :code:`$PSVersionTable` to view
the contents of the $PSVersionTable `automatic variable
<https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6>`_.

.. code-block:: none

    PS C:\> $PSVersionTable

    Name                           Value
    ----                           -----
    PSVersion                      5.0.10586.117
    PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
    BuildVersion                   10.0.10586.117
    CLRVersion                     4.0.30319.34209
    WSManStackVersion              3.0
    PSRemotingProtocolVersion      2.3
    SerializationVersion           1.1.0.1


.. _pstools-updating:

Updating the |TWPlong| and AWS Tools for PowerShell Core
========================================================

Periodically, as updated versions of the |TWP| or Tools for PowerShell Core are released, you should update the version that you are running locally. Run the :code:`Get-AWSPowerShellVersion` cmdlet to 
determine the version that you are running, and compare that with the version of |TWP| that is available at `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ or on the `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell>`_ website. 
A suggested time period for checking for an updated AWS Tools for PowerShell package is every two to three weeks. 

Update the Tools for Windows PowerShell
---------------------------------------

Update your installed |TWP| by downloading the most recent version of the MSI package from `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ and comparing the package version number in the MSI file name with the version
number you get when you run the :code:`Get-AWSPowerShellVersion` cmdlet.

If the download version is a higher number than the version you have installed, close all |TWP|
consoles, then uninstall :guilabel:`AWS Tools for Windows` by selecting it in the :guilabel:`Control
Panel | Programs and Features | Uninstall a program` dialog box, and then clicking
:guilabel:`Uninstall`. Wait for uninstallation to finish.

If you installed the existing version of the AWS Tools for PowerShell by running :code:`Install-Module`, you can uninstall the existing version by running :code:`Uninstall-Module`.

Install the newer version of the |TWP| by running the MSI package you downloaded.

Update the Tools for PowerShell Core
------------------------------------

Before you install a newer release of the AWS Tools for PowerShell Core, uninstall the existing module. Close any open 
PowerShell or AWS Tools for PowerShell sessions before you uninstall the existing Tools for PowerShell Core package. Run the following command 
to uninstall the package.

.. code-block:: none

    PS C:\> Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions

When uninstallation is finished, install the updated module by running the following command. By default, 
this command installs the latest version of the AWS Tools for PowerShell Core. This module is available on the 
`PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_, 
but the easiest method of installation is to run :code:`Install-Module`.

.. code-block:: none

    PS C:\> Install-Module -Name AWSPowerShell.NetCore


.. _pstools-seealso-setup:

See Also
========

* :ref:`pstools-getting-started`

* :ref:`pstools-using`

* :ref:`pstools-appendix-sign-up`


