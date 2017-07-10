.. _pstools-iam-policy:

#################################
Set an IAM Policy for an IAM User
#################################

The following commands show how to assign an IAM policy to an IAM user. The policy specified below
provides the user with "Power User Access". This policy is identical to the *Power User Access*
policy template provided in the IAM console. The name for the policy shown below follows the naming
convention used for IAM policy templates such as the template for *Power User Access*. The
convention is

.. code-block:: none

    <template name>+<user name>+<date stamp>

In order to specify the policy document, we use a PowerShell here-string. We assign the contents of
the here-string to a variable and then use the variable as a parameter value in
:code:`Write-IAMUserPolicy`.

.. code-block:: none

    PS C:\> $policyDoc = @"
    >> {
    >>   "Version": "2012-10-17",
    >>   "Statement": [
    >>     {
    >>       "Effect": "Allow",
    >>       "NotAction": "iam:*",
    >>       "Resource": "*"
    >>     }
    >>   ]
    >> }
    >> "@
    >>
    
    PS C:\> Write-IAMUserPolicy -UserName myNewUser -PolicyName "PowerUserAccess-myNewUser-201211201605" -PolicyDocument $policyDoc 
    
    ServiceResponse
    ---------------
    Amazon.IdentityManagement.Model.PutUserPolicyResponse

.. _pstools-seealso-iam-policy:

See Also
--------

* :ref:`pstools-using`

* `Using Windows PowerShell "Here-Strings" <http://technet.microsoft.com/en-us/library/ee692792.aspx>`_

* `PutUserPolicy <http://docs.aws.amazon.com/IAM/latest/APIReference/API_PutUserPolicy.html>`_



