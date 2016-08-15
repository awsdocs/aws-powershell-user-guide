.. _pstools-ec2-launch:

#################################################
Launch an |EC2| Instance Using Windows PowerShell
#################################################

To launch an Amazon EC2 instance, you need the key pair and security group that you created. You
also need the ID of an Amazon Machine Image (AMI). For more information, see the following
documentation:

* :ref:`pstools-ec2-keypairs`

* :ref:`pstools-ec2-sg`

* :ref:`pstools-ec2-get-amis`

If you launch an instance that is not within the Free Tier, you are billed after you launch the
instance and charged for the time that the instance is running even if it remains idle.


.. contents:: **Topics**
    :local:
    :depth: 1

.. _new-ec2instance:

Launching an Instance in EC2-Classic
------------------------------------

The following command creates a single :code:`t1.micro` instance.

.. code-block:: none

    PS C:\> New-EC2Instance -ImageId ami-c49c0dac -MinCount 1 -MaxCount 1 -KeyName myPSKeyPair -SecurityGroups myPSSecurityGroup -InstanceType t1.micro
    
    ReservationId   : r-b70a0ef1
    OwnerId         : 123456789012
    RequesterId     :
    Groups          : {myPSSecurityGroup}
    GroupName       : {myPSSecurityGroup}
    Instances       : {}

Your instance is in the :code:`pending` state initially, but will be in the :code:`running` state in
a few minutes. To view information about your instance, use the :code:`Get-EC2Instance` cmdlet. If
you have more than one instance, you can filter the results on the reservation ID using the
:code:`Filter` parameter. First, create an object of type :code:`Amazon.EC2.Model.Filter`. Next,
call :code:`Get-EC2Instance` with the filter and view the :code:`Instances` property.

.. code-block:: none

    PS C:\> $reservation = New-Object 'collections.generic.list[string]'
    $reservation.add("r-5caa4371")
    $filter_reservation = New-Object Amazon.EC2.Model.Filter -Property @{Name = "reservation-id"; Values = $reservation}
    (Get-EC2Instance -Filter $filter_reservation).Instances

The following is example output.

.. code-block:: none

    AmiLaunchIndex        : 0
    Architecture          : x86_64
    BlockDeviceMappings   : {/dev/sda1}
    ClientToken           : 
    EbsOptimized          : False
    Hypervisor            : xen
    IamInstanceProfile    : 
    ImageId               : ami-c49c0dac
    InstanceId            : i-5203422c
    InstanceLifecycle     :
    InstanceType          : t1.micro
    KernelId              :
    KeyName               : myPSKeyPair
    LaunchTime            : 12/2/2014 3:38:52 PM
    Monitoring            : Amazon.EC2.Model.Monitoring
    NetworkInterfaces     : {}
    Placement             : Amazon.EC2.Model.Placement
    Platform              : Windows
    PrivateDnsName        : 
    PrivateIpAddress      : 10.25.1.11
    ProductCodes          : {}
    PublicDnsName         :
    PublicIpAddress       : 198.51.100.245
    RamdiskId             :
    RootDeviceName        : /dev/sda1
    RootDeviceType        : ebs
    SecurityGroups        : {myPSSecurityGroup}
    SourceDestCheck       : True
    SpotInstanceRequestId :
    SriovNetSupport       :
    State                 : Amazon.EC2.Model.InstanceState
    StateReason           :
    StateTransitionReason :
    SubnetId              : 
    Tags                  : {}
    VirtualizationType    : hvm
    VpcId                 : 


.. _new-ec2instance-vpc:

Launching an Instance in a VPC
------------------------------

The following command creates a single :code:`m1.small` instance in the specified private subnet.
The security group must be one you created for the VPC that contains the specified subnet.

.. code-block:: none

    PS C:\> New-EC2Instance -ImageId ami-c49c0dac -MinCount 1 -MaxCount 1 -KeyName myPSKeyPair -SecurityGroupId sg-5d293231 -InstanceType m1.small -SubnetId subnet-d60013bf
    
    ReservationId   : r-b70a0ef1
    OwnerId         : 123456789012
    RequesterId     :
    Groups          : {}
    GroupName       : {}
    Instances       : {}

Your instance is in the :code:`pending` state initially, but will be in the :code:`running` state in
a few minutes. To view information about your instance, use the :code:`Get-EC2Instance` cmdlet. If
you have more than one instance, you can filter the results on the reservation ID using the
:code:`Filter` parameter. First, create an object of type :code:`Amazon.EC2.Model.Filter`. Next,
call :code:`Get-EC2Instance` with the filter and view the :code:`Instances` property.

.. code-block:: none

    PS C:\> $reservation = New-Object 'collections.generic.list[string]'
    $reservation.add("r-b70a0ef1")
    $filter_reservation = New-Object Amazon.EC2.Model.Filter -Property @{Name = "reservation-id"; Values = $reservation}
    (Get-EC2Instance -Filter $filter_reservation).Instances

The following is example output.

.. code-block:: none

    AmiLaunchIndex        : 0
    Architecture          : x86_64
    BlockDeviceMappings   : {/dev/sda1}
    ClientToken           : 
    EbsOptimized          : False
    Hypervisor            : xen
    IamInstanceProfile    : 
    ImageId               : ami-c49c0dac
    InstanceId            : i-5203422c
    InstanceLifecycle     :
    InstanceType          : m1.small
    KernelId              :
    KeyName               : myPSKeyPair
    LaunchTime            : 12/2/2014 3:38:52 PM
    Monitoring            : Amazon.EC2.Model.Monitoring
    NetworkInterfaces     : {}
    Placement             : Amazon.EC2.Model.Placement
    Platform              : Windows
    PrivateDnsName        : 
    PrivateIpAddress      : 10.25.1.11
    ProductCodes          : {}
    PublicDnsName         :
    PublicIpAddress       : 198.51.100.245
    RamdiskId             :
    RootDeviceName        : /dev/sda1
    RootDeviceType        : ebs
    SecurityGroups        : {myPSSecurityGroup}
    SourceDestCheck       : True
    SpotInstanceRequestId :
    SriovNetSupport       :
    State                 : Amazon.EC2.Model.InstanceState
    StateReason           :
    StateTransitionReason :
    SubnetId              : subnet-d60013bf
    Tags                  : {}
    VirtualizationType    : hvm
    VpcId                 : vpc-a01106c2


.. _new-ec2instance-spot:

Launching a Spot Instance in a VPC
----------------------------------

The following command requests a Spot Instance in the specified subnet. The security group must be
one you created for the VPC that contains the specified subnet.

.. code-block:: none

    PS C:\> $interface1 = New-Object Amazon.EC2.Model.InstanceNetworkInterfaceSpecification
    PS C:\> $interface1.DeviceIndex = 0
    PS C:\> $interface1.SubnetId = "subnet-b61f49f0"
    PS C:\> $interface1.PrivateIpAddress = "10.0.1.5"
    PS C:\> $interface1.Groups.Add("sg-5d293231")
    PS C:\> Request-EC2SpotInstance -SpotPrice 0.007 -InstanceCount 1 -Type one-time -LaunchSpecification_ImageId ami-7527031c -LaunchSpecification_InstanceType m1.small -Region us-west-2 -LaunchSpecification_NetworkInterfaces $interface1
