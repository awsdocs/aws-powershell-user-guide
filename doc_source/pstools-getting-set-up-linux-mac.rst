.. _pstools-getting-set-up-linux-mac:

###########################################################
Setting up the AWS Tools for PowerShell on Linux or macOS X
###########################################################

.. _pstools-installing-core-prerequisites:

Prerequisites
=============

To use the the AWS Tools for PowerShell Core, you must have an AWS account. If you do not yet have an AWS account, see
:ref:`pstools-appendix-sign-up` for instructions.

To run the AWS Tools for PowerShell Core, your system must be running PowerShell Core 6.0 or newer. 
For more information, see "Install the AWS Tools for PowerShell Core" in this topic. For more information 
about how to install PowerShell Core 6.0 or newer on a Linux-based computer, see 
`PowerShell Package Installation Instructions <https://github.com/PowerShell/PowerShell/blob/master/docs/installation/linux.md>`_.

Download and Install the AWS Tools for PowerShell Core on Linux, macOS X, and Other non-Windows Systems
=======================================================================================================

You can install the AWS Tools for PowerShell Core on computers that are running Microsoft PowerShell Core 6.0 or newer.
Microsoft PowerShell Core 6.0 is supported on the following non-Windows-based operating systems.

* Ubuntu 14.04 LTS and newer
* CentOS Linux 7
* Arch Linux
* Debian 8
* Red Hat Enterprise Linux 7
* macOS 10.12
* Kali

For more information about how to install PowerShell Core on computers that do not run Windows, see 
`Package installation instructions (Linux) <https://github.com/PowerShell/PowerShell/blob/master/docs/installation/linux.md>`_ in the GitHub repository for the Microsoft PowerShell project. 

After you install PowerShell Core, you can find the AWS Tools for PowerShell Core on 
Microsoft's `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website.
The simplest way to install the Tools for PowerShell Core is by running the :code:`Install-Module` cmdlet. First, start your PowerShell session by running :code:`pwsh` in a shell.

.. note::

    Although you can start PowerShell by running :code:`sudo pwsh` to run PowerShell with elevated rights, be aware that this is a potential security risk, and not consistent with the principle of least privilege.

Next, run :code:`Install-Module` as shown in the following command.

.. code-block:: none

    PS> Install-Module -Name AWSPowerShell.NetCore -AllowClobber

It is not necessary to run this command as Administrator, unless you want to install the AWS Tools for PowerShell Core for all users of a computer. To do this, run the following command in a PowerShell session that is running as Administrator:

.. code-block:: none

    PS> Install-Module -Scope CurrentUser -Name AWSPowerShell.NetCore -Force

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, `Introducing AWS Tools for PowerShell Core Edition <https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition>`_.

Installation Troubleshooting Tips
=================================

