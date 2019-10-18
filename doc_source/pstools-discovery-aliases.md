# Cmdlet Discovery and Aliases<a name="pstools-discovery-aliases"></a>

This section shows you how to list services that are supported by the AWS Tools for PowerShell, how to show the set of cmdlets provided by the AWS Tools for PowerShell in support of those services, and how to find alternative cmdlet names \(also called aliases\) to access those services\.

## Cmdlet Discovery<a name="pstools-cmdlet-discovery"></a>

All AWS service operations \(or APIs\) are documented in the API Reference Guide for each service\. For example, see the [IAM API Reference](https://docs.aws.amazon.com/IAM/latest/APIReference/)\. There is, in most cases, a one\-to\-one correspondence between an AWS service API and an AWS PowerShell cmdlet\. To get the cmdlet name that corresponds to an AWS service API name, run the AWS `Get-AWSCmdletName` cmdlet with the `-ApiOperation` parameter and the AWS service API name\. For example, to get all possible cmdlet names that are based on any available `DescribeInstances` AWS service API, run the following command:

```
PS > Get-AWSCmdletName -ApiOperation DescribeInstances

CmdletName        ServiceOperation    ServiceName                    CmdletNounPrefix
----------        ----------------    -----------                    ----------------
Get-EC2Instance   DescribeInstances   Amazon Elastic Compute Cloud   EC2
Get-GMLInstance   DescribeInstances   Amazon GameLift Service        GML
Get-OPSInstance   DescribeInstances   AWS OpsWorks                   OPS
```

The `-ApiOperation` parameter is the default parameter, so you can omit the parameter name\. The following example is equivalent to the previous one:

```
PS > Get-AWSCmdletName DescribeInstances
```

If you know the names of both the API and the service, you can include the `-Service` parameter along with either the cmdlet noun prefix or part of the AWS service name\. For example, the cmdlet noun prefix for Amazon EC2 is `EC2`\. To get the cmdlet name that corresponds to the `DescribeInstances` API in the Amazon EC2 service, run one of the following commands\. They are all result in the same output:

```
PS > Get-AWSCmdletName -ApiOperation DescribeInstances -Service EC2
PS > Get-AWSCmdletName -ApiOperation DescribeInstances -Service Compute
PS > Get-AWSCmdletName -ApiOperation DescribeInstances -Service "Compute Cloud"

CmdletName        ServiceOperation    ServiceName                    CmdletNounPrefix
----------        ----------------    -----------                    ----------------
Get-EC2Instance   DescribeInstances   Amazon Elastic Compute Cloud   EC2
```

Parameter values in these commands are case\-insensitive\.

If you do not know the name of either the desired AWS service API or the AWS service, you can use the `-ApiOperation` parameter, along with the pattern to match, and the `-MatchWithRegex` parameter\. For example, to get all available cmdlet names that contain `SecurityGroup`, run the following command:

```
PS > Get-AWSCmdletName -ApiOperation SecurityGroup -MatchWithRegex

CmdletName                                    ServiceOperation                            ServiceName                        CmdletNounPrefix
----------                                    ----------------                            -----------                        ----------------
Approve-ECCacheSecurityGroupIngress           AuthorizeCacheSecurityGroupIngress          Amazon ElastiCache                 EC
Get-ECCacheSecurityGroup                      DescribeCacheSecurityGroups                 Amazon ElastiCache                 EC
New-ECCacheSecurityGroup                      CreateCacheSecurityGroup                    Amazon ElastiCache                 EC
Remove-ECCacheSecurityGroup                   DeleteCacheSecurityGroup                    Amazon ElastiCache                 EC
Revoke-ECCacheSecurityGroupIngress            RevokeCacheSecurityGroupIngress             Amazon ElastiCache                 EC
Add-EC2SecurityGroupToClientVpnTargetNetwrk   ApplySecurityGroupsToClientVpnTargetNetwork Amazon Elastic Compute Cloud       EC2
Get-EC2SecurityGroup                          DescribeSecurityGroups                      Amazon Elastic Compute Cloud       EC2
Get-EC2SecurityGroupReference                 DescribeSecurityGroupReferences             Amazon Elastic Compute Cloud       EC2
Get-EC2StaleSecurityGroup                     DescribeStaleSecurityGroups                 Amazon Elastic Compute Cloud       EC2
Grant-EC2SecurityGroupEgress                  AuthorizeSecurityGroupEgress                Amazon Elastic Compute Cloud       EC2
Grant-EC2SecurityGroupIngress                 AuthorizeSecurityGroupIngress               Amazon Elastic Compute Cloud       EC2
New-EC2SecurityGroup                          CreateSecurityGroup                         Amazon Elastic Compute Cloud       EC2
Remove-EC2SecurityGroup                       DeleteSecurityGroup                         Amazon Elastic Compute Cloud       EC2
Revoke-EC2SecurityGroupEgress                 RevokeSecurityGroupEgress                   Amazon Elastic Compute Cloud       EC2
Revoke-EC2SecurityGroupIngress                RevokeSecurityGroupIngress                  Amazon Elastic Compute Cloud       EC2
Update-EC2SecurityGroupRuleEgressDescription  UpdateSecurityGroupRuleDescriptionsEgress   Amazon Elastic Compute Cloud       EC2
Update-EC2SecurityGroupRuleIngressDescription UpdateSecurityGroupRuleDescriptionsIngress  Amazon Elastic Compute Cloud       EC2
Edit-EFSMountTargetSecurityGroup              ModifyMountTargetSecurityGroups             Amazon Elastic File System         EFS
Get-EFSMountTargetSecurityGroup               DescribeMountTargetSecurityGroups           Amazon Elastic File System         EFS
Join-ELBSecurityGroupToLoadBalancer           ApplySecurityGroupsToLoadBalancer           Elastic Load Balancing             ELB
Set-ELB2SecurityGroup                         SetSecurityGroups                           Elastic Load Balancing V2          ELB2
Get-EMLInputSecurityGroup                     DescribeInputSecurityGroup                  AWS Elemental MediaLive            EML
Get-EMLInputSecurityGroupList                 ListInputSecurityGroups                     AWS Elemental MediaLive            EML
New-EMLInputSecurityGroup                     CreateInputSecurityGroup                    AWS Elemental MediaLive            EML
Remove-EMLInputSecurityGroup                  DeleteInputSecurityGroup                    AWS Elemental MediaLive            EML
Update-EMLInputSecurityGroup                  UpdateInputSecurityGroup                    AWS Elemental MediaLive            EML
Enable-RDSDBSecurityGroupIngress              AuthorizeDBSecurityGroupIngress             Amazon Relational Database Service RDS
Get-RDSDBSecurityGroup                        DescribeDBSecurityGroups                    Amazon Relational Database Service RDS
New-RDSDBSecurityGroup                        CreateDBSecurityGroup                       Amazon Relational Database Service RDS
Remove-RDSDBSecurityGroup                     DeleteDBSecurityGroup                       Amazon Relational Database Service RDS
Revoke-RDSDBSecurityGroupIngress              RevokeDBSecurityGroupIngress                Amazon Relational Database Service RDS
Approve-RSClusterSecurityGroupIngress         AuthorizeClusterSecurityGroupIngress        Amazon Redshift                    RS
Get-RSClusterSecurityGroup                    DescribeClusterSecurityGroups               Amazon Redshift                    RS
New-RSClusterSecurityGroup                    CreateClusterSecurityGroup                  Amazon Redshift                    RS
Remove-RSClusterSecurityGroup                 DeleteClusterSecurityGroup                  Amazon Redshift                    RS
Revoke-RSClusterSecurityGroupIngress          RevokeClusterSecurityGroupIngress           Amazon Redshift                    RS
```

If you know the name of the AWS service but not the AWS service API, include both the `-MatchWithRegex` parameter and the `-Service` parameter to scope the search down to a single service\. For example, to get all cmdlet names that contain `SecurityGroup` in only the Amazon EC2 service, run the following command

```
PS > Get-AWSCmdletName -ApiOperation SecurityGroup -MatchWithRegex -Service EC2

CmdletName                                    ServiceOperation                            ServiceName                  CmdletNounPrefix
----------                                    ----------------                            -----------                  ----------------
Add-EC2SecurityGroupToClientVpnTargetNetwrk   ApplySecurityGroupsToClientVpnTargetNetwork Amazon Elastic Compute Cloud EC2
Get-EC2SecurityGroup                          DescribeSecurityGroups                      Amazon Elastic Compute Cloud EC2
Get-EC2SecurityGroupReference                 DescribeSecurityGroupReferences             Amazon Elastic Compute Cloud EC2
Get-EC2StaleSecurityGroup                     DescribeStaleSecurityGroups                 Amazon Elastic Compute Cloud EC2
Grant-EC2SecurityGroupEgress                  AuthorizeSecurityGroupEgress                Amazon Elastic Compute Cloud EC2
Grant-EC2SecurityGroupIngress                 AuthorizeSecurityGroupIngress               Amazon Elastic Compute Cloud EC2
New-EC2SecurityGroup                          CreateSecurityGroup                         Amazon Elastic Compute Cloud EC2
Remove-EC2SecurityGroup                       DeleteSecurityGroup                         Amazon Elastic Compute Cloud EC2
Revoke-EC2SecurityGroupEgress                 RevokeSecurityGroupEgress                   Amazon Elastic Compute Cloud EC2
Revoke-EC2SecurityGroupIngress                RevokeSecurityGroupIngress                  Amazon Elastic Compute Cloud EC2
Update-EC2SecurityGroupRuleEgressDescription  UpdateSecurityGroupRuleDescriptionsEgress   Amazon Elastic Compute Cloud EC2
Update-EC2SecurityGroupRuleIngressDescription UpdateSecurityGroupRuleDescriptionsIngress  Amazon Elastic Compute Cloud EC2
```

If you know the name of the AWS Command Line Interface \(AWS CLI\) command, you can use the `-AwsCliCommand` parameter and the desired AWS CLI command name to get the name of the cmdlet that's based on the same API\. For example, to get the cmdlet name that corresponds to the `authorize-security-group-ingress` AWS CLI command call in the Amazon EC2 service, run the following command:

```
PS > Get-AWSCmdletName -AwsCliCommand "aws ec2 authorize-security-group-ingress"

CmdletName                    ServiceOperation              ServiceName                  CmdletNounPrefix
----------                    ----------------              -----------                  ----------------
Grant-EC2SecurityGroupIngress AuthorizeSecurityGroupIngress Amazon Elastic Compute Cloud EC2
```

The `Get-AWSCmdletName` cmdlet needs only enough of the AWS CLI command name to identify the service and the AWS API\. 

To get a list of all of the cmdlets in the Tools for PowerShell Core, run the PowerShell `Get-Command` cmdlet, as shown in the following example\.

```
PS > Get-Command -Module AWSPowerShell.NetCore
```

You can run the same command with `-Module AWSPowerShell` to see the cmdlets in the AWS Tools for Windows PowerShell\.

The `Get-Command` cmdlet generates the list of cmdlets in alphabetical order\. Note that by default the list is sorted by PowerShell verb, rather than PowerShell noun\.

To sort results by service instead, run the following command:

```
PS > Get-Command -Module AWSPowerShell.NetCore | Sort-Object Noun,Verb
```

To filter the cmdlets that are returned by the `Get-Command` cmdlet, pipe the output to the PowerShell `Select-String` cmdlet\. For example, to view the set of cmdlets that work with AWS regions, run the following command:

```
PS > Get-Command -Module AWSPowerShell.NetCore | Select-String region

Clear-DefaultAWSRegion
Copy-HSM2BackupToRegion
Get-AWSRegion
Get-DefaultAWSRegion
Get-EC2Region
Get-LSRegionList
Get-RDSSourceRegion
Set-DefaultAWSRegion
```

You can also find cmdlets for a specific service by filtering for the service prefix of cmdlet nouns\. To see the list of available service prefixes, run `Get-AWSPowerShellVersion -ListServiceVersionInfo`\. The following example returns cmdlets that support the Amazon CloudWatch Events service\.

```
PS > Get-Command -Module AWSPowerShell -Noun CWE*

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Add-CWEResourceTag                                 3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Disable-CWEEventSource                             3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Disable-CWERule                                    3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Enable-CWEEventSource                              3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Enable-CWERule                                     3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEEventBus                                    3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEEventBusList                                3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEEventSource                                 3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEEventSourceList                             3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEPartnerEventSource                          3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEPartnerEventSourceAccountList               3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEPartnerEventSourceList                      3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWEResourceTag                                 3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWERule                                        3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWERuleDetail                                  3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWERuleNamesByTarget                           3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Get-CWETargetsByRule                               3.3.563.1  AWSPowerShell.NetCore
Cmdlet          New-CWEEventBus                                    3.3.563.1  AWSPowerShell.NetCore
Cmdlet          New-CWEPartnerEventSource                          3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWEEventBus                                 3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWEPartnerEventSource                       3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWEPermission                               3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWEResourceTag                              3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWERule                                     3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Remove-CWETarget                                   3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Test-CWEEventPattern                               3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Write-CWEEvent                                     3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Write-CWEPartnerEvent                              3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Write-CWEPermission                                3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Write-CWERule                                      3.3.563.1  AWSPowerShell.NetCore
Cmdlet          Write-CWETarget                                    3.3.563.1  AWSPowerShell.NetCore
```

## Cmdlet Naming and Aliases<a name="pstools-cmdlet-naming-aliases"></a>

The cmdlets in the AWS Tools for PowerShell for each service are based on the methods provided by the AWS SDK for the service\. However, because of PowerShell's mandatory naming conventions, the name of a cmdlet might be different from the name of the API call or method on which it is based\. For example, the `Get-EC2Instance` cmdlet is based on the Amazon EC2`DescribeInstances` method\.

In some cases, the cmdlet name may be similar to a method name, but it may actually perform a different function\. For example, the Amazon S3`GetObject` method retrieves an Amazon S3 object\. However, the `Get-S3Object` cmdlet returns *information* about an Amazon S3 object rather than the object itself\.

```
PS > Get-S3Object -BucketName text-content -Key aws-tech-docs

ETag         : "df000002a0fe0000f3c000004EXAMPLE"
BucketName   : aws-tech-docs
Key          : javascript/frameset.js
LastModified : 6/13/2011 1:24:18 PM
Owner        : Amazon.S3.Model.Owner
Size         : 512
StorageClass : STANDARD
```

To get an S3 object with the AWS Tools for PowerShell, run the `Read-S3Object` cmdlet:

```
PS > Read-S3Object -BucketName text-content -Key text-object.txt -file c:\tmp\text-object-download.text

Mode          LastWriteTime            Length Name
----          -------------            ------ ----
-a---         11/5/2012   7:29 PM      20622  text-object-download.text
```

**Note**  
The cmdlet help for an AWS cmdlet provides the name of the AWS SDK API on which the cmdlet is based\.  
For more information about standard PowerShell verbs and their meanings, see [Approved Verbs for PowerShell Commands](https://docs.microsoft.com/en-us/powershell/developer/cmdlet/approved-verbs-for-windows-powershell-commands)\.

All AWS cmdlets that use the `Remove` verb – and the `Stop-EC2Instance` cmdlet when you add the `-Terminate` parameter – prompt for confirmation before proceeding\. To bypass confirmation, add the `-Force` parameter to your command\.

**Important**  
AWS cmdlets do not support the `-WhatIf` switch\.

### Aliases<a name="pstools-aliases"></a>

Setup of the AWS Tools for PowerShell installs an aliases file that contains aliases for many of the AWS cmdlets\. You might find these aliases to be more intuitive than the cmdlet names\. For example, service names and AWS SDK method names replace PowerShell verbs and nouns in some aliases\. An example is the `EC2-DescribeInstances` alias\.

Other aliases use verbs that, though they do not follow standard PowerShell conventions, can be more descriptive of the actual operation\. For example, the alias file maps the alias `Get-S3Content` to the cmdlet `Read-S3Object`\.

```
PS > Set-Alias -Name Get-S3Content -Value Read-S3Object
```

The aliases file is located in the AWS Tools for PowerShell installation directory\. To load the aliases into your environment, *dot\-source* the file\. The following is a Windows\-based example\.

```
PS > . "C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowershell\AWSAliases.ps1"
```

For a Linux or macOS shell, it might look like this:

```
. ~/.local/share/powershell/Modules/AWSPowerShell.NetCore/3.3.563.1/AWSAliases.ps1
```

To show all AWS Tools for PowerShell aliases, run the following command\. This command uses the `?` alias for the PowerShell `Where-Object` cmdlet and the `Source` property to filter for only aliases that come from the `AWSPowerShell.NetCore` module\.

```
PS > Get-Alias | ? Source -like "AWSPowerShell.NetCore"

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           Add-ASInstances                                    3.3.343.0  AWSPowerShell
Alias           Add-CTTag                                          3.3.343.0  AWSPowerShell
Alias           Add-DPTags                                         3.3.343.0  AWSPowerShell
Alias           Add-DSIpRoutes                                     3.3.343.0  AWSPowerShell
Alias           Add-ELBTags                                        3.3.343.0  AWSPowerShell
Alias           Add-EMRTag                                         3.3.343.0  AWSPowerShell
Alias           Add-ESTag                                          3.3.343.0  AWSPowerShell
Alias           Add-MLTag                                          3.3.343.0  AWSPowerShell
Alias           Clear-AWSCredentials                               3.3.343.0  AWSPowerShell
Alias           Clear-AWSDefaults                                  3.3.343.0  AWSPowerShell
Alias           Dismount-ASInstances                               3.3.343.0  AWSPowerShell
Alias           Edit-EC2Hosts                                      3.3.343.0  AWSPowerShell
Alias           Edit-RSClusterIamRoles                             3.3.343.0  AWSPowerShell
Alias           Enable-ORGAllFeatures                              3.3.343.0  AWSPowerShell
Alias           Find-CTEvents                                      3.3.343.0  AWSPowerShell
Alias           Get-ASACases                                       3.3.343.0  AWSPowerShell
Alias           Get-ASAccountLimits                                3.3.343.0  AWSPowerShell
Alias           Get-ASACommunications                              3.3.343.0  AWSPowerShell
Alias           Get-ASAServices                                    3.3.343.0  AWSPowerShell
Alias           Get-ASASeverityLevels                              3.3.343.0  AWSPowerShell
Alias           Get-ASATrustedAdvisorCheckRefreshStatuses          3.3.343.0  AWSPowerShell
Alias           Get-ASATrustedAdvisorChecks                        3.3.343.0  AWSPowerShell
Alias           Get-ASATrustedAdvisorCheckSummaries                3.3.343.0  AWSPowerShell
Alias           Get-ASLifecycleHooks                               3.3.343.0  AWSPowerShell
Alias           Get-ASLifecycleHookTypes                           3.3.343.0  AWSPowerShell
Alias           Get-AWSCredentials                                 3.3.343.0  AWSPowerShell
Alias           Get-CDApplications                                 3.3.343.0  AWSPowerShell
Alias           Get-CDDeployments                                  3.3.343.0  AWSPowerShell
Alias           Get-CFCloudFrontOriginAccessIdentities             3.3.343.0  AWSPowerShell
Alias           Get-CFDistributions                                3.3.343.0  AWSPowerShell
Alias           Get-CFGConfigRules                                 3.3.343.0  AWSPowerShell
Alias           Get-CFGConfigurationRecorders                      3.3.343.0  AWSPowerShell
Alias           Get-CFGDeliveryChannels                            3.3.343.0  AWSPowerShell
Alias           Get-CFInvalidations                                3.3.343.0  AWSPowerShell
Alias           Get-CFNAccountLimits                               3.3.343.0  AWSPowerShell
Alias           Get-CFNStackEvents                                 3.3.343.0  AWSPowerShell

...
```

To add your own aliases to this file, you might need to raise the value of PowerShell's `$MaximumAliasCount` [preference variable](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-6) to a value greater than 5500\. The default value is 4096; you can raise it to a maximum of 32768\. To do this, run the following\.

```
PS > $MaximumAliasCount = 32768
```

To verify that your change was successful, enter the variable name to show its current value\.

```
PS > $MaximumAliasCount
32768
```