.. _shared-credentials-in-aws-powershell:

##############################################
Shared Credentials in AWS Tools for PowerShell
##############################################

The |sdk-net|_, |TWP|, and the AWS Toolkit for Visual Studio now support the use of
the |CLI| credentials file, similarly to other AWS SDKs. The |TWP| now support reading and writing of :code:`basic`, :code:`session`,
and :code:`assume role` credential profiles to both the .NET credentials file and the shared credential file. This functionality is
enabled by a new :code:`Amazon.Runtime.CredentialManagement` namespace.

The new profile types and access to the shared credential file are supported by the following parameters that have been added to the 
credentials-related cmdlets, `Initialize-AWSDefaultConfiguration <http://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html>`_,
`New-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/New-AWSCredential.html>`_, and 
`Set-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/Set-AWSCredential.html>`_.  In service cmdlets, you can refer to new profiles by adding the 
common parameter, :code:`-ProfileName`.

+----------------+------------------------------------------------------------------------------------------+
| Parameter Name | Description                                                                              |
+================+==========================================================================================+
| ExternalId     | The user-defined external ID to be used when assuming a role, if required by the role.   |
+----------------+------------------------------------------------------------------------------------------+
| MfaSerial      | The MFA serial number to be used when assuming a role, if required by the role.          |
+----------------+------------------------------------------------------------------------------------------+
| RoleArn        | The ARN of the role to assume for assume role credentials.                               |
+----------------+------------------------------------------------------------------------------------------+
| SourceProfile  | The name of the source profile to be used by assume role credentials.                    |
+----------------+------------------------------------------------------------------------------------------+

The :code:`ProfilesLocation` Common Parameter
=============================================

The behavior of the :code:`ProfileLocation` common parameter also changes. You can use :code:`-ProfileLocation` to write to the shared credential
file as well as instruct a cmdlet to read from the credential file. Adding the :code:`-ProfileLocation` parameter controls whether |TWP| uses the shared credential
file or the .NET credential file. The following table describes how the parameter works in |TWP|.

+---------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Profile Location Value                                  | Profile Resolution Behavior                                                                                                                                       |
+=========================================================+===================================================================================================================================================================+
| null (not set) or empty                                 | First, search the .NET credential file for a profile with the specified name. If the profile isn't found, search :code:`(user's home directory)\.aws\credentials`.|
+---------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| The path to a file in the shared credential file format | Search only the specified file for a profile with the given name.                                                                                                 |
+---------------------------------------------------------+-------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Using the Credential Profile Types
==================================

To set a credential profile type, understand which parameters provide the information required by the profile type.

+------------------+---------------------------------------------------------------------+
| Credentials Type | Parameter Combination                                               |
+==================+=====================================================================+
| Basic            | -AccessKey and -SecretKey values                                    |
+------------------+---------------------------------------------------------------------+
| Session          | -AccessKey, -SecretKey, -SessionToken                               |
+------------------+---------------------------------------------------------------------+
| AssumeRole       | -SourceProfile, -RoleArn (optionally, -ExternalId and -MfaSerial)   |
+------------------+---------------------------------------------------------------------+

Setup
-----

The following is an example showing how to set up a source profile for an assume_role_profile credential profile type. In the first line, you set up a source profile for an assume role profile.
In the second line, you set up the assume role profile itself. The third line shows the credentials from the new profile.

.. code-block:: none

    PS C:\> Set-AWSCredential -StoreAs source_profile -AccessKey access_key -SecretKey secret_key
    PS C:\> Set-AWSCredential -StoreAs assume_role_profile -SourceProfile source-profile -RoleArn arn:aws:iam::999999999999:role/some-role
    PS C:\> Get-AWSCredential -ProfileName assume_role_profile
    
    SourceCredentials                  RoleArn                                  RoleSessionName                           Options
    -----------------                  -------                                  ---------------                           -------
    Amazon.Runtime.BasicAWSCredentials arn:aws:iam::999999999999:role/some-role aws-dotnet-sdk-session-636238288466144357 Amazon.Runtime.AssumeRoleAWSCredentialsOptions

To use a new credential type with the |TWP| service cmdlets, do the following.

#. First, store credentials in one of the credential files, either the .NET credential file, or the |CLI| shared credential file.
#. Add the -ProfileName common parameter to a |TWP| cmdlet to reference the credentials.

The following is an example showing how to configure :code:`assume role` credentials with the `Get-S3Bucket <http://docs.aws.amazon.com/powershell/latest/reference/items/Get-S3Bucket.html>`_ cmdlet.

