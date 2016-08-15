.. _specifying-your-aws-credentials:

#####################
Using AWS Credentials
#####################

Each |TWP| command must include a set of AWS credentials, which are used to cryptographically sign
the corresponding web service request. You can specify credentials per-command, per-session, or for
all sessions. As a best practice, to avoid exposing your credentials, do not put literal credentials
in a command. Instead, create a profile for each set of credentials that you want to use, and store
the profile in either of two credentials stores. Specify the correct profile by name in your
command, and the |TWP| retrieve the associated credentials. For a general discussion of how to
safely manage AWS credentials, see 
:aws-gr:`Best Practices for Managing AWS Access Keys <aws-access-keys-best-practices>`.

.. note:: If you do not yet have an AWS account, you will need one in order to obtain credentials 
   and use the |TWP|. For information about how to sign up for an account, see 
   :ref:`pstools-appendix-sign-up`.


.. contents:: **Topics**
    :local:
    :depth: 1

.. _specifying-your-aws-credentials-store:

Managing Profiles
-----------------

The |TWP| can use either of two credentials stores.

* The AWS SDK store, which encrypts your credentials and stores them in your home folder.

  The |sdk-net|_ and |TVS|_ can also use the AWS SDK store.

* The credentials file, which is also located in your home folder, but stores credentials as plain
  text.

  By default, the credentials file is stored here: :file:`C:\\Users\\username\\.aws`. The AWS SDKs
  and the |CLIlong| can also use the credentials file. If you are running a script outside of your
  AWS user context, be sure that the file that contains your credentials is copied to a location
  where all user accounts (local system and user) can access your credentials.

This topic describes how to use the |TWP| to manage your profiles in the AWS SDK store. You can also
manage the AWS SDK store by using the :tvs-ug:`Toolkit for Visual Studio <tkv_setup>` or 
programmatically by using the |sdk-net|_. For directions about how to manage profiles in the 
credentials file, see :aws-gr:`Best Practices for Managing AWS Access Keys <aws-access-keys-best-practices>`.

Add a new profile

To add a new profile to the AWS SDK store, call :code:`Set-AWSCredentials` as follows:

    .. code-block:: none

        PS C:\> Set-AWSCredentials -AccessKey {AKIAIOSFODNN7EXAMPLE} -SecretKey {wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY} -StoreAs {MyProfileName}

    * :code:`-AccessKey` |ndash| The access key.

    * :code:`-SecretKey` |ndash| The secret key.

    * :code:`-StoreAs` |ndash| The profile name, which must be unique.

        To specify the default profile, set the profile name to :code:`default`.


Update a profile

The AWS SDK store must be maintained manually. If you later change credentials on the
service|mdash|for example, by using the `IAM console <iam/home#s=Users>`_ |mdash| running a
command with the locally stored credentials fails with the following error message: 

    .. code-block:: none

        The AWS Access Key Id you provided does not exist in our records.

You can update a profile by repeating the:code:`Set-AWSCredentials` command for the profile, and 
passing it the new access and secret keys.

List profiles

You can check the current list of names as follows:

    .. code-block:: none

        PS C:\> Get-AWSCredentials -ListStoredCredentials

Remove a profile

To remove a profile, use the following command: 

    .. code-block:: none

        PS C:\> Clear-AWSCredentials -StoredCredentials {MyProfileName}

The :code:`-StoredCredentials` parameter specifies the profile name.


.. _specifying-your-aws-credentials-use:

Specifying Credentials
----------------------

There are several ways to specify credentials. The preferred approach is to use a profile rather
than incorporating literal credentials into your command line. The |TWP| locates the profile using a
search order that is described in :ref:`pstools-cred-provider-chain`. This section describes the
most common ways to specify a profile.

AWS credentials are encrypted with the logged-on Windows user identity; they cannot be decrypted by
using another account, or used on a different device from the one on which they were originally
created. To perform tasks in the context of another user, such as a user account under which a
scheduled task will run, set up an encrypted credential profile, as described in the preceding
section, that you can use when you log on to the computer as that user. Log on as the
task-performing user to complete the credential setup steps, create a profile that will work for
that user, and then log off and log on again by using your own credentials to set up the scheduled
task.

