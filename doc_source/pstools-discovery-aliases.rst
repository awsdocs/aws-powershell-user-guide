.. _pstools-discovery-aliases:

############################
Cmdlet Discovery and Aliases
############################

This section discusses which services are supported by the |TWPlong|, the set of cmdlets provided by
the |TWP| in support of those services, and alternative names (aliases) for accessing those
services.

.. _pstools-cmdlet-discovery:

Cmdlet Discovery
----------------

To get the cmdlet name that corresponds to an AWS service API name, use the AWS
:code:`Get-AWSCmdletName` cmdlet along with the :code:`?ApiOperation` parameter and the AWS service
API name. For example, to get all of the possible cmdlet names that correspond to any available
:code:`DescribeInstances` AWS service API:

.. code-block:: none

    PS C:\> Get-AWSCmdletName -ApiOperation DescribeInstances
    
    CmdletName              ServiceOperation         ServiceName                         CmdletNounPrefix
    ----------              ----------------         -----------                         ----------------
    Get-EC2Instance         DescribeInstances        Amazon Elastic Compute Cloud        EC2
    Get-OPSInstances        DescribeInstances        AWS OpsWorks                        OPS

Note that you can omit the :code:`?ApiOperation` parameter name in the preceding call, so the
following is equivalent:

.. code-block:: none

    PS C:\> Get-AWSCmdletName DescribeInstances

If you know the names of both the desired AWS service API and the AWS service, add the
:code:`?Service` parameter along with either the cmdlet noun prefix or some portion of the AWS
service name. (For instance, the cmdlet noun prefix for |EC2| is :code:`EC2`.) For example, to get
the cmdlet name that corresponds to the :code:`DescribeInstances` API in the |EC2| service, call one
of the following:

.. code-block:: none

    PS C:\> Get-AWSCmdletName ?ApiOperation DescribeInstances -Service EC2
    PS C:\> Get-AWSCmdletName ?ApiOperation DescribeInstances -Service Compute
    PS C:\> Get-AWSCmdletName ?ApiOperation DescribeInstances -Service "Compute Cloud"
    
    CmdletName              ServiceOperation         ServiceName                         CmdletNounPrefix
    ----------              ----------------         -----------                         ----------------
    Get-EC2Instance         DescribeInstances        Amazon Elastic Compute Cloud        EC2

Note that the parameter values in these calls are case-insensitive.

If you do not know the name of either the desired AWS service API or the AWS service, use the
:code:`?ApiOperation` parameter along with the pattern to match and the :code:`-MatchWithRegex`
parameter. For example, to get all of the available cmdlet names that contain :code:`SecurityGroup`:

.. code-block:: none

    PS C:\> Get-AWSCmdletName ?ApiOperation SecurityGroup -MatchWithRegex
    
    CmdletName                              ServiceOperation                        ServiceName                            CmdletNounPrefix
    ----------                              ----------------                        -----------                            ----------------
    Approve-ECCacheSecurityGroupIngress     AuthorizeCacheSecurityGroupIngress      Amazon ElastiCache                     EC
    Get-ECCacheSecurityGroup                DescribeCacheSecurityGroups             Amazon ElastiCache                     EC
    New-ECCacheSecurityGroup                CreateCacheSecurityGroup                Amazon ElastiCache                     EC
    Remove-ECCacheSecurityGroup             DeleteCacheSecurityGroup                Amazon ElastiCache                     EC
    Revoke-ECCacheSecurityGroupIngress      RevokeCacheSecurityGroupIngress         Amazon ElastiCache                     EC
    Get-EC2SecurityGroup                    DescribeSecurityGroups                  Amazon Elastic Compute Cloud           EC2
    Grant-EC2SecurityGroupEgress            AuthorizeSecurityGroupEgress            Amazon Elastic Compute Cloud           EC2
    Grant-EC2SecurityGroupIngress           AuthorizeSecurityGroupIngress           Amazon Elastic Compute Cloud           EC2
    New-EC2SecurityGroup                    CreateSecurityGroup                     Amazon Elastic Compute Cloud           EC2
    Remove-EC2SecurityGroup                 DeleteSecurityGroup                     Amazon Elastic Compute Cloud           EC2
    Revoke-EC2SecurityGroupEgress           RevokeSecurityGroupEgress               Amazon Elastic Compute Cloud           EC2
    Revoke-EC2SecurityGroupIngress          RevokeSecurityGroupIngress              Amazon Elastic Compute Cloud           EC2
    Join-ELBSecurityGroupToLoadBalancer     ApplySecurityGroupsToLoadBalancer       Elastic Load Balancing                 ELB
    Enable-RDSDBSecurityGroupIngress        AuthorizeDBSecurityGroupIngress         Amazon Relational Database Service     RDS
    Get-RDSDBSecurityGroup                  DescribeDBSecurityGroups                Amazon Relational Database Service     RDS
    New-RDSDBSecurityGroup                  CreateDBSecurityGroup                   Amazon Relational Database Service     RDS
    Remove-RDSDBSecurityGroup               DeleteDBSecurityGroup                   Amazon Relational Database Service     RDS
    Revoke-RDSDBSecurityGroupIngress        RevokeDBSecurityGroupIngress            Amazon Relational Database Service     RDS
    Approve-RSClusterSecurityGroupIngress   AuthorizeClusterSecurityGroupIngress    Amazon Redshift                        RS
    Get-RSClusterSecurityGroups             DescribeClusterSecurityGroups           Amazon Redshift                        RS
    New-RSClusterSecurityGroup              CreateClusterSecurityGroup              Amazon Redshift                        RS
    Remove-RSClusterSecurityGroup           DeleteClusterSecurityGroup              Amazon Redshift                        RS
    Revoke-RSClusterSecurityGroupIngress    RevokeClusterSecurityGroupIngress       Amazon Redshift                        RS

