.. _pstools-ec2-get-amis:

#####################################################
Find an Amazon Machine Image Using Windows PowerShell
#####################################################

When you launch an Amazon EC2 instance, you need to specify an Amazon Machine Image (AMI) to serve
as a template for the instance. However, the IDs for the AWS Windows AMIs change monthly because AWS
provides new AMIs with the latest updates and security enhancements. You can use the `Get-EC2Image
<http://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2Image.html>`_ and `Get-EC2ImageByName <http://docs.aws.amazon.com/powershell/latest/reference/items/Get-EC2ImageByName.html>`_ cmdlets to
find the current Windows AMIs and get their IDs.


.. contents:: **Topics**
    :local:
    :depth: 1

.. _pstools-ec2-get-image:

Get-EC2Image
------------

The :code:`Get-EC2Image` cmdlet retrieves a list of AMIs that you can use.

Use the :code:`-Owner` parameter with the array value :code:`amazon, self` so that
:code:`Get-EC2Image` retrieves only AMIs that belong to Amazon or to you. In this context, *you*
refers to the user who corresponds to the credentials with which the cmdlet is invoked.

.. code-block:: none

    PS C:\> Get-EC2Image -Owner amazon, self

You can scope the results using the :code:`-Filter` parameter. To specify the filter, create an
object of type :code:`Amazon.EC2.Model.Filter`. For example, use the following filter to display
only Windows AMIs. (To test this example, copy all four lines and paste them into the Windows
PowerShell for AWS window.)

.. code-block:: none

    PS C:\> $platform_values = New-Object 'collections.generic.list[string]' $platform_values.add("windows") $filter_platform = New-Object Amazon.EC2.Model.Filter -Property @{Name = "platform"; Values = $platform_values} Get-EC2Image -Owner amazon, self -Filter $filter_platform`

The following is an example of one of the AMIs returned by the cmdlet; the actual output of the
previous command provides information for many AMIs.

.. code-block:: none

    Architecture        : x86_64
    BlockDeviceMappings : {/dev/sda1, xvdca, xvdcb, xvdcc...}
    Description         : Microsoft Windows Server 2012 RTM 64-bit Locale English Base AMI provided by Amazon
    Hypervisor          : xen
    ImageId             : ami-e49a0b8c
    ImageLocation       : amazon/Windows_Server-2012-RTM-English-64Bit-Base-2014.11.19
    ImageOwnerAlias     : amazon
    ImageType           : machine
    KernelId            :
    Name                : Windows_Server-2012-RTM-English-64Bit-Base-2014.11.19
    OwnerId             : 801119661308
    Platform            : Windows
    ProductCodes        : {}
    Public              : True
    RamdiskId           :
    RootDeviceName      : /dev/sda1
    RootDeviceType      : ebs
    SriovNetSupport     :
    State               : available
    StateReason         :
    Tags                : {}
    VirtualizationType  : hvm


.. _pstools-ec2-get-ec2imagebyname:

Get-EC2ImageByName
------------------

The :code:`Get-EC2ImageByName` cmdlet enables you to filter the list of AWS Windows AMIs based on
the type of server configuration you are interested in.

When run with no parameters, as follows, the cmdlet emits the complete set of current filter names.

.. code-block:: none

    PS C:\> Get-EC2ImageByName
    
    WINDOWS_2016_BASE
    WINDOWS_2016_NANO
    WINDOWS_2016_CORE
    WINDOWS_2016_CONTAINER
    WINDOWS_2016_SQL_SERVER_ENTERPRISE_2016
    WINDOWS_2016_SQL_SERVER_STANDARD_2016
    WINDOWS_2016_SQL_SERVER_WEB_2016
    WINDOWS_2016_SQL_SERVER_EXPRESS_2016
    WINDOWS_2012R2_BASE
    WINDOWS_2012R2_CORE
    WINDOWS_2012R2_SQL_SERVER_EXPRESS_2016
    WINDOWS_2012R2_SQL_SERVER_STANDARD_2016
    WINDOWS_2012R2_SQL_SERVER_WEB_2016
    WINDOWS_2012R2_SQL_SERVER_EXPRESS_2014
    WINDOWS_2012R2_SQL_SERVER_STANDARD_2014
    WINDOWS_2012R2_SQL_SERVER_WEB_2014
    WINDOWS_2012_BASE
    WINDOWS_2012_SQL_SERVER_EXPRESS_2014
    WINDOWS_2012_SQL_SERVER_STANDARD_2014
    WINDOWS_2012_SQL_SERVER_WEB_2014
    WINDOWS_2012_SQL_SERVER_EXPRESS_2012
    WINDOWS_2012_SQL_SERVER_STANDARD_2012
    WINDOWS_2012_SQL_SERVER_WEB_2012
    WINDOWS_2012_SQL_SERVER_EXPRESS_2008
    WINDOWS_2012_SQL_SERVER_STANDARD_2008
    WINDOWS_2012_SQL_SERVER_WEB_2008
    WINDOWS_2008R2_BASE
    WINDOWS_2008R2_SQL_SERVER_EXPRESS_2012
    WINDOWS_2008R2_SQL_SERVER_STANDARD_2012
    WINDOWS_2008R2_SQL_SERVER_WEB_2012
    WINDOWS_2008R2_SQL_SERVER_EXPRESS_2008
    WINDOWS_2008R2_SQL_SERVER_STANDARD_2008
    WINDOWS_2008R2_SQL_SERVER_WEB_2008
    WINDOWS_2008RTM_BASE
    WINDOWS_2008RTM_SQL_SERVER_EXPRESS_2008
    WINDOWS_2008RTM_SQL_SERVER_STANDARD_2008
    WINDOWS_2008_BEANSTALK_IIS75
    WINDOWS_2012_BEANSTALK_IIS8
    VPC_NAT

To narrow the set of images returned, specify one or more filter names using the :code:`Names`
parameter.

.. code-block:: none

    PS C:\> Get-EC2ImageByName -Names WINDOWS_2012R2_SQL_SERVER_EXPRESS_2014
    
    Architecture        : x86_64
    BlockDeviceMappings : {/dev/sda1, xvdca, xvdcb, xvdcc...}
    Description         : Microsoft Windows Server 2012 R2 RTM 64-bit Locale English with SQL 2014 Express AMI provided by Amazon
    Hypervisor          : xen
    ImageId             : ami-de9c0db6
    ImageLocation       : amazon/Windows_Server-2012-R2_RTM-English-64Bit-SQL_2014_RTM_Express-2014.11.19
    ImageOwnerAlias     : amazon
    ImageType           : machine
    KernelId            :
    Name                : Windows_Server-2012-R2_RTM-English-64Bit-SQL_2014_RTM_Express-2014.11.19
    OwnerId             : 801119661308
    Platform            : Windows
    ProductCodes        : {}
    Public              : True
    RamdiskId           :
    RootDeviceName      : /dev/sda1
    RootDeviceType      : ebs
    SriovNetSupport     : simple
    State               : available
    StateReason         :
    Tags                : {}
    VirtualizationType  : hvm
