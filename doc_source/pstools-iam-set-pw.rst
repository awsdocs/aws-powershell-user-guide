.. _pstools-iam-set-pw:

#######################################
Set an Initial Password for an IAM User
#######################################

The following example demonstrates how to use the :code:`New-IAMLoginProfile` cmdlet to set an
initial password for an IAM user. For more information about character limits and recommendations
for passwords, see 
:iam-ug-deep:`Password Policy Options <id_credentials_passwords_account-policy.html#password-policy-details>` 
in the |iam-ug|.

.. code-block:: none

    PS C:\> New-IAMLoginProfile -UserName myNewUser -Password "&!123!&" 
    
    UserName                                                    CreateDate
    --------                                                    ----------
    myNewUser                                                   11/20/2012 4:23:05 PM

Use the :code:`Update-IAMLoginProfile` cmdlet to update the password for an IAM user.

.. _pstools-seealso-iam-pw:

See Also
--------

* :ref:`pstools-using`

* :iam-ug:`Managing Passwords <Using_ManagingLogins>`

* :iam-ug:`CreateLoginProfile <API_CreateLoginProfile>`



