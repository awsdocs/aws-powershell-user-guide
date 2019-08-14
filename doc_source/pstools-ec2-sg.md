# Create a Security Group Using Windows PowerShell<a name="pstools-ec2-sg"></a>

You can use the AWS Tools for Windows PowerShell to create and configure a security group\. When you create a security group, you specify whether it is for EC2\-Classic or EC2\-VPC\. The response is the ID of the security group\.

If you need to connect to your instance, you must configure the security group to allow SSH traffic \(Linux\) or RDP traffic \(Windows\)\.

**Topics**
+ [Prerequisites](#sg-prerequisites)
+ [Creating a Security Group for EC2\-Classic](#get-ec2securitygroup)
+ [Creating a Security Group for EC2\-VPC](#new-ec2securitygroup-vpc)

## Prerequisites<a name="sg-prerequisites"></a>

You need the public IP address of your computer, in CIDR notation\. You can get the public IP address of your local computer using a service\. For example, we provide the following service: [http://checkip\.amazonaws\.com/](http://checkip.amazonaws.com/) or [https://checkip\.amazonaws\.com/](https://checkip.amazonaws.com/)\. To locate another service that provides your IP address, use the search phrase "what is my IP address"\. If you are connecting through an ISP or from behind your firewall without a static IP address, you need to find the range of IP addresses used by client computers\.

If you use `0.0.0.0/0`, you enable all IP addresses to access your instance\. For the SSH and RDP protocols, this is acceptable for a short time in a test environment, but it's unsafe for production environments\. In production, you'll authorize only a specific IP address or range of addresses to access your instance\.

## Creating a Security Group for EC2\-Classic<a name="get-ec2securitygroup"></a>

The following example uses the `New-EC2SecurityGroup` cmdlet to create a security group for EC2\-Classic\.

```
PS > New-EC2SecurityGroup -GroupName myPSSecurityGroup -GroupDescription "EC2-Classic from PowerShell"

sg-9cf9e5d9
```

To view the initial configuration of the security group, use the `Get-EC2SecurityGroup` cmdlet\.

```
PS > Get-EC2SecurityGroup -GroupNames myPSSecurityGroup

OwnerId             : 123456789012
GroupName           : myPSSecurityGroup
GroupId             : sg-9cf9e5d9
Description         : EC2-Classic from PowerShell
IpPermissions       : {}
IpPermissionsEgress : {}
VpcId               :
Tags                : {}
```

To configure the security group to allow inbound traffic on TCP port 22 \(SSH\) and TCP port 3389, use the `Grant-EC2SecurityGroupIngress` cmdlet\. For example, the following script shows how you enable SSH traffic from a single IP address, `203.0.113.25/32`\.

```
PS > $cidrBlocks = New-Object 'collections.generic.list[string]'
      $cidrBlocks.add("203.0.113.25/32")
      $ipPermissions = New-Object Amazon.EC2.Model.IpPermission
      $ipPermissions.IpProtocol = "tcp"
      $ipPermissions.FromPort = 22
      $ipPermissions.ToPort = 22
      $ipPermissions.IpRanges = $cidrBlocks
      Grant-EC2SecurityGroupIngress -GroupName myPSSecurityGroup -IpPermissions $ipPermissions
```

To verify the security group has been updated, run the `Get-EC2SecurityGroup` cmdlet again\. You can't specify an outbound rule for EC2\-Classic\.

```
PS > Get-EC2SecurityGroup -GroupNames myPSSecurityGroup

OwnerId             : 123456789012
GroupName           : myPSSecurityGroup
GroupId             : sg-9cf9e5d9
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

The following example uses the `New-EC2SecurityGroup` cmdlet to create a security group for the specified VPC\.

```
PS > $groupid = New-EC2SecurityGroup -VpcId "vpc-da0013b3" -GroupName "myPSSecurityGroup" -GroupDescription "EC2-VPC from PowerShell"
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

To define the permissions for inbound traffic on TCP port 22 \(SSH\) and TCP port 3389, use the `New-Object` cmdlet, which works with PowerShell 2\.0 and later\. For example, here's how you define permissions for TCP ports 22 and 3389 from a single IP address, `203.0.113.25/32`\.

```
PS > $ip1 = new-object Amazon.EC2.Model.IpPermission $ip1.IpProtocol = "tcp" $ip1.FromPort = 22 $ip1.ToPort = 22 $ip1.IpRanges.Add("203.0.113.25/32") $ip2 = new-object Amazon.EC2.Model.IpPermission $ip2.IpProtocol = "tcp" $ip2.FromPort = 3389 $ip2.ToPort = 3389 $ip2.IpRanges.Add("203.0.113.25/32") Grant-EC2SecurityGroupIngress -GroupId $groupid -IpPermissions @( $ip1, $ip2 )
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

To view the inbound rules, use the `IpPermissions` property\.

```
PS > ($groupid | Get-EC2SecurityGroup).IpPermissions

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