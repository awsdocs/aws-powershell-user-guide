.. _pstools-ec2-sg:

################################################
Create a Security Group Using Windows PowerShell
################################################

You can use the |TWPlong| to create and configure a security group. When you create a security
group, you specify whether it is for EC2-Classic or EC2-VPC. The response is the ID of the security
group.

If you need to connect to your instance, you must configure the security group to allow SSH traffic
(Linux) or RDP traffic (Windows).


.. contents:: **Topics**
    :local:
    :depth: 1

.. _sg-prerequisites:

Prerequisites
-------------

You need the public IP address of your computer, in CIDR notation. You can get the public IP address
of your local computer using a service. For example, we provide the following service:
`http://checkip.amazonaws.com/ <http://checkip.amazonaws.com/>`_ or `https://checkip.amazonaws.com/ <https://checkip.amazonaws.com/>`_. 
To locate another service that provides your IP address, use the search phrase "what is my IP address". 
If you are connecting through an ISP or from behind your firewall without a static IP address, 
you need to find the range of IP addresses used by client computers.

If you use :code:`0.0.0.0/0`, you enable all IP addresses to access your instance. For the SSH and
RDP protocols, this is acceptable for a short time in a test environment, but it's unsafe for
production environments. In production, you'll authorize only a specific IP address or range of
addresses to access your instance.


.. _get-ec2securitygroup:

Creating a Security Group for EC2-Classic
-----------------------------------------

The following example uses the :code:`New-EC2SecurityGroup` cmdlet to create a security group for
EC2-Classic.

.. code-block:: none

    PS C:\> New-EC2SecurityGroup -GroupName myPSSecurityGroup -GroupDescription "EC2-Classic from PowerShell"
            
    sg-9cf9e5d9

To view the initial configuration of the security group, use the :code:`Get-EC2SecurityGroup`
cmdlet.

.. code-block:: none

    PS C:\> Get-EC2SecurityGroup -GroupNames myPSSecurityGroup
    
    OwnerId             : 123456789012
    GroupName           : myPSSecurityGroup
    GroupId             : sg-9cf9e5d9
    Description         : EC2-Classic from PowerShell
    IpPermissions       : {}
    IpPermissionsEgress : {}
    VpcId               :
    Tags                : {}

To configure the security group to allow inbound traffic on TCP port 22 (SSH) and TCP port 3389, use
the :code:`Grant-EC2SecurityGroupIngress` cmdlet. For example, the following script shows how you enable SSH traffic
from a single IP address, :code:`203.0.113.25/32`.

.. code-block:: none

  PS C:\> $cidrBlocks = New-Object 'collections.generic.list[string]'
	$cidrBlocks.add("203.0.113.25/32")
	$ipPermissions = New-Object Amazon.EC2.Model.IpPermission
	$ipPermissions.IpProtocol = "tcp"
	$ipPermissions.FromPort = 22
	$ipPermissions.ToPort = 22
	$ipPermissions.IpRanges = $cidrBlocks
	Grant-EC2SecurityGroupIngress -GroupName myPSSecurityGroup -IpPermissions $ipPermissions

To verify the security group has been updated, run the :code:`Get-EC2SecurityGroup` cmdlet again.
You can't specify an outbound rule for EC2-Classic.

.. code-block:: none

    PS C:\> Get-EC2SecurityGroup -GroupNames myPSSecurityGroup
    
    OwnerId             : 123456789012
    GroupName           : myPSSecurityGroup
    GroupId             : sg-9cf9e5d9
    Description         : EC2-Classic from PowerShell
    IpPermissions       : {Amazon.EC2.Model.IpPermission}
    IpPermissionsEgress : {}
    VpcId               :
    Tags                : {}

To view the security group rule, use the :code:`IpPermissions` property.

.. code-block:: none

    PS C:\> (Get-EC2SecurityGroup -GroupNames myPSSecurityGroup).IpPermissions
    
    IpProtocol       : tcp
    FromPort         : 22
    ToPort           : 22
    UserIdGroupPairs : {}
    IpRanges         : {203.0.113.25/32}


.. _new-ec2securitygroup-vpc:

Creating a Security Group for EC2-VPC
-------------------------------------

The following example uses the :code:`New-EC2SecurityGroup` cmdlet to create a security group for
the specified VPC.

.. code-block:: none

    PS C:\> $groupid = New-EC2SecurityGroup -VpcId "vpc-da0013b3" -GroupName "myPSSecurityGroup" -GroupDescription "EC2-VPC from PowerShell"

To view the initial configuration of the security group, use the :code:`Get-EC2SecurityGroup`
cmdlet. By default, the security group for a VPC contains a rule that allows all outbound traffic.
Notice that you can't reference a security group for EC2-VPC by name.

.. code-block:: none

    PS C:\> Get-EC2SecurityGroup -GroupId sg-5d293231
    
    OwnerId             : 123456789012
    GroupName           : myPSSecurityGroup
    GroupId             : sg-5d293231
    Description         : EC2-VPC from PowerShell
    IpPermissions       : {}
    IpPermissionsEgress : {Amazon.EC2.Model.IpPermission}
    VpcId               : vpc-da0013b3
    Tags                : {}

To define the permissions for inbound traffic on TCP port 22 (SSH) and TCP port 3389, use the
:code:`New-Object` cmdlet, which works with PowerShell 2.0 and later. For example, here's how you
define permissions for TCP ports 22 and 3389 from a single IP address, :code:`203.0.113.25/32`.

.. code-block:: none

    PS C:\> $ip1 = new-object Amazon.EC2.Model.IpPermission $ip1.IpProtocol = "tcp" $ip1.FromPort = 22 $ip1.ToPort = 22 $ip1.IpRanges.Add("203.0.113.25/32") $ip2 = new-object Amazon.EC2.Model.IpPermission $ip2.IpProtocol = "tcp" $ip2.FromPort = 3389 $ip2.ToPort = 3389 $ip2.IpRanges.Add("203.0.113.25/32") Grant-EC2SecurityGroupIngress -GroupId $groupid -IpPermissions @( $ip1, $ip2 )

To verify the security group has been updated, use the :code:`Get-EC2SecurityGroup` cmdlet again.

.. code-block:: none

    PS C:\> Get-EC2SecurityGroup -GroupIds sg-5d293231
    
    OwnerId             : 123456789012
    GroupName           : myPSSecurityGroup
    GroupId             : sg-5d293231
    Description         : EC2-VPC from PowerShell
    IpPermissions       : {Amazon.EC2.Model.IpPermission}
    IpPermissionsEgress : {Amazon.EC2.Model.IpPermission}
    VpcId               : vpc-da0013b3
    Tags                : {}

To view the inbound rules, use the :code:`IpPermissions` property.

.. code-block:: none

    PS C:\> ($groupid | Get-EC2SecurityGroup).IpPermissions
    
    IpProtocol       : tcp
    FromPort         : 22
    ToPort           : 22
    UserIdGroupPairs : {}
    IpRanges         : {203.0.113.25/32}
    
    IpProtocol       : tcp
    FromPort         : 3389
    ToPort           : 3389
    UserIdGroupPairs : {}
    IpRanges         : {203.0.113.25/32}      
