.. _pstools-discovery-aliases:

############################
Cmdlet Discovery and Aliases
############################

This section shows you how to list services that are supported by the |TWPALLlong|, how to show the set of cmdlets provided by
the |TWPALL| in support of those services, and how to find alternative cmdlet names (also called aliases) for accessing those
services.

.. _pstools-cmdlet-discovery:

Cmdlet Discovery
----------------

To get the cmdlet name that corresponds to an AWS service API name, run the AWS
:code:`Get-AWSCmdletName` cmdlet with the :code:`-ApiOperation` parameter and the AWS service
API name. For example, to get all possible cmdlet names that are based on any available
:code:`DescribeInstances` AWS service API, run the following command.

.. code-block:: none

    PS C:\> Get-AWSCmdletName -ApiOperation DescribeInstances
    
    CmdletName              ServiceOperation         ServiceName                         CmdletNounPrefix
    ----------              ----------------         -----------                         ----------------
    Get-EC2Instance         DescribeInstances        Amazon Elastic Compute Cloud        EC2
    Get-OPSInstance         DescribeInstances        AWS OpsWorks                        OPS

You can omit the :code:`-ApiOperation` parameter name in the preceding call, and just run the following.

.. code-block:: none

    PS C:\> Get-AWSCmdletName DescribeInstances

If you know the names of both the AWS service API and the AWS service, add the
:code:`-Service` parameter along with either the cmdlet noun prefix or part of the AWS
service name. (For example, the cmdlet noun prefix for |EC2| is :code:`EC2`.) To get
the cmdlet name that is based on the :code:`DescribeInstances` API in the |EC2| service, run one
of the following commands.

.. code-block:: none

    PS C:\> Get-AWSCmdletName -ApiOperation DescribeInstances -Service EC2
    PS C:\> Get-AWSCmdletName -ApiOperation DescribeInstances -Service Compute
    PS C:\> Get-AWSCmdletName -ApiOperation DescribeInstances -Service "Compute Cloud"
    
    CmdletName              ServiceOperation         ServiceName                         CmdletNounPrefix
    ----------              ----------------         -----------                         ----------------
    Get-EC2Instance         DescribeInstances        Amazon Elastic Compute Cloud        EC2

Parameter values in these commands are case-insensitive.

If you do not know the name of either the desired AWS service API or the AWS service, add the
:code:`-ApiOperation` parameter, along with the pattern to match, and the :code:`-MatchWithRegex`
parameter. For example, to get all available cmdlet names that contain :code:`SecurityGroup`, run the following command.

.. code-block:: none

    PS C:\> Get-AWSCmdletName -ApiOperation SecurityGroup -MatchWithRegex
    
    CmdletName                                    ServiceOperation                           ServiceName
    ----------                                    ----------------                           -----------
    Approve-ECCacheSecurityGroupIngress           AuthorizeCacheSecurityGroupIngress         Amazon ElastiCache
    Get-ECCacheSecurityGroup                      DescribeCacheSecurityGroups                Amazon ElastiCache
    New-ECCacheSecurityGroup                      CreateCacheSecurityGroup                   Amazon ElastiCache
    Remove-ECCacheSecurityGroup                   DeleteCacheSecurityGroup                   Amazon ElastiCache
    Revoke-ECCacheSecurityGroupIngress            RevokeCacheSecurityGroupIngress            Amazon ElastiCache
    Get-EC2SecurityGroup                          DescribeSecurityGroups                     Amazon Elastic Compute Cloud
    Get-EC2SecurityGroupReference                 DescribeSecurityGroupReferences            Amazon Elastic Compute Cloud
    Get-EC2StaleSecurityGroup                     DescribeStaleSecurityGroups                Amazon Elastic Compute Cloud
    Grant-EC2SecurityGroupEgress                  AuthorizeSecurityGroupEgress               Amazon Elastic Compute Cloud
    Grant-EC2SecurityGroupIngress                 AuthorizeSecurityGroupIngress              Amazon Elastic Compute Cloud
    New-EC2SecurityGroup                          CreateSecurityGroup                        Amazon Elastic Compute Cloud
    Remove-EC2SecurityGroup                       DeleteSecurityGroup                        Amazon Elastic Compute Cloud
    Revoke-EC2SecurityGroupEgress                 RevokeSecurityGroupEgress                  Amazon Elastic Compute Cloud
    Revoke-EC2SecurityGroupIngress                RevokeSecurityGroupIngress                 Amazon Elastic Compute Cloud
    Update-EC2SecurityGroupRuleEgressDescription  UpdateSecurityGroupRuleDescriptionsEgress  Amazon Elastic Compute Cloud
    Update-EC2SecurityGroupRuleIngressDescription UpdateSecurityGroupRuleDescriptionsIngress Amazon Elastic Compute Cloud
    Edit-EFSMountTargetSecurityGroup              ModifyMountTargetSecurityGroups            Amazon Elastic File System
    Get-EFSMountTargetSecurityGroup               DescribeMountTargetSecurityGroups          Amazon Elastic File System
    Join-ELBSecurityGroupToLoadBalancer           ApplySecurityGroupsToLoadBalancer          Elastic Load Balancing
    Set-ELB2SecurityGroup                         SetSecurityGroups                          Elastic Load Balancing V2
    Get-EMLInputSecurityGroup                     DescribeInputSecurityGroup                 AWS Elemental MediaLive
    Get-EMLInputSecurityGroupList                 ListInputSecurityGroups                    AWS Elemental MediaLive
    New-EMLInputSecurityGroup                     CreateInputSecurityGroup                   AWS Elemental MediaLive
    Remove-EMLInputSecurityGroup                  DeleteInputSecurityGroup                   AWS Elemental MediaLive
    Update-EMLInputSecurityGroup                  UpdateInputSecurityGroup                   AWS Elemental MediaLive
    Enable-RDSDBSecurityGroupIngress              AuthorizeDBSecurityGroupIngress            Amazon Relational Database ...
    Get-RDSDBSecurityGroup                        DescribeDBSecurityGroups                   Amazon Relational Database ...
    New-RDSDBSecurityGroup                        CreateDBSecurityGroup                      Amazon Relational Database ...
    Remove-RDSDBSecurityGroup                     DeleteDBSecurityGroup                      Amazon Relational Database ...
    Revoke-RDSDBSecurityGroupIngress              RevokeDBSecurityGroupIngress               Amazon Relational Database ...
    Approve-RSClusterSecurityGroupIngress         AuthorizeClusterSecurityGroupIngress       Amazon Redshift
    Get-RSClusterSecurityGroup                    DescribeClusterSecurityGroups              Amazon Redshift
    New-RSClusterSecurityGroup                    CreateClusterSecurityGroup                 Amazon Redshift
    Remove-RSClusterSecurityGroup                 DeleteClusterSecurityGroup                 Amazon Redshift
    Revoke-RSClusterSecurityGroupIngress          RevokeClusterSecurityGroupIngress          Amazon Redshift

