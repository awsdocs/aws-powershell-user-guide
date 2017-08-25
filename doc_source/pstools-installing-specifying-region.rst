.. _pstools-installing-specifying-region:

######################
Specifying AWS Regions
######################

There are two ways to specify the AWS region to use when running |CLI| commands, the :code:`-Region`
common parameter, or the :code:`Set-DefaultAWSRegion` command.

Most AWS cmdlets fail if you do not specify a region. The exceptions are cmdlets for :ref:`Amazon S3
<pstools-s3>`, |SES|, and :ref:`AWS Identity and Access Management (IAM ) <pstools-iam>`.

In the absence of a specified region, |S3| and |SES| use :rande:`US East (N. Virginia) <ses>`.

:console:`Amazon SES <ses>` and :console:`IAM <iam>` are services that do not require a region to be 
specified.

**To specify the region for a single AWS command**

Add the :code:`-Region` parameter to your command, such as:

.. code-block:: none

   PS C:\> Get-EC2Image -Region us-west-1

**To set a default region for all AWS CLI commands in the session**

From the PowerShell command prompt, type the following command:

.. code-block:: none

   PS C:\> Set-DefaultAWSRegion -Region us-west-1

.. note:: This setting persists only for the current session. To apply the setting to all of your PowerShell
   sessions, add this command to your PowerShell profile as you did for the :code:`Import-Module`
   command.

**To view the current default region for all AWS CLI commands**

From the PowerShell command prompt, type the following command:

.. code-block:: none

   PS C:\> Get-DefaultAWSRegion
   
   Region                Name                                IsShellDefault
   ----------            ----                                --------------
   us-west-1             US West (N. California)             True

**To clear the current default region for all AWS CLI commands**

From the PowerShell command prompt, type the following command:

.. code-block:: none

   PS C:\> Clear-DefaultAWSRegion

**To view a list of all available AWS regions**

From the PowerShell command prompt, type the following command. Note that the third column 
identifies which region is the default for your current session.

.. code-block:: none

   PS C:\> Get-AWSRegion
   
   Region         Name                      IsShellDefault
   ------         ----                      --------------
    ap-northeast-1 Asia Pacific (Tokyo)      False
    ap-northeast-2 Asia Pacific (Seoul)      False
    ap-south-1     Asia Pacific (Mumbai)     False
    ap-southeast-1 Asia Pacific (Singapore)  False
    ap-southeast-2 Asia Pacific (Sydney)     False
    ca-central-1   Canada (Central)          False
    eu-central-1   EU Central (Frankfurt)    False
    eu-west-1      EU West (Ireland)         False
    eu-west-2      EU West (London)          False
    sa-east-1      South America (Sao Paulo) False
    us-east-1      US East (Virginia)        False
    us-east-2      US East (Ohio)            False
    us-west-1      US West (N. California)   False
    us-west-2      US West (Oregon)          True