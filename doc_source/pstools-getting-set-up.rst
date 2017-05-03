.. _pstools-getting-set-up:

########################
Setting up the |TWPlong|
########################

.. contents:: **Topics**
    :local:
    :depth: 1

.. _pstools-installing-prerequisites:

Prerequisites
=============

To use the |TWPlong| or the AWS Tools for PowerShell Core, you must have an AWS account. If you do not yet have an AWS account, see
:ref:`pstools-appendix-sign-up` for instructions on how to sign up.

To use the |TWPlong|, your installed system must meet the following prerequisites:

* Microsoft Windows XP or later

* Windows PowerShell 2.0 or later (PowerShell 5.1 or later for the Tools for PowerShell Core)

Windows 7 and Windows Server 2008 R2 come with Windows PowerShell 2.0 installed. Windows 8 and
Windows Server 2012 come with Windows PowerShell 3.0 installed. For earlier releases of Windows,
such as Windows XP, Windows Vista, Windows Server 2003, and Windows Server 2008, you can get
PowerShell 2.0 by installing the Windows Management Framework.

* `Windows Management Framework (Windows PowerShell 2.0, WinRM 2.0, and BITS 4.0)
  <http://support.microsoft.com/kb/968929>`_


.. _pstools-installing-download:

Download and Install the |TWPlong|
==================================

**Install the AWS Tools for Windows PowerShell on a Windows-based computer**

The |TWPlong| is one of the optional components that you can install by running the AWS Tools for
Windows installer :file:`.msi`. Download the installer by opening the following webpage, and
clicking :guilabel:`AWS Tools for Windows`.

* http://aws.amazon.com/powershell/

The installer for the |TWP| installs the most recent version of the |sdk-net|_. If you have
Microsoft Visual Studio installed, the installer can also install the :tvs-ug:`AWS Toolkit for
Visual Studio <welcome>`.

All Windows Amazon Machine Images (AMIs) have the |TWPlong| pre-installed. For an example of using
the |TWP| on an Amazon EC2 instance, view the AWS EC2 sample from within Visual Studio, by
selecting:

* :guilabel:`New | Project... | AWS | Compute and Networking | AWS EC2 Sample`

**Install the AWS Tools for PowerShell Core**

The AWS Tools for PowerShell Core can be installed on computers that are running Microsoft PowerShell 5.1 or a later
release of PowerShell. AWS Tools for PowerShell Core is therefore supported on the following operating systems.
For more information about how to install PowerShell 5.1 on computers that do not run Windows, see 
`Package installation instructions (Linux) <https://github.com/PowerShell/PowerShell/blob/master/docs/installation/linux.md>`_ in the GitHub repository for the Microsoft PowerShell project. 
For more information about how to install PowerShell 5.1 on computers that run Windows 8.1 or Windows 10, see `Package installation instructions (Windows) 
<https://github.com/PowerShell/PowerShell/blob/master/docs/installation/windows.md>`_, also in the GitHub repository for PowerShell.

* Ubuntu 14.04 LTS and later
* CentOS Linux 7
* Mac OS X
* Windows 8.1 Enterprise
* Windows Server 2012 R2
* Windows 10 for Business

After you have PowerShell 5.1 installed, you can find the AWS Tools for PowerShell Core on 
Microsoft's `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website.
The simplest way to install the Tools for PowerShell Core is by running the :code:`Install-Package` cmdlet. We currently recommend 
using the NuGet provider to avoid installation errors. A suggested destination path on Linux systems is :code:`~/.local/share/powershell/Modules`.

.. code-block:: none

    PS C:\> Install-Package -Name AWSPowerShell.NetCore -Source
    https://www.powershellgallery.com/api/v2/ -ProviderName NuGet -ExcludeVersion
    -Destination <path to destination folder>

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, `Introducing AWS Tools for PowerShell Core Edition <https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition>`_.

.. _enable-script-execution:

Enable Script Execution
=======================

To load the |TWPlong| module, enable PowerShell script execution if you have not already done so. To
enable script execution, run the :code:`Set-ExecutionPolicy` cmdlet to set a policy of
:code:`RemoteSigned`. By default, PowerShell uses a policy of :code:`Restricted`. For more
information about execution policies, see `Microsoft's TechNet documentation
<http://technet.microsoft.com/en-us/library/ee176961.aspx>`_.