If you know the name of the AWS service but not the AWS service API, add the
:code:`-MatchWithRegex` parameter and the :code:`-Service` parameter to scope the search to a
single service. For example, to get all cmdlet names that contain
:code:`SecurityGroup` in only the |EC2| service, run the following command.

.. code-block:: none

    PS C:\> Get-AWSCmdletName -ApiOperation SecurityGroup -MatchWithRegex -Service EC2
    
    CmdletName                                    ServiceOperation                           ServiceName                  CmdletNounPrefix
    ----------                                    ----------------                           -----------                  ----------------
    Get-EC2SecurityGroup                          DescribeSecurityGroups                     Amazon Elastic Compute Cloud EC2
    Get-EC2SecurityGroupReference                 DescribeSecurityGroupReferences            Amazon Elastic Compute Cloud EC2
    Get-EC2StaleSecurityGroup                     DescribeStaleSecurityGroups                Amazon Elastic Compute Cloud EC2
    Grant-EC2SecurityGroupEgress                  AuthorizeSecurityGroupEgress               Amazon Elastic Compute Cloud EC2
    Grant-EC2SecurityGroupIngress                 AuthorizeSecurityGroupIngress              Amazon Elastic Compute Cloud EC2
    New-EC2SecurityGroup                          CreateSecurityGroup                        Amazon Elastic Compute Cloud EC2
    Remove-EC2SecurityGroup                       DeleteSecurityGroup                        Amazon Elastic Compute Cloud EC2
    Revoke-EC2SecurityGroupEgress                 RevokeSecurityGroupEgress                  Amazon Elastic Compute Cloud EC2
    Revoke-EC2SecurityGroupIngress                RevokeSecurityGroupIngress                 Amazon Elastic Compute Cloud EC2
    Update-EC2SecurityGroupRuleEgressDescription  UpdateSecurityGroupRuleDescriptionsEgress  Amazon Elastic Compute Cloud EC2
    Update-EC2SecurityGroupRuleIngressDescription UpdateSecurityGroupRuleDescriptionsIngress Amazon Elastic Compute Cloud EC2

If you know the name of the |CLIlong| (|CLI|) command, add the :code:`-AwsCliCommand` parameter
and the desired |CLI| command call to get the name of the cmdlet that's based on the same API. For example, to get
the cmdlet name that corresponds to the :code:`authorize-security-group-ingress` |CLI| command call
in the |EC2| service, run the following command.

.. code-block:: none

    PS C:\> Get-AWSCmdletName -AwsCliCommand "aws ec2 authorize-security-group-ingress"
    
    CmdletName                           ServiceOperation                     ServiceName                         CmdletNounPrefix
    ----------                           ----------------                     -----------                         ----------------
    Grant-EC2SecurityGroupIngress        AuthorizeSecurityGroupIngress        Amazon Elastic Compute Cloud        EC2

The Get-AWSCmdletName cmdlet needs only enough of the |CLI| command name to identify
the service and the AWS API. For example, you could omit the :code:`aws` portion of :code:`aws ec2
authorize-security-group-ingress`.

To get a list of all of the cmdlets in the |TWPALL|, run the PowerShell
:code:`Get-Command` cmdlet, as shown in the following example.

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell

