# Migrating from AWS Tools for PowerShell Version 3\.3 to Version 4<a name="v4migration"></a>

AWS Tools for PowerShell version 4 is a backward\-compatible update to AWS Tools for PowerShell version 3\.3\. It adds significant improvements while maintaining existing cmdlet behavior\. 

Your existing scripts should continue to work after upgrading to the new version, but we do recommend that you test them thoroughly before upgrading your production environments\.

This section describes the changes and explains how they might impact your scripts\.

## New Fully Modularized AWS\.Tools Version<a name="migrate-aws-tools"></a>

The AWSPowerShell\.NetCore and AWSPowerShell packages were "monolithic"\. This meant that all of the AWS services were supported in the same module, making it very large, and growing larger as each new AWS service and feature was added\. The new AWS\.Tools package is broken up into smaller modules that give you the flexibility to download and install only those that you require for the AWS services that you use\. The package includes a shared `AWS.Tools.Common` module that is required by all of the other modules, and an `AWS.Tools.Installer` module that simplifies installing, updating, and removing modules as needed\.

This also enables auto\-importing of cmdlets on first call, without having to first call `Import-module`\. However, to interact with the associated \.NET objects before calling a cmdlet, you must still call `Import-Module` to let PowerShell know about the relevant \.NET types\. 

For example, the following command has a reference to `Amazon.EC2.Model.Filter`\. This type of reference can't trigger auto\-importing, so you must call `Import-Module` first or the command fails\.

```
PS > $filter = [Amazon.EC2.Model.Filter]@{Name="vpc-id";Values="vpc-1234abcd"}
  InvalidOperation: Unable to find type [Amazon.EC2.Model.Filter].
```

```
PS > Import-Module AWS.Tools.EC2
PS > $filter = [Amazon.EC2.Model.Filter]@{Name="vpc-id";Values="vpc-1234abcd"}
PS > Get-EC2Instance -Filter $filter -Select Reservations.Instances.InstanceId
  i-0123456789abcdefg
  i-0123456789hijklmn
```

## New `Get-AWSService` cmdlet<a name="migrate-get-awsservice"></a>

To help you discover the names of the modules for each AWS service in the AWS\.Tools collection of modules, you can use the `Get-AWSService` cmdlet\.

```
PS > Get-AWSService
  Service : ACMPCA
  CmdletNounPrefix : PCA
  ModuleName : AWS.Tools.ACMPCA
  SDKAssemblyVersion : 3.3.101.56
  ServiceName : AWS Certificate Manager Private Certificate Authority

  Service : AlexaForBusiness
  CmdletNounPrefix : ALXB
  ModuleName : AWS.Tools.AlexaForBusiness
  SDKAssemblyVersion : 3.3.106.26
  ServiceName : Alexa For Business
  ...
```

## New `-Select` Parameter to Control the Object Returned by a Cmdlet<a name="migrate-select"></a>

Most cmdlets in version 4 support a new `-Select` parameter\. Each cmdlet calls the AWS service APIs for you using the AWS SDK for \.NET\. Then the AWS Tools for PowerShell client converts the response into an object that you can use in your PowerShell scripts and pipe to other commands\. Sometimes the final PowerShell object has more fields or properties in the original response than you need, and other times you might want the object to include fields or properties of the response that are not there by default\. The `-Select` parameter enables you to specify what is included in the \.NET object returned by the cmdlet\.