If you know the name of the desired AWS service but not the AWS service API, use the
:code:`-MatchWithRegex` parameter along with the :code:`?Service` parameter to scope the search to a
single service. For example, to get all of the possible cmdlet names that contain
:code:`SecurityGroup` in just the |EC2| service:

.. code-block:: none

    PS C:\> Get-AWSCmdletName ?ApiOperation SecurityGroup ?MatchWithRegex ?Service EC2
    
    CmdletName                              ServiceOperation                        ServiceName                            CmdletNounPrefix
    ----------                              ----------------                        -----------                            ----------------
    Get-EC2SecurityGroup                    DescribeSecurityGroups                  Amazon Elastic Compute Cloud           EC2
    Grant-EC2SecurityGroupEgress            AuthorizeSecurityGroupEgress            Amazon Elastic Compute Cloud           EC2
    Grant-EC2SecurityGroupIngress           AuthorizeSecurityGroupIngress           Amazon Elastic Compute Cloud           EC2
    New-EC2SecurityGroup                    CreateSecurityGroup                     Amazon Elastic Compute Cloud           EC2
    Remove-EC2SecurityGroup                 DeleteSecurityGroup                     Amazon Elastic Compute Cloud           EC2
    Revoke-EC2SecurityGroupEgress           RevokeSecurityGroupEgress               Amazon Elastic Compute Cloud           EC2
    Revoke-EC2SecurityGroupIngress          RevokeSecurityGroupIngress              Amazon Elastic Compute Cloud           EC2

If you know the name of the |CLIlong| (|CLI|) command, use the :code:`?AwsCliCommand` parameter
along with the desired |CLI| command call to get the corresponding cmdlet name. For example, to get
the cmdlet name that corresponds to the :code:`authorize-security-group-ingress` |CLI| command call
in the |EC2| service:

.. code-block:: none

    PS C:\> Get-AWSCmdletName -AwsCliCommand "aws ec2 authorize-security-group-ingress"
    
    CmdletName                           ServiceOperation                     ServiceName                         CmdletNounPrefix
    ----------                           ----------------                     -----------                         ----------------
    Grant-EC2SecurityGroupIngress        AuthorizeSecurityGroupIngress        Amazon Elastic Compute Cloud        EC2

Note that the Get-AWSCmdletName cmdlet needs only enough of the |CLI| command to be able to identify
the service and the AWS API. For example, you could omit the :code:`aws` portion of :code:`aws ec2
authorize-security-group-ingress`.

To get a list of all of the cmdlets that are provided by the |TWP|, use the PowerShell
:code:`Get-Command` cmdlet, for example:

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell

The :code:`Get-Command` cmdlet generates this list in alphabetical order. Therefore, the list of
cmdlets is sorted by PowerShell verb rather than PowerShell noun.

The following script generates a list of the cmdlets sorted by the PowerShell nouns that correspond
to the supported AWS services.

.. code-block:: none

    $services =
    "AS",   # Auto Scaling
    "ASA",  # AWS Support API
    "CC",   # AWS CodeCommit
    "CD",   # AWS CodeDeploy
    "CF",   # Amazon CloudFront
    "CFG",  # Amazon Config
    "CFN",  # AWS CloudFormation
    "CGI",  # Amazon Cognito Identity
    "CP",   # Amazon CodePipeline
    "CS",   # Amazon CloudSearch
    "CSD",  # Amazon CloudSearchDomain 
    "CT",   # AWS CloudTrail
    "CW",   # Amazon CloudWatch
    "CWL",  # Amazon CloudWatch Logs
    "DC",   # AWS Direct Connect
    "DDB",  # Amazon DynamoDB
    "DF",   # AWS Device Farm
    "DP",   # AWS Data Pipeline
    "DS",   # AWS Directory Service 
    "EB",   # AWS Elastic Beanstalk
    "EC",   # Amazon ElastiCache
    "EC2",  # Amazon Elastic Compute Cloud
    "ECS",  # Amazon EC2 Container Service
    "EFS",  # Amazon Elastic File System
    "ELB",  # Amazon Elastic Load Balancing 
    "EMR",  # Amazon Elastic MapReduce
    "ETS",  # Amazon Elastic Transcoder 
    "HSM",  # AWS Cloud HSM
    "IAM",  # AWS Identity and Access Management 
    "IE",   # AWS Import/Export
    "KIN",  # AWS Kinesis 
    "KMS",  # AWS Key Management Service
    "LM",   # Amazon Lambda
    "ML",   # Amazon Machine Learning
    "OPS",  # AWS OpsWorks
    "R53",  # AWS Route 53
    "R53D", # AWS Route 53 Domains
    "RDS",  # Amazon Relational Database Service
    "RS",   # Amazon Redshift
    "S3",   # Amazon Simple Storage Service
    "SES",  # Amazon Simple Email Service 
    "SG",   # AWS Storage Gateway
    "SNS",  # Amazon Simple Notification Service
    "SQS",  # Amazon Simple Queue Service
    "SSM",  # Amazon Simple Systems Management
    "STS",  # AWS Security Token Service
    "WKS",  # Amazon WorkSpaces
    
    foreach ($s in $services)
    {
      "-----"; Get-Command -Noun ${s}*
    }