.. code-block:: none

    PS C:\> Set-AWSCredential -StoreAs source_profile -AccessKey access_key -SecretKey secret_key
    PS C:\> Set-AWSCredential -StoreAs assume_role_profile -SourceProfile source_profile -RoleArn arn:aws:iam::999999999999:role/some-role
    PS C:\> Get-S3Bucket -ProfileName assume_role_profile
    
    CreationDate           BucketName
    ------------           ----------
    2/27/2017 8:57:53 AM   4ba3578c-f88f-4d8b-b95f-92a8858dac58-bucket1
    2/27/2017 10:44:37 AM  2091a504-66a9-4d69-8981-aaef812a02c3-bucket2

Save Credentials to a Credentials File
--------------------------------------

To write and save credentials to one of the two credential files, run the :code:`Set-AWSCredential` cmdlet. The following example shows how to do this. In the first line, run :code:`Set-AWSCredential` to use the new :code:`-ProfileLocation`
write functionality to add access and secret keys to a profile named :code:`basic_profile` by adding the :code:`-ProfileName` parameter. In the second line, run
the `Get-Content <https://msdn.microsoft.com/en-us/powershell/reference/5.0/microsoft.powershell.management/get-content>`_ cmdlet to display
the contents of the credentials file.

.. code-block:: none

    PS C:\> Set-AWSCredential -ProfileLocation C:\Users\auser\.aws\credentials -ProfileName basic_profile -AccessKey access_key2 -SecretKey secret_key2
    PS C:\> Get-Content C:\Users\auser\.aws\credentials
    
    aws_access_key_id=access_key2
    aws_secret_access_key=secret_key2

Showing Credential Profiles
---------------------------

Run the `Get-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/Get-AWSCredential.html>`_ cmdlet and add the new -ListProfileDetail parameter to return credential file types and locations, and a list of profile names.

.. code-block:: none

    PS C:\> Get-AWSCredential -ListProfileDetail
    
    ProfileName                     StoreTypeName         ProfileLocation                                                                                                                               
    -----------                     -------------         ---------------                                                                                                                               
    source_profile                  NetSDKCredentialsFile                                                                                                                                               
    assume_role_profile             NetSDKCredentialsFile                                                                                                                                               
    basic_profile                   SharedCredentialsFile C:\Users\auser\.aws\credentials

Removing Credential Profiles
============================

To remove credential profiles, run the new `Remove-AWSCredentialProfile <http://docs.aws.amazon.com/powershell/latest/reference/items/Remove-AWSCredentialProfile.html>`_ cmdlet. You can continue to
use `Clear-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/Clear-AWSCredential.html>`_ for backward compatibility, but :code:`Remove-AWSCredentialProfile` is preferred.

Important Notes
===============

If you rely on Exception types or Exception messages from the three credentials cmdlets to control script flow, you might need to update existing scripts to account for the changes.

Only `Initialize-AWSDefaultConfiguration <http://docs.aws.amazon.com/powershell/latest/reference/items/Initialize-AWSDefaultConfiguration.html>`_, `New-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/New-AWSCredential.html>`_, and 
`Set-AWSCredential <http://docs.aws.amazon.com/powershell/latest/reference/items/Set-AWSCredential.html>`_ have the four new parameters. A command such as :code:`Get-S3Bucket -AccessKey access_key -SecretKey secret_key`
will continue to work.  However, :code:`Get-S3Bucket -SourceProfile source_profile_name -RoleArn arn:aws:iam::999999999999:role/role_name` will not work 
because the :code:`Get-S3Bucket` cmdlet does not support the :code:`SourceProfile` or :code:`RoleArn` parameters.

