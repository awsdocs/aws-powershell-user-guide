.. _pstools-history:

################
Document History
################

This topic describes significant changes to the documentation for the |TWPALLlong|.

*Last documentation update: September 17, 2018*

We also update the documentation periodically in response to customer feedback. To send feedback about a topic, 
use the feedback buttons next to "Did this page help you?" located at the bottom of each page.

For additional information about changes and updates to the |TWP|, see the `release notes
<http://aws.amazon.com/releasenotes/PowerShell>`_.


.. _pstools-release-v3-3-343-0:

|TWPALLlong| 3.3.343.0
======================
*Release Date:* 2018-09-11

*Summary of Changes*

* Added information to the :twp-ug:`Using the AWS Tools for PowerShell <pstools-using>` section 
  introducing the AWS Lambda Tools for PowerShell for PowerShell Core developers to build |LAMlong| 
  functions.

|TWPlong| 3.1.31.0
==================

*Release Date:* 2015-12-01

*Summary of Changes*

* Added information to the :twp-ug:`Getting Started <pstools-getting-started>` section about new 
  cmdlets that use Security Assertion Markup Language (SAML) to support configuring federated 
  identity for users.


.. _pstools-release-v2-3-19:

|TWPlong| 2.3.19
================

*Release Date:* 2015-02-05

*Summary of Changes*

* Added information to the :twp-ug:`Cmdlets Discovery and Aliases <pstools-discovery-aliases>`
  section about the new :code:`Get-AWSCmdletName` cmdlet that can help users more easily find
  their desired AWS cmdlets.


.. _pstools-release-v1-1-1-0:

|TWPlong| 1.1.1.0
=================

*Release Date:* 2013-05-15

*Summary of Changes*

* Collection output from cmdlets is always enumerated to the PowerShell pipeline

* Automatic support for pageable service calls

* New $AWSHistory shell variable collects service responses and optionally service requests

* AWSRegion instances use Region field instead of SystemName to aid pipelining

* Remove-S3Bucket supports a -DeleteObjects switch option

* Fixed usability issue with Set-AWSCredentials

* Initialize-AWSDefaults reports from where it obtained credentials and region data

* Stop-EC2Instance accepts Amazon.EC2.Model.Reservation instances as input

* Generic List<T> parameter types replaced with array types (T[])

* Cmdlets that delete or terminate resources prompt for confirmation prior to deletion

* Write-S3Object supports in-line text content to upload to Amazon S3


.. _pstools-release-v1-0-1-0:

|TWPlong| 1.0.1.0
=================

*Release Date:* 2012-12-21

The install location of the |TWP| module has changed so that environments using Windows PowerShell
version 3 can take advantage of auto-loading.

* The module and supporting files are now installed to an *AWSPowerShell* subfolder beneath *AWS
  Tools\PowerShell*. Files from previous versions that exist in the *AWS Tools\PowerShell* folder
  are automatically removed by the installer.

* The :code:`PSModulePath` for Windows PowerShell (all versions) is updated in this release to contain
  the parent folder of the module (*AWS Tools\PowerShell*).

* For systems with Windows PowerShell version 2, the Start Menu shortcut :guilabel:`Amazon Web 
  Services\Windows PowerShell for AWS` is updated to import the module from the new location and
  then run :code:`Initialize-AWSDefaults`.

* For systems with Windows PowerShell version 3, the Start Menu shortcut :guilabel:`Amazon Web 
  Services\Windows PowerShell for AWS` is updated to remove the :code:`Import-Module` command, 
  leaving just :code:`Initialize-AWSDefaults`.

* If you edited your PowerShell profile to perform an :code:`Import-Module` of the
  :file:`AWSPowerShell.psd1` file, you will need to update it to point to the file's new location (or,
  if using PowerShell version 3, remove the :code:`Import-Module` statement as it is no longer
  needed).

As a result of these changes, the |TWP| module is now listed as an available module when executing
:code:`Get-Module -ListAvailable`. In addition, for users of Windows PowerShell version 3, the
execution of any cmdlet exported by the module will automatically load the module in the current
PowerShell shell without needing to use :code:`Import-Module` first. This enables interactive use of
the cmdlets on a system with an execution policy that disallows script execution.


.. _pstools-release-v1-0-0-0:

|TWPlong| 1.0.0.0
=================

*Release Date:* 2012-12-06

Initial release