.. note:: You use the :code:`-ProfileName` parameter to specify a profile. This parameter is equivalent to the
   :code:`-StoredCredentials` parameter used by earlier |TWP| releases. For backward compatibility,
   :code:`-StoredCredentials` is still supported.

Default profile (recommended)

Use :code:`Initialize-AWSDefaults` to specify a default profile for every PowerShell session.

    .. code-block:: none

        PS C:\> Initialize-AWSDefaults -ProfileName {MyProfileName} -Region {us-west-2}

    .. note:: The default credentials are included in the AWS SDK store under the :code:`default` profile name.
       The command overwrites any existing profile with that name.

Session profile

Use :code:`Set-AWSCredentials` to specify a default profile for a particular session. This 
profile overrides any default profile for the duration of the session.

    .. code-block:: none

        PS C:\> Set-AWSCredentials -ProfileName {MyProfileName}

    .. note:: In versions of the |TWP| that are older than 1.1, the :code:`Set-AWSCredentials` 
       command did not work correctly, and would overwrite the profile specified by {MyProfileName}. 
       We recommend using a more recent version of the |TWP|.

Command profile

Add the :code:`-ProfileName` parameter to specify a profile for a particular command. This 
profile overrides any default or session profiles. For example: 

    .. code-block:: none

        PS C:\> Get-EC2Instance -ProfileName {MyProfileName}

.. tip:: When you specify a default or session profile, you can also add a :code:`-Region` parameter to
   specify a default or session region. For more information, see
   :ref:`pstools-installing-specifying-region`. The following example specifies a default profile
   and region.

    .. code-block:: none

       PS C:\> Initialize-AWSDefaults -ProfileName {MyProfileName} -Region {us-west-2}

By default, the credentials file is assumed to be in the user's home folder
(:file:`C:\users\username\.aws`). To specify a credentials file in another location, include a
:code:`-ProfilesLocation` parameter, set to the credentials file path. The following example
specifies a non-default credentials file for a specific command.

.. code-block:: none

   PS C:\> Get-EC2Instance -ProfileName {MyProfileName} -ProfilesLocation C:\aws_service_credentials\credentials

.. tip:: If you are running a PowerShell script during a time that you are not normally signed in to
   AWS |mdash| for example, you are running a PowerShell script as a scheduled task outside of your
   normal work hours |mdash| add the :code:`-ProfilesLocation` parameter when you specify the
   profile that you want to use, and set the value to the path of the file that stores your
   credentials. To be certain that your |TWP| script runs with the correct account credentials, you
   should add the :code:`-ProfilesLocation` parameter whenever your script runs in a context or
   process that does not use an AWS account. You can also copy your credentials file to a location
   that is accessible to the local system or other account that your scripts use to perform tasks.


.. _pstools-cred-provider-chain:

Credentials Search Order
------------------------

When you run a command, the |TWP| search for credentials in the following order, and uses the first
available set.

1. Use literal credentials that are embedded in the command line.

   We strongly recommend using profiles rather than putting literal credentials in your command
   lines.

2. Use a specified profile name or profile location.

   * If you specify only a profile name, use a specified profile from the AWS SDK store and, if that does
     not exist, the specified profile from the credentials file in the default location.

   * If you specify only a profile location, use the :code:`default` profile from that credentials file.

   * If you specify a name and a location, use the specified profile from that credentials file.

   If the specified profile or location is not found, the command throws an exception. Search
   proceeds to the following steps only if you have not specified a profile or location.

3. Use credentials specified by the :code:`-Credentials` parameter.

4. Use a session profile.

5. Use a default profile, in the following order:

   1. The :code:`default` profile in the AWS SDK store.

   2. The :code:`default` profile in the credentials file.

   3. Use the :file:`AWS PS Default` profile in the AWS SDK store.

6. If you are using running the command on an |EC2| instance that is configured for an |IAM| role, use
   EC2 instance credentials stored in an instance profile.

   For more information about using |IAM| roles for |EC2| Instances, see the |sdk-net|_.

If this search fails to locate the specified credentials, the command throws an exception.