Some users have reported issues with the Install-Module cmdlet that is included with older releases of PowerShell Core, including errors 
related to semantic versioning (see https://github.com/OneGet/oneget/issues/202). Using the NuGet provider appears to 
resolve the issue. Newer versions of PowerShell Core have resolved this issue.

To install AWS Tools for PowerShell Core by using NuGet, run the following command. Specify an appropriate destination folder (on Linux, try -Destination ~/.local/share/powershell/Modules).

.. code-block:: none

    PS> Install-Package -Name AWSPowerShell.NetCore -Source
    https://www.powershellgallery.com/api/v2/ -ProviderName NuGet -ExcludeVersion
    -Destination <path to destination folder>


.. _enable-script-execution:

Script Execution
================

The :code:`Set-ExecutionPolicy` command is not available in PowerShell Core running on non-Windows systems. You can run :code:`Get-ExecutionPolicy`, which shows that the default execution policy setting in 
PowerShell Core running on non-Windows systems is :code:`Unrestricted`. For more
information about execution policies, see `About Execution Policies <https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-5.1>`_ on the Microsoft Technet website.


The |TWP| installer updates the `PSModulePath
<http://msdn.microsoft.com/en-us/library/windows/desktop/dd878326.aspx>`_ to include the location of
the directory that contains the AWSPowerShell module. 

Because the :code:`PSModulePath` includes the location of the AWS module's directory, the
:code:`Get-Module -ListAvailable` cmdlet shows the module.

.. code-block:: none

    PS> Get-Module -ListAvailable
    
    Directory: /home/ubuntu/.local/share/powershell/Modules
    
    ModuleType Version    Name                                ExportedCommands
    ---------- -------    ----                                ----------------
    Binary     3.3.219.0  AWSPowerShell.NetCore               {Add-AASScalableTarget, Add-ACMCertificateTag, Add-ADSC...


.. _pstools-config-ps-window:

Configure a PowerShell Console to Use the AWS Tools for PowerShell Core
=======================================================================

Because PowerShell 3.0 and newer automatically load the AWSPowerShell module whenever you run an AWS
cmdlet, and AWSPowerShell.NetCore requires at least PowerShell 6.0, there is no need to configure PowerShell to use the AWS PowerShell Tools. 
When you start PowerShell on a Linux-based system after you have installed the AWS Tools for PowerShell Core, run :code:`Initialize-AWSDefaultConfiguration`
<https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html>`_ 
to specify your AWS access and secret keys. For more information about :code:`Initialize-AWSDefaultConfiguration`,
see :ref:`specifying-your-aws-credentials`. In older (before 3.3.96.0) releases of the AWS Tools for PowerShell, this cmdlet was named
:code:`Initialize-AWSDefaults`.

.. _pstools-versioning:

Versioning
==========

|AWS| releases new versions of the AWS Tools for PowerShell and AWS Tools for PowerShell Core periodically to support new AWS services and features. To determine 
the version of the Tools that you have installed, run the `Get-AWSPowerShellVersion
<https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html>`_ cmdlet:

.. code-block:: none

    PS> Get-AWSPowerShellVersion
    
    AWS Tools for PowerShell Core
    Version 3.3.219.0
    Copyright 2012-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    
    Amazon Web Services SDK for .NET
    Core Runtime Version 3.3.21.6
    Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    
    Release notes: https://aws.amazon.com/releasenotes/PowerShell
    
    This software includes third party software subject to the following copyrights:
    - Logging from log4net, Apache License
    [http://logging.apache.org/log4net/license.html]


You can also add the :code:`-ListServiceVersionInfo` parameter to a `Get-AWSPowerShellVersion
<https://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSPowerShellVersion.html>`_ command to see a list of which AWS
services are supported in the current version of the tools.

.. code-block:: none

    PS C:\> Get-AWSPowerShellVersion -ListServiceVersionInfo
    
    AWS Tools for PowerShell Core
    Version 3.3.219.0
    Copyright 2012-2018 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    
    Amazon Web Services SDK for .NET
    Core Runtime Version 3.3.21.6
    Copyright 2009-2015 Amazon.com, Inc. or its affiliates. All Rights Reserved.
    
    Release notes: https://aws.amazon.com/releasenotes/PowerShell
    
    This software includes third party software subject to the following copyrights:
    - Logging from log4net, Apache License
    [http://logging.apache.org/log4net/license.html]


    Service                               Noun Prefix API Version
    -------                               ----------- -----------
    AWS AppStream                         APS         2016-12-01
    AWS AppSync                           ASYN        2017-07-25
    AWS Batch                             BAT         2016-08-10
    AWS Budgets                           BGT         2016-10-20
    AWS Certificate Manager               ACM         2015-12-08
    AWS Cloud Directory                   CDIR        2016-05-10
    AWS Cloud HSM                         HSM         2014-05-30
    AWS Cloud HSM V2                      HSM2        2017-04-28
    AWS Cloud9                            C9          2017-09-23
    AWS CloudFormation                    CFN         2010-05-15
    AWS CloudTrail                        CT          2013-11-01
    AWS CodeBuild                         CB          2016-10-06
    AWS CodeCommit                        CC          2015-04-13
    AWS CodeDeploy                        CD          2014-10-06
    AWS CodePipeline                      CP          2015-07-09
    AWS CodeStar                          CST         2017-04-19
    AWS Config                            CFG         2014-11-12
    AWS Cost Explorer                     CE          2017-10-25
    AWS Cost and Usage Report             CUR         2017-01-06
    AWS Data Pipeline                     DP          2012-10-29
    AWS Database Migration Service        DMS         2016-01-01
    AWS Device Farm                       DF          2015-06-23
    AWS Direct Connect                    DC          2012-10-25
    AWS Directory Service                 DS          2015-04-16
    AWS Elastic Beanstalk                 EB          2010-12-01
    AWS Elemental MediaConvert            EMC         2017-08-29
    AWS Elemental MediaLive               EML         2017-10-14
    AWS Elemental MediaPackage            EMP         2017-10-12
    AWS Elemental MediaStore              EMS         2017-09-01
    AWS Elemental MediaStore Data Plane   EMSD        2017-09-01
    AWS Greengrass                        GG          2017-06-07
    AWS Health                            HLTH        2016-08-04
    AWS Identity and Access Management    IAM         2010-05-08
    ...

To determine the version of PowerShell that you are running, enter :code:`$PSVersionTable` to view
the contents of the $PSVersionTable `automatic variable
<http://technet.microsoft.com/library/hh847768.aspx>`_.

.. code-block:: none

    PS> $PSVersionTable

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

Periodically, as updated versions of the AWS Tools for PowerShell Core are released, you should update the version that you are running locally. Run the :code:`Get-AWSPowerShellVersion` cmdlet to 
determine the version that you are running, and compare that with the version of AWS Tools for PowerShell Core that is available at `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ or on the `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website. 
A suggested time period for checking for an updated AWS Tools for PowerShell package is every two to three weeks. 


Update the Tools for PowerShell Core (All systems)
--------------------------------------------------

Before you install a newer release of the AWS Tools for PowerShell Core, close any open 
PowerShell or AWS Tools for PowerShell Core sessions before you uninstall the existing Tools for PowerShell Core package. 
You can exit a PowerShell session on a Linux-based system by pressing :guilabel:`Ctrl+D`. Run the following command 
to uninstall the package.

.. code-block:: none

    PS> Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions

When uninstallation is finished, install the updated module by running the following command. By default, 
this command installs the latest version of the AWS Tools for PowerShell Core. This module is available on the 
`PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_, 
but the easiest method of installation is to run :code:`Install-Module`.

.. code-block:: none

    PS> Install-Module -Name AWSPowerShell.NetCore


.. _pstools-seealso-setup:

See Also
========

* :ref:`pstools-getting-started`

* :ref:`pstools-using`

* :ref:`pstools-appendix-sign-up`


