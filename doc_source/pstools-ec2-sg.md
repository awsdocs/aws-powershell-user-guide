# Create a Security Group Using Windows PowerShell<a name="pstools-ec2-sg"></a>

You can use the AWS Tools for PowerShell to create and configure a security group\. When you create a security group, you specify whether it is for EC2\-Classic or EC2\-VPC\. The response is the ID of the security group\.

If you need to connect to your instance, you must configure the security group to allow SSH traffic \(Linux\) or RDP traffic \(Windows\)\.

**Topics**
+ [Prerequisites](#sg-prerequisites)
+ [Creating a Security Group for EC2\-Classic](#get-ec2securitygroup)
+ [Creating a Security Group for EC2\-VPC](#new-ec2securitygroup-vpc)

## Prerequisites<a name="sg-prerequisites"></a>

You need the public IP address of your computer, in CIDR notation\. You can get the public IP address of your local computer using a service\. For example, Amazon provides the following service: [http://checkip\.amazonaws\.com/](http://checkip.amazonaws.com/) or [https://checkip\.amazonaws\.com/](https://checkip.amazonaws.com/)\. To locate another service that provides your IP address, use the search phrase "what is my IP address"\. If you are connecting through an ISP or from behind your firewall without a static IP address, you need to find the range of IP addresses that can be used by your client computers\.

**Warning**  
If you specify `0.0.0.0/0`, you are enabling traffic from any IP addresses in the world\. For the SSH and RDP protocols, you might consider this acceptable for a short time in a test environment, but it's unsafe for production environments\. In production, be sure to authorize access only from the appropriate individual IP address or range of addresses\.

## Creating a Security Group for EC2\-Classic<a name="get-ec2securitygroup"></a>

The following example uses the `New-EC2SecurityGroup` cmdlet to create a security group for `EC2-Classic`\.

```
PS > New-EC2SecurityGroup -GroupName myPSSecurityGroup -GroupDescription "EC2-Classic from PowerShell"

sg-0a346530123456789
```

To view the initial configuration of the security group, use the `Get-EC2SecurityGroup` cmdlet\.

```
PS > Get-EC2SecurityGroup -GroupNames myPSSecurityGroup

Description         : EC2-Classic from PowerShell
GroupId             : sg-0a346530123456789
GroupName           : myPSSecurityGroup
IpPermissions       : {}
IpPermissionsEgress : {Amazon.EC2.Model.IpPermission}
OwnerId             : 123456789012
Tags                : {}
VpcId               : vpc-9668ddef
```

To configure the security group to allow inbound traffic on TCP port 22 \(SSH\) and TCP port 3389, use the `Grant-EC2SecurityGroupIngress` cmdlet\. For example, the following example script shows how you could enable SSH traffic from a single IP address, `203.0.113.25/32`\.

```
$cidrBlocks = New-Object 'collections.generic.list[string]'
$cidrBlocks.add("203.0.113.25/32")
$ipPermissions = New-Object Amazon.EC2.Model.IpPermission
$ipPermissions.IpProtocol = "tcp"
$ipPermissions.FromPort = 22
$ipPermissions.ToPort = 22
ipPermissions.IpRanges = $cidrBlocks
Grant-EC2SecurityGroupIngress -GroupName myPSSecurityGroup -IpPermissions $ipPermissions
```

To verify the security group was updated, run the `Get-EC2SecurityGroup` cmdlet again\. Note that you can't specify an outbound rule for `EC2-Classic`\.

```
PS > Get-EC2SecurityGroup -GroupNames myPSSecurityGroup

OwnerId             : 123456789012
GroupName           : myPSSecurityGroup
GroupId             : sg-0a346530123456789
Description         : EC2-Classic from PowerShell
IpPermissions       : {Amazon.EC2.Model.IpPermission}
IpPermissionsEgress : {}
VpcId               :
Tags                : {}
```

To view the security group rule, use the `IpPermissions` property\.

```
PS > (Get-EC2SecurityGroup -GroupNames myPSSecurityGroup).IpPermissions

IpProtocol       : tcp
FromPort         : 22
ToPort           : 22
UserIdGroupPairs : {}
IpRanges         : {203.0.113.25/32}
```

## Creating a Security Group for EC2\-VPC<a name="new-ec2securitygroup-vpc"></a>

The following `New-EC2SecurityGroup` example adds the `-VpcId` parameter to create a security group for the specified VPC\.

```
PS > $groupid = New-EC2SecurityGroup `
    -VpcId "vpc-da0013b3" `
    -GroupName "myPSSecurityGroup" `
    -GroupDescription "EC2-VPC from PowerShell"
```

To view the initial configuration of the security group, use the `Get-EC2SecurityGroup` cmdlet\. By default, the security group for a VPC contains a rule that allows all outbound traffic\. Notice that you can't reference a security group for EC2\-VPC by name\.

```
PS > Get-EC2SecurityGroup -GroupId sg-5d293231

OwnerId             : 123456789012
GroupName           : myPSSecurityGroup
GroupId             : sg-5d293231
Description         : EC2-VPC from PowerShell
IpPermissions       : {}
IpPermissionsEgress : {Amazon.EC2.Model.IpPermission}
VpcId               : vpc-da0013b3
Tags                : {}
```

To define the permissions for inbound traffic on TCP port 22 \(SSH\) and TCP port 3389, use the `New-Object` cmdlet\. The following example script defines permissions for TCP ports 22 and 3389 from a single IP address, `203.0.113.25/32`\.

```
$ip1 = new-object Amazon.EC2.Model.IpPermission 
$ip1.IpProtocol = "tcp" 
$ip1.FromPort = 22 
$ip1.ToPort = 22 
$ip1.IpRanges.Add("203.0.113.25/32") 
$ip2 = new-object Amazon.EC2.Model.IpPermission 
$ip2.IpProtocol = "tcp" 
$ip2.FromPort = 3389 
$ip2.ToPort = 3389 
$ip2.IpRanges.Add("203.0.113.25/32") 
Grant-EC2SecurityGroupIngress -GroupId $groupid -IpPermissions @( $ip1, $ip2 )
```

To verify the security group has been updated, use the `Get-EC2SecurityGroup` cmdlet again\.

```
PS > Get-EC2SecurityGroup -GroupIds sg-5d293231

OwnerId             : 123456789012
GroupName           : myPSSecurityGroup
GroupId             : sg-5d293231
Description         : EC2-VPC from PowerShell
IpPermissions       : {Amazon.EC2.Model.IpPermission}
IpPermissionsEgress : {Amazon.EC2.Model.IpPermission}
VpcId               : vpc-da0013b3
Tags                : {}
```

To view the inbound rules, you can retrieve the `IpPermissions` property from the collection object returned by the previous command\.

```
PS > (Get-EC2SecurityGroup -GroupIds sg-5d293231).IpPermissions

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
```