The :code:`Get-Command` cmdlet generates the list of |TWPALL| cmdlets in alphabetical order. The resulting list of
cmdlets is sorted by PowerShell verb, rather than PowerShell noun.

To sort results by service instead, run the following command.

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell | Sort-Object Noun,Verb

To filter the cmdlets that are returned by the :code:`Get-Command` cmdlet, run the
PowerShell :code:`Select-String` cmdlet. For example, to view the set of cmdlets that work with AWS regions, run the following command.

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell | Select-String region
    
    Clear-DefaultAWSRegion
    Copy-HSM2BackupToRegion
    Get-AWSRegion
    Get-DefaultAWSRegion
    Get-EC2Region
    Get-LSRegionList
    Get-RDSSourceRegion
    Set-DefaultAWSRegion

You can also find cmdlets for a specific service by filtering for the service prefix of cmdlet
nouns. To show service prefixes, run :code:`Get-AWSPowerShellVersion -ListServiceVersionInfo`. The following
example returns cmdlets that support the |CWlong| service.

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell -Noun CW*


.. _pstools-cmdlet-naming-aliases:

Cmdlet Naming and Aliases
-------------------------

The cmdlets in the |TWPALL| for each service are based on the methods
provided by the AWS SDK for the service. However, because of PowerShell's naming conventions, the
name of a cmdlet can be different from the name of the API call or method on which it is based. For example,
the :code:`Get-EC2Instance` cmdlet is based on the |EC2|
:code:`DescribeInstances` method.

In some cases, the cmdlet name may be similar to a method name, but it may actually perform a
different function. For example, the |S3| :code:`GetObject` method retrieves an |S3| object.
However, the :code:`Get-S3Object` cmdlet returns *information* about an |S3| object rather than the
object itself.

.. code-block:: none

    PS C:\> Get-S3Object -BucketName text-content -Key aws-tech-docs
    
    ETag         : "df000002a0fe0000f3c000004EXAMPLE"
    BucketName   : aws-tech-docs
    Key          : javascript/frameset.js
    LastModified : 6/13/2011 1:24:18 PM
    Owner        : Amazon.S3.Model.Owner
    Size         : 512
    StorageClass : STANDARD

To get an S3 object with the |TWPALL|, run the :code:`Read-S3Object` cmdlet.

.. code-block:: none

    PS C:\> Read-S3Object -BucketName text-content -Key text-object.txt -file c:\tmp\text-object-download.text
    
    Mode          LastWriteTime            Length Name
    ----          -------------            ------ ----
    -a---         11/5/2012   7:29 PM      20622  text-object-download.text

.. note:: The cmdlet help for an AWS cmdlet provides the name of the AWS SDK API on which the
    cmdlet is based. For more information about standard PowerShell verbs and their meanings, see `Approved Verbs for PowerShell Commands
    <https://docs.microsoft.com/en-us/powershell/developer/cmdlet/approved-verbs-for-windows-powershell-commands>`_.

All AWS cmdlets that use the :code:`Remove` verb--and the :code:`Stop-EC2Instance` cmdlet when you add the
:code:`-Terminate` parameter--prompt for confirmation before proceeding. To bypass confirmation,
add the :code:`-Force` parameter to your command.

AWS cmdlets do not support the :code:`-WhatIf` switch.

.. _pstools-aliases:

Aliases
~~~~~~~

Setup of the |TWPALL| installs an aliases file that contains aliases for many of the AWS 
cmdlets. You might find these aliases to be more intuitive than the cmdlet names. For example, service names and 
AWS SDK method names replace PowerShell verbs and nouns in some aliases. An example is the :code:`EC2-DescribeInstances` alias.

Other aliases use verbs that, though they do not follow standard PowerShell conventions, can be
more descriptive of the actual operation. For example, the alias file maps the alias
:code:`Get-S3Content` to the cmdlet :code:`Read-S3Object`.

.. code-block:: none

    PS C:\> Set-Alias -Name Get-S3Content -Value Read-S3Object

The aliases file is located in the |TWPALL| installation directory. To load the aliases into your
environment, *dot-source* the file. The following is a Windows-based example.

.. code-block:: none

    PS C:\>. "C:\Program Files (x86)\AWS Tools\PowerShell\AWSPowershell\AWSAliases.ps1"

To show all |TWPALL| aliases, run the following command. This command uses the :code:`?` alias for the PowerShell :code:`Where-Object` cmdlet and :code:`Source` property to filter for aliases that come only from the AWSPowerShell module.

.. code-block:: none

    PS C:\> Get-Alias | ? Source -like "AWSPowerShell"
    
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

To add your own aliases to this file, you might need to raise the value of PowerShell's :code:`$MaximumAliasCount` `preference variable <https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-6>`_ to a value greater than 5500. The default value is 4096; you can raise it to a maximum of 32768. To do this, run the following.

.. code-block:: none

    PS C:\> $MaximumAliasCount = 32768

To verify that your change was successful, enter the variable name to show its current value.

.. code-block:: none

    PS C:\> $MaximumAliasCount
    32768


