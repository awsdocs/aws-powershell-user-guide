.. _pstools-iam-create-creds:

###########################################
Create Security Credentials for an IAM User
###########################################

The following example uses the :code:`New-IAMAccesskey` cmdlet to create security credentials for an
IAM user. A set of security credentials comprises an Access Key ID and a Secret Key. Note that an
IAM user can have no more than two sets of credentials at any given time. If you attempt to create a
third set, the :code:`New-IAMAccessKey` cmdlet returns an error.

.. code-block:: none

    PS C:\> New-IAMAccessKey -UserName myNewUser 
    
    UserName        : myNewUser
    AccessKeyId     : AKIAIOSFODNN7EXAMPLE
    Status          : Active
    SecretAccessKey : wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKE
    CreateDate      : 11/20/2012 4:30:04 PM

Use the :code:`Remove-IAMAccessKey` cmdlet to delete a set of credentials for an IAM user. Specify
credentials to delete using the Access Key ID.

.. code-block:: none

    PS C:\> Remove-IAMAccessKey -UserName myNewUser -AccessKeyId AKIAIOSFODNN7EXAMPLE 
    
    ServiceResponse
    ---------------
    Amazon.IdentityManagement.Model.DeleteAccessKeyResponse