**To enable script execution**

1. Administrator rights are required to set the execution policy. If you are not logged on as a user
   with administrator rights, open a PowerShell session as Administrator: Click :guilabel:`Start`
   and then click :guilabel:`All Programs`; click :guilabel:`Accessories`, and then click
   :guilabel:`Windows PowerShell`; now right-click :guilabel:`Windows PowerShell`, and then choose
   :guilabel:`Run as administrator` from the context menu.

2. At the command prompt, type: :code:`Set-ExecutionPolicy RemoteSigned`

.. note:: On a 64-bit system, you must also perform these steps for the 32-bit version of PowerShell,
   **Windows PowerShell (x86)**.

If you do not have the execution policy set correctly, PowerShell generates the following message.

.. code-block:: none

    File C:\Users\teslan\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1 cannot be loaded because the execution
     of scripts is disabled on this system. Please see "get-help about_signing" for more details.
    At line:1 char:2
    + . <<<<  'C:\Users\teslan\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1'
        + CategoryInfo          : NotSpecified: (:) [], PSSecurityException
        + FullyQualifiedErrorId : RuntimeException

The installer for the |TWP| updates the `PSModulePath
<http://msdn.microsoft.com/en-us/library/windows/desktop/dd878326.aspx>`_ to include the location of
the directory that contains the AWSPowerShell module. As a result, if you are running PowerShell
3.0, the AWSPowerShell module is loaded automatically whenever you run one of the AWS cmdlets. This
lets you use the AWS cmdlets interactively, even if the execution policy on your system is set to
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

The installer creates a :guilabel:`Start Menu` group called, :guilabel:`Amazon Web Services`, which
contains a shortcut called :guilabel:`Windows PowerShell for AWS`. For PowerShell 2.0, this shortcut
automatically imports the AWSPowerShell module and then runs the :code:`Initialize-AWSDefaults`
cmdlet. For PowerShell 3.0, the AWSPowerShell module is loaded automatically whenever you run an AWS
cmdlet. So, for PowerShell 3.0, the shortcut created by the installer only runs the
:code:`Initialize-AWSDefaults` cmdlet. For more information about :code:`Initialize-AWSDefaults`,
see :ref:`specifying-your-aws-credentials`.

The installer also creates an additional shortcut called :guilabel:`AWS Tools for Windows`, which
opens a visual display of AWS resources for Windows developers.

If you run PowerShell 3.0, or if you only use the shortcut installed by the installer, you do not
need to configure a PowerShell window to use the |TWPlong|. However, if, for example, you use
PowerShell 2.0 with a specially-configured PowerShell console, and you want to add support for the
tools, you must load the AWS module yourself.

.. _pstools-installing-integration:

How to Load the |TWPlong| Module (PowerShell 2.0)
-------------------------------------------------

**To load the Powershell Tools module into your current session**

1. Open a PowerShell prompt and type the following command:

    .. code-block:: none

        PS C:\> Import-Module "C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowerShell\AWSPowerShell.psd1"

    .. note:: In PowerShell 4.0 and later releases, Import-Module also searches the Program Files folder for
       installed modules, so it is not necessary to provide the full path to the module. You can
       run the following command to import the AWSPowerShell module. In PowerShell 3.0 and later,
       running a cmdlet in the module also automatically imports a module into your session.

        .. code-block:: none

            PS C:\> Import-Module AWSPowerShell

2. To verify that the module was loaded, type the following command:

   .. code-block:: none

      PS C:\> Get-Module

   If you see an entry in the list named **AWSPowerShell**, then the |TWP| module was loaded
   successfully.

    .. code-block:: none

       ModuleType Version   Name           ExportedCommands
       ---------- -------   ----           ----------------
       Binary     2.3.16.0  AWSPowerShell  {Add-ASAAttachmentsToSet, Add-ASACommunicationToCase, Add-ASInstances, Add-AWSLoggingListener...}
       ...


.. _pstools-installing-integration-profile:

Load AWS CLI for PowerShell Module into Every Session (PowerShell 2.0)
----------------------------------------------------------------------

