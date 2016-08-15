.. _pstools-iam-new-user-group:

###############################
Create New IAM Users and Groups
###############################

This section describes how to create a new IAM group and a new IAM user and then add the user to the
group.

First, use the :code:`New-IAMGroup` cmdlet to create the group. Although we've included it here, the
:code:`-Path` parameter is optional.

.. code-block:: none

    PS C:\> New-IAMGroup -Path "/ps-created-groups/" -GroupName "powerUsers" 
    
    Path       : /ps-created-groups/
    GroupName  : powerUsers
    GroupId    : AGPAJPHUEYD5XPCGIUH3E
    Arn        : arn:aws:iam::455364113843:group/ps-created-groups/powerUsers
    CreateDate : 11/20/2012 3:32:50 PM

Next, use the :code:`New-IAMUser` cmdlet to create the user. Similar to the preceding example, the
:code:`-Path` parameter is optional.

.. code-block:: none

    PS C:\> New-IAMUser -Path "/ps-created-users/" -UserName "myNewUser" 
    
    Path       : /ps-created-users/
    UserName   : myNewUser
    UserId     : AIDAJOJSPSPXADHBT7IN6
    Arn        : arn:aws:iam::455364113843:user/ps-created-users/myNewUser
    CreateDate : 11/20/2012 3:26:31 PM

Finally, use the :code:`Add-IAMUserToGroup` cmdlet to add the user to the group.

.. code-block:: none

    PS C:\> Add-IAMUserToGroup -UserName myNewUser -GroupName powerUsers 
    
    ServiceResponse
    ---------------
    Amazon.IdentityManagement.Model.AddUserToGroupResponse

To verify that the :code:`powerUsers` group contains the :code:`myNewUser`, use the
:code:`Get-IAMGroup` cmdlet.

.. code-block:: none

    PS C:\> Get-IAMGroup -GroupName powerUsers 
    
    Group                         Users                         IsTruncated                   Marker
    -----                         -----                         -----------                   ------
    Amazon.IdentityManagement.... {myNewUser}                   False

You can also view IAM users and groups with the AWS Management Console

    `http://console.aws.amazon.com/iam/home?#s=Users <http://console.aws.amazon.com/iam/home?#s=Users>`_ [Users View]


    `http://console.aws.amazon.com/iam/home?#s=Groups <http://console.aws.amazon.com/iam/home?#s=Groups>`_ [Groups View]

    
.. _pstools-seealso-iam-users:

See Also
--------

* :ref:`pstools-using`

* :iam-ug:`Adding a New User to Your AWS Account (IAM User Guide) <Using_SettingUpUser>`

* :iam-api:`CreateGroup (IAM Service Reference) <API_CreateGroup>` 