To filter the list of cmdlets that are returned by the :code:`Get-Command` cmdlet, run the
PowerShell :code:`Select-String` cmdlet. For example, to view the set of AWS cmdlets that work with
regions:

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell | Select-String region
    
    Clear-DefaultAWSRegion
    Get-AWSRegion
    Get-DefaultAWSRegion
    Get-EC2Region
    Set-DefaultAWSRegion

You can also find cmdlets for a specific service by filtering for the service prefix of cmdlet
nouns. Service prefixes are shown in quotation marks in the previous script example. The following
example returns cmdlets that support the |CWlong| service.

.. code-block:: none

    PS C:\> Get-Command -Module AWSPowerShell -Noun CW*


.. _pstools-cmdlet-naming-aliases:

Cmdlet Naming and Aliases
-------------------------

The cmdlets provided by the |TWP| for a given service correspond approximately to the methods
provided by the AWS SDK for that service. However, because of PowerShell's naming conventions, the
name of a cmdlet may be somewhat different than the name of the corresponding method. For example,
the :code:`Get-EC2Instances` cmdlet performs a similar function to the |EC2|
:code:`DescribeInstances` method.

In other cases, the cmdlet name may be similar to a method name, but it may actually perform a
different function. For example, the |S3| :code:`GetObject` method retrieves an |S3| object.
However, the :code:`Get-S3Object` cmdlet returns *information* about an |S3| object rather than the
object itself.

.. code-block:: none

    PS C:\> Get-S3Object -BucketName text-content -Key text-object
    
    Key          : text-object.txt
    BucketName   : text-content
    LastModified : Mon, 27 Aug 2012 19:39:34 GMT
    ETag         : "f738612c5e842b39819c6d8fc4eb5b9b"
    Size         : 20622
    Owner        : Amazon.S3.Model.Owner
    StorageClass : STANDARD

To retrieve the object with the |TWP|, use the :code:`Read-S3Object` cmdlet.

.. code-block:: none

    PS C:\> Read-S3Object -BucketName text-content -Key text-object.txt -file c:\tmp\text-object-download.text
    
    Mode          LastWriteTime            Length Name
    ----          -------------            ------ ----
    -a---         11/5/2012   7:29 PM      20622  text-object-download.text

.. note:: The cmdlet help for an AWS cmdlet provides the name of the AWS SDK API that corresponds to the
    cmdlet. For more information about the standard PowerShell verbs and their expected meanings, go
    to the `Windows DevCenter
    <http://msdn.microsoft.com/en-us/library/windows/desktop/ms714428.aspx>`_.

All AWS cmdlets that use the Remove verb, and the :code:`Stop-EC2Instance` cmdlet when used with the
:code:`-Terminate` switch, now prompt for confirmation before proceeding. To bypass confirmation,
use the :code:`-Force` switch.

The AWS cmdlets do not support the :code:`-WhatIf` switch.

.. _pstools-aliases:

Aliases
~~~~~~~

The setup program for the |TWP| installs an aliases file that contains aliases for many of the |TWP|
cmdlets. You may find these aliases to be more intuitive than the cmdlet names. For example, aliases
are provided that are prefixed with the service name |mdash| rather than a PowerShell verb |mdash|
and followed by an AWS SDK method name. An example is the :code:`EC2-DescribeInstances` alias.

Other aliases use verbs that, although they do not follow standard PowerShell conventions, may be
more descriptive of the actual operation. For example, the alias file maps the alias
:code:`Get-S3Content` to the cmdlet :code:`Read-S3Object`.

.. code-block:: none

    PS C:\> Set-Alias -Name Get-S3Content -Value Read-S3Object

The aliases file is located in the |TWPlong| installation directory. To load the aliases into your
environment, "dot-source" the file.

.. code-block:: none

    PS C:\>. c:\Program Files (x86)\AWS Tools\PowerShell\AWSPowershell\AWSAliases.ps1