To load the AWSPowerShell module automatically every time you start a PowerShell session, add it to
your PowerShell profile. Note, however, that adding commands to your PowerShell profile can slow
down the speed at which a PowerShell session starts.

The PowerShell :code:`$profile` variable contains the full path to the text file that contains your
PowerShell profile. This variable is available only in a PowerShell session; it is not a Windows
environment variable. To view the value of this variable, run :code:`echo`.

.. code-block:: none

   echo $profile C:\Users\{username}\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1

You can edit this file with any text editor, such as notepad.exe.

.. code-block:: none

   notepad $profile

You might need to create both the profile directory and the profile itself if they do not already
exist.



.. _pstools-versioning:

Versioning
==========

New versions of the |TWP| release periodically to support new AWS services and features. To see what
version of the |TWP| you have installed, run the `Get-AWSPowerShellVersion
<http://docs.aws.amazon.com/powershell/latest/reference/Index.html>`_ cmdlet:

.. code-block:: none

    PS C:\> Get-AWSPowerShellVersion

    AWS Tools for Windows PowerShell
    Version 3.1.76.0
    Copyright 2012-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

    Amazon Web Services SDK for .NET
    Core Runtime Version 3.1.7.0
    Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.

    Release notes: https://aws.amazon.com/releasenotes/PowerShell

    This software includes third party software subject to the following copyrights:
    - Logging from log4net, Apache License
    [http://logging.apache.org/log4net/license.html]

You can also specify the :code:`-ListServices` parameter of `Get-AWSPowerShellVersion
<http://docs.aws.amazon.com/powershell/latest/reference/Index.html>`_ to see a list of which AWS
services are supported in the current version of the tools.

.. code-block:: none

    PS C:\> Get-AWSPowerShellVersion -ListServices

    AWS Tools for Windows PowerShell
    Version 3.1.76.0
    Copyright 2012-2017 Amazon.com, Inc. or its affiliates. All Rights Reserved.

    Amazon Web Services SDK for .NET
    Core Runtime Version 3.1.7.0
    Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.

    Release notes: https://aws.amazon.com/releasenotes/PowerShell

    This software includes third party software subject to the following copyrights:
    - Logging from log4net, Apache License
    [http://logging.apache.org/log4net/license.html]


    Service                            Noun Prefix Version
    -------                            ----------- -------
    AWS Certificate Manager            ACM         2015-12-08
    AWS Cloud HSM                      HSM         2014-05-30
    AWS CloudFormation                 CFN         2010-05-15
    AWS CloudTrail                     CT          2013-11-01
    AWS CodeCommit                     CC          2015-04-13
    AWS CodeDeploy                     CD          2014-10-06
    AWS CodePipeline                   CP          2015-07-09
    AWS Config                         CFG         2014-11-12
    AWS Data Pipeline                  DP          2012-10-29
    AWS Database Migration Service     DMS         2016-01-01
    AWS Device Farm                    DF          2015-06-23
    AWS Direct Connect                 DC          2012-10-25
    AWS Directory Service              DS          2015-04-16
    AWS Elastic Beanstalk              EB          2010-12-01
    AWS Identity and Access Management IAM         2010-05-08
    AWS Import/Export                  IE          2010-06-01
    AWS IoT                            IOT         2015-05-28
    AWS Key Management Service         KMS         2014-11-01
    AWS Marketplace Commerce Analytics MCA         2015-07-01
    AWS Marketplace Metering           MM          2016-01-14
    AWS OpsWorks                       OPS         2013-02-18
    AWS Security Token Service         STS         2011-06-15
    AWS Storage Gateway                SG          2013-06-30
    AWS Support API                    ASA         2013-04-15
    AWS WAF                            WAF         2015-08-24
    Amazon API Gateway                 AG          2015-07-09
    Amazon CloudFront                  CF          2016-01-28
    Amazon CloudSearch                 CS          2013-01-01
    Amazon CloudSearchDomain           CSD         2013-01-01
    Amazon CloudWatch                  CW          2010-08-01
    Amazon CloudWatch Events           CWE         2015-10-07
    Amazon CloudWatch Logs             CWL         2014-03-28
    Amazon Cognito Identity            CGI         2014-06-30
    Amazon Cognito Identity Provider   CGIP        2016-04-18
    Amazon DynamoDB                    DDB         2012-08-10
    Amazon EC2 Container Registry      ECR         2015-09-21
    Amazon EC2 Container Service       ECS         2014-11-13
    Amazon ElastiCache                 EC          2015-02-02
    Amazon Elastic Compute Cloud       EC2         2015-10-01
    Amazon Elastic File System         EFS         2015-02-01
    Amazon Elastic MapReduce           EMR         2009-03-31
    Amazon Elastic Transcoder          ETS         2012-09-25
    Amazon Elasticsearch               ES          2015-01-01
    Amazon GameLift Service            GML         2015-10-01
    Amazon Inspector                   INS         2016-02-16
    Amazon Kinesis                     KIN         2013-12-02
    Amazon Kinesis Firehose            KINF        2015-08-04
    Amazon Lambda                      LM          2015-03-31
    Amazon Machine Learning            ML          2014-12-12
    Amazon Redshift                    RS          2012-12-01
    Amazon Relational Database Service RDS         2014-10-31
    Amazon Route 53                    R53         2013-04-01
    Amazon Route 53 Domains            R53D        2014-05-15
    Amazon Simple Email Service        SES         2010-12-01
    Amazon Simple Notification Service SNS         2010-03-31
    Amazon Simple Queue Service        SQS         2012-11-05
    Amazon Simple Storage Service      S3          2006-03-01
    Amazon Simple Systems Management   SSM         2014-11-06
    Amazon WorkSpaces                  WKS         2015-04-08
    Application Auto Scaling           AAS         2016-02-06
    Application Discovery Service      ADS         2015-11-01
    Auto Scaling                       AS          2011-01-01
    Elastic Load Balancing             ELB         2012-06-01

To determine the version of PowerShell that you are running, enter :code:`$PSVersionTable` to view
the contents of the $PSVersionTable `automatic variable
<http://technet.microsoft.com/library/hh847768.aspx>`_.

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



Updating the |TWPlong| and AWS Tools for PowerShell Core
========================================================

Periodically, as updated versions of the |TWP| or Tools for PowerShell Core are released, you'll want 
to update the version that you are running locally. Run the :code:`Get-AWSPowerShellVersion` cmdlet to 
determine the version that you are running, and compare that with the version of |TWP| that is available at `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ or `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell>`_. 
A suggested time period for checking for an updated AWS Tools for PowerShell package is every two to three weeks. 

**Update the Tools for Windows PowerShell**

Update your installed |TWP| by downloading the most recent version of the MSI package from `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ and comparing the package version number in the MSI file name with the version
number you get when you run the :code:`Get-AWSPowerShellVersion` cmdlet.

If the download version is a higher number than the version you have installed, close all |TWP|
consoles, then uninstall :guilabel:`AWS Tools for Windows` by selecting it in the :guilabel:`Control
Panel | Programs and Features | Uninstall a program` dialog box, and then clicking
:guilabel:`Uninstall`. Wait for uninstallation to finish.

Install the newer version of the |TWP| by running the MSI package you downloaded.

**Update the Tools for PowerShell Core**

Before you install a newer release of the AWS Tools for PowerShell Core, uninstall the existing package. Close any open 
PowerShell or AWS Tools for PowerShell sessions before you uninstall the existing Tools for PowerShell Core package. Run the following command 
to uninstall the package.

.. code-block:: none

    PS C:\> Uninstall-Package -Name AWSPowerShell.NetCore -AllVersions

When uninstallation is finished, install the updated package by running the following command. By default, 
this command installs the latest version of the AWS Tools for PowerShell Core. This package is available on the 
`PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_, 
but the easiest method of installation is to run :code:`Install-Package`.

.. code-block:: none

    PS C:\> Install-Package -Name AWSPowerShell.NetCore -ProviderName NuGet
    -Destination <path to destination folder>

.. _pstools-seealso-setup:

See Also
========

* :ref:`pstools-getting-started`

* :ref:`pstools-using`

* :doc:`pstools-appendix-sign-up`

.. toctree::
    :titlesonly:
    :maxdepth: 1
    :hidden:

    pstools-appendix-sign-up