For example, the [Get\-S3Object](https://docs.aws.amazon.com/powershell/latest/reference/items/Get-S3Object.html) cmdlet invokes the Amazon S3 SDK operation [ListObjects](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=S3/MS3ListObjectsListObjectsRequest.html)\. That operation returns a [ListObjectsResponse](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/index.html?page=S3/MS3ListObjectsListObjectsRequest.h) object\. However, by default, the `Get-S3Object` cmdlet returns only the `S3Objects` element of the SDK response to the PowerShell user\. In the following example, that object is an array with two elements\.

```
PS > Get-S3Object -BucketName mybucket

ETag : "01234567890123456789012345678901111"
BucketName : mybucket
Key : file1.txt
LastModified : 9/30/2019 1:31:40 PM
Owner : Amazon.S3.Model.Owner
Size : 568
StorageClass : STANDARD

ETag : "01234567890123456789012345678902222"
BucketName : mybucket
Key : file2.txt
LastModified : 7/15/2019 9:36:54 AM
Owner : Amazon.S3.Model.Owner
Size : 392
StorageClass : STANDARD
```

In AWS Tools for PowerShell version 4, you can specify `-Select *` to return the complete \.NET response object returned by the SDK API call\.

```
PS > Get-S3Object -BucketName mybucket -Select *
  IsTruncated    : False
  NextMarker     :
  S3Objects      : {file1.txt, file2.txt}
  Name           : mybucket
  Prefix         :
  MaxKeys        : 1000
  CommonPrefixes : {}
  Delimiter      :
```

You can also specify the path to the specific nested property you want\. The following example returns only the `Key` property of each element in the `S3Objects` array\.

```
PS > Get-S3Object -BucketName mybucket -Select S3Objects.Key
file1.txt
file2.txt
```

In certain situations it can be useful to return a cmdlet parameter\. You can do this with `-Select ^ParameterName`\. This feature supplants the `-PassThru` parameter, which is still available but deprecated\. 

```
PS > Get-S3Object -BucketName mybucket -Select S3Objects.Key |
>> Write-S3ObjectTagSet -Select ^Key -BucketName mybucket -Tagging_TagSet @{ Key='key'; Value='value'}
  file1.txt
  file2.txt
```

[The reference topic](https://docs.aws.amazon.com/powershell/latest/reference/) for each cmdlet identifies whether it supports the `-Select` parameter\.

## More Consistent Limiting of the Number of Items in the Output<a name="migrate-iterate"></a>

Earlier versions of AWS Tools for PowerShell enabled you to use the `-MaxItems` parameter to specify the maximum number of objects returned in the final output\.

This behavior is removed from AWS\.Tools\.

This behavior is deprecated in AWSPowerShell\.NetCore and AWSPowerShell, and will be removed from those versions in a future release\.

If the underlying service API supports a `MaxItems` parameter, it's still available and functions as the API specifies\. But it no longer has the added behavior of limiting the number of items returned in the output of the cmdlet\.

To limit the number of items returned in the final output, pipe the output to the `Select-Items` cmdlet and specify the `-First n` parameter, where *n* is the maximum number of items to include in the final output\.

```
PS > Get-S3Object -BucketName mybucket -Select S3Objects.Key | select -first 1*
file1.txt
```

Not all AWS services supported `-MaxItems` in the same way, so this removes that inconsistency and the unexpected results that sometimes occurred\. Also, `-MaxItems` combined with the new [`-Select`](#migrate-select) parameter could sometimes result in confusing results\.

## Easier to Use Stream Parameters<a name="migrate-streamparam"></a>

Parameters of type `Stream` or `byte[]` can now accept `string`, `string[]`, or `FileInfo` values\.

For example, you can use any of the following examples\.

```
PS > Invoke-LMFunction -FunctionName MyTestFunction -PayloadStream '{
>> "some": "json"
>> }'
```

```
PS > Invoke-LMFunction -FunctionName MyTestFunction -PayloadStream (ls .\some.json)
```

```
PS > Invoke-LMFunction -FunctionName MyTestFunction -PayloadStream @('{', '"some": "json"', '}')
```

 AWS Tools for PowerShell converts all strings to `byte[]` using UTF\-8 encoding\.

## Extending the Pipe by Property Name<a name="migrate-pipes"></a>

To make the user experience more consistent, you can now pass pipeline input by specifying the property name for *any* parameter\. 

In the following example, we create a custom object with properties that have names that match the parameter names of the target cmdlet\. When the cmdlet runs, it automatically consumes those properties as its parameters\.

```
PS > [pscustomobject] @{ BucketName='myBucket'; Key='file1.txt'; PartNumber=1 } | Get-S3ObjectMetadata
```

**Note**  
Some properties supported this in earlier versions of AWS Tools for PowerShell\. Version 4 makes this more consistent by enabling it for *all* parameters\.

## Static Common Parameters<a name="migrate-staticcommonparams"></a>

To improve consistency in version 4\.0 of AWS Tools for PowerShell, all parameters are static\.

In earlier versions of AWS Tools for PowerShell, some common parameters such as `AccessKey`,`SecretKey`, `ProfileName`, or `Region`, were [dynamic](https://docs.microsoft.com/dotnet/api/system.management.automation.idynamicparameters), while all other parameters were static\. This could create problems because PowerShell binds static parameters before dynamic ones\. For example, let's say you ran the following command\.

```
PS > Get-EC2Region -Region us-west-2
```

Earlier versions of PowerShell bound the value `us-west-2` to the `-RegionName` static parameter instead of the `-Region` dynamic parameter\. Likely, this could confuse users\.

## AWS\.Tools Declares and Enforces Manadatory Parameters<a name="migrate-mandatoryparams"></a>

The AWS\.Tools\.\* modules now declare and enforce mandatory cmdlet parameters\. When an AWS Service declares that a parameter of an API is required, PowerShell prompts you for the corresponding cmdlet parameter if you didn't specify it\. This applies only to AWS\.Tools\. To ensure backward compatibility, this does not apply to AWSPowerShell\.NetCore or AWSPowerShell\.

## All Parameters Are Nullable<a name="migrate-nullableparams"></a>

You can now assign `$null` to value type parameters \(numbers and dates\)\. This change should not affect existing scripts\. This enables you to bypass the prompt for a mandatory parameter\. Mandatory parameters are enforced in AWS\.Tools only\.

If you run the following example using version 4, it effectively bypasses client\-side validation because you provide a "value" for each mandatory parameter\. However, the Amazon EC2 API service call fails because the AWS service still requires that information\.

```
PS > Get-EC2InstanceAttribute -InstanceId $null -Attribute $null
WARNING: You are passing $null as a value for parameter Attribute which is marked as required.
In case you believe this parameter was incorrectly marked as required, report this by opening 
an issue at [https://github\.com/aws/aws\-tools\-for\-powershell/issues](https://github.com/aws/aws-tools-for-powershell/issues).
WARNING: You are passing $null as a value for parameter InstanceId which is marked as required.
In case you believe this parameter was incorrectly marked as required, report this by opening
an issue at [https://github\.com/aws/aws\-tools\-for\-powershell/issues](https://github.com/aws/aws-tools-for-powershell/issues).

Get-EC2InstanceAttribute : The request must contain the parameter instanceId
```

## Removing Previously Deprecated Features<a name="migrate-removeprevdeprecated"></a>

The following features were deprecated in previous releases of AWS Tools for PowerShell and are removed in version 4:
+ Removed the `-Terminate` parameter from the `Stop-EC2Instance` cmdlet\. Use `Remove-EC2Instance` instead\.
+ Removed the `-ProfileName` parameter from the Clear\-AWSCredential cmdlet\. Use `Remove-AWSCredentialProfile` instead\.
+ Removed cmdlets `Import-EC2Instance` and `Import-EC2Volume`\.