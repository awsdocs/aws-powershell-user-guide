.. _pstools-getting-set-up-linux-mac:

################################################################
Setting up the AWS Tools for PowerShell Core on Linux or macOS X
################################################################

.. _pstools-installing-core-prerequisites:

Overview of Setup
=================

.. note::

    You can skip AWS Tools for PowerShell Core installation on the .NET Core with Ubuntu Server 16.04 and .NET Core with Amazon Linux 2 LTS Candidate Amazon Machine Images (AMIs). These AMIs are preconfigured with .NET Core 2.0, PowerShell Core 6.0, the AWS Tools for PowerShell Core, and the |CLI|.
	
A non-Windows-based computer can run only the AWS Tools for PowerShell Core (AWSPowerShell.NetCore). Setting up the AWS Tools for PowerShell Core involves the following tasks, described in this topic.

#. Installing Microsoft PowerShell Core 6.0 or newer on a supported non-Windows system.
#. After installing Microsoft PowerShell Core, starting PowerShell by running :code:`pwsh` in your system shell.
#. Installing the AWS Tools for PowerShell Core.
#. Running the `Initialize-AWSDefaultConfiguration <https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html>`_ cmdlet to provide your AWS credentials.

Prerequisites
=============

To use the the AWS Tools for PowerShell Core, you must have an AWS account. If you do not yet have an AWS account, see
:ref:`pstools-appendix-sign-up` for instructions.

To run the AWS Tools for PowerShell Core, your system must be running Microsoft PowerShell Core 6.0 or newer. For more information 
about how to install PowerShell Core 6.0 or newer on a Linux-based computer, see 
`Installing PowerShell Core on Linux <https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-linux?view=powershell-6>`_. 
For information about how to install PowerShell Core 6.0 on macOS 10.12 or higher, see `Installing PowerShell Core on macOS <https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-powershell-core-on-macos?view=powershell-6>`_.

Install the |TPClong| on Linux, macOS X, and Other Non-Windows Systems
======================================================================

To upgrade to a newer release of the AWS Tools for PowerShell Core, follow instructions in pstools-updating-core_. Uninstall older versions of PowerShell first.

You can install the AWS Tools for PowerShell Core on computers that are running Microsoft PowerShell Core 6.0 or newer.
Microsoft PowerShell Core 6.0 is supported on the following non-Windows-based operating systems.

* Ubuntu 14.04 LTS and newer
* CentOS Linux 7
* Arch Linux
* Debian 8.7 and newer
* Red Hat Enterprise Linux 7
* OpenSUSE 42.3
* Fedora 25 and 26
* Snap
* Raspbian Stretch
* macOS 10.12

Some Linux-based operating systems, such as Arch, Kali, and Raspbian, are not officially supported, but have community support. 

After you install PowerShell Core, you can find the AWS Tools for PowerShell Core on 
Microsoft's `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website.
The simplest way to install the AWS Tools for PowerShell Core is by running the :code:`Install-Module` cmdlet. First, start your PowerShell session by running :code:`pwsh` in a shell.

.. note::

    Although you can start PowerShell by running :code:`sudo pwsh` to run PowerShell with elevated rights, be aware that this is a potential security risk, and not consistent with the principle of least privilege.

Next, run :code:`Install-Module` as shown in the following command.

.. code-block:: none

    PS> Install-Module -Name AWSPowerShell.NetCore -AllowClobber

It is not necessary to run this command as Administrator, unless you want to install the AWS Tools for PowerShell Core for all users of a computer. To do this, run the following command in a PowerShell session that you have started with :code:`sudo pwsh`:

.. code-block:: none

    PS> Install-Module -Scope CurrentUser -Name AWSPowerShell.NetCore -Force

For more information about the release of AWS Tools for PowerShell Core, see the AWS blog post, `Introducing AWS Tools for PowerShell Core Edition <https://blogs.aws.amazon.com/net/post/TxTUNCCDVSG05F/Introducing-AWS-Tools-for-PowerShell-Core-Edition>`_.

Installation Troubleshooting Tips
=================================

Some users have reported issues with the Install-Module cmdlet that is included with older releases of PowerShell Core, including errors 
related to semantic versioning (see https://github.com/OneGet/oneget/issues/202). Using the NuGet provider appears to 
resolve the issue. Newer versions of PowerShell Core have resolved this issue.

To install AWS Tools for PowerShell Core by using NuGet, run the following command. Specify an appropriate destination folder (on Linux, try :code:`-Destination ~/.local/share/powershell/Modules`).

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


The AWS Tools installer updates the `PSModulePath
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

Configure a PowerShell Console to Use the |TPClong|
===================================================

Because PowerShell 3.0 and newer automatically load the AWSPowerShell module whenever you run an AWS
cmdlet, and AWSPowerShell.NetCore requires at least PowerShell 6.0, there is no need to configure PowerShell to use the AWS PowerShell Tools. 
When you start PowerShell on a Linux-based system after you have installed the AWS Tools for PowerShell Core, run `Initialize-AWSDefaultConfiguration <https://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html>`_ 
to specify your AWS access and secret keys. For more information about :code:`Initialize-AWSDefaultConfiguration`,
see :ref:`specifying-your-aws-credentials`. In older (before 3.3.96.0) releases of the AWS Tools for PowerShell, this cmdlet was named
:code:`Initialize-AWSDefaults`.

.. _pstools-versioning:

Versioning
==========

AWS releases new versions of the AWS Tools for PowerShell and AWS Tools for PowerShell Core periodically to support new AWS services and features. To determine 
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

    PS> Get-AWSPowerShellVersion -ListServiceVersionInfo
    
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
    ...

To determine the version of PowerShell that you are running, enter :code:`$PSVersionTable` to view
the contents of the $PSVersionTable `automatic variable
<https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_automatic_variables?view=powershell-6>`_.

.. code-block:: none

    PS> $PSVersionTable
    
    Name                           Value
    ----                           -----
    PSVersion                      6.0.0
    PSEdition                      Core
    GitCommitId                    v6.0.0
    OS                             Linux 4.4.0-1047-aws #56-Ubuntu SMP Sat Jan 6 19:39:06 UTC 2018
    Platform                       Unix
    PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
    PSRemotingProtocolVersion      2.3
    SerializationVersion           1.1.0.1
    WSManStackVersion              3.0


.. _pstools-updating-core:
	
Updating the |TPClong|
======================

Periodically, as updated versions of the AWS Tools for PowerShell Core are released, you should update the version that you are running locally. Run the :code:`Get-AWSPowerShellVersion` cmdlet to 
determine the version that you are running, and compare that with the version of AWS Tools for PowerShell Core that is available at `AWS Tools for Windows PowerShell
<https://aws.amazon.com/powershell/>`_ or on the `PowerShell Gallery <https://www.powershellgallery.com/packages/AWSPowerShell.NetCore>`_ website. 
A suggested time period for checking for an updated AWS Tools for PowerShell package is every two to three weeks. 


Update the |TPC| (All systems)
------------------------------

Before you install a newer release of the |TPClong|, close any open 
PowerShell or |TPClong| sessions before you uninstall the existing |TPC| package. 
You can exit a PowerShell session on a Linux-based system by pressing :guilabel:`Ctrl+D`. Run the following command 
to uninstall the package.

.. code-block:: none

    PS> Uninstall-Module -Name AWSPowerShell.NetCore -AllVersions

When uninstallation is finished, install the updated module by running the following command. By default, 
this command installs the latest version of the |TPC|. This module is available on the 
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


