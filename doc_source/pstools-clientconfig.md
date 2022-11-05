# Using the ClientConfig parameter in cmdlets<a name="pstools-clientconfig"></a>

The `ClientConfig` parameter can be used to specify certain configuration settings when you connect to a service\. Most of the possible properties of this parameter are defined in the [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/Runtime/TClientConfig.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/Runtime/TClientConfig.html) class, which is inherited into the APIs for AWS services\. For an example of simple inheritance, see the [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/Keyspaces/TKeyspacesConfig.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/Keyspaces/TKeyspacesConfig.html) class\. In addition, some services define additional properties that are appropriate only for that service\. For an example of additional properties that have been defined, see the [https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/S3/TS3Config.html](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/S3/TS3Config.html) class, specifically the `ForcePathStyle` property\.

## Using the `ClientConfig` parameter<a name="clientconfig-operation"></a>

To use the `ClientConfig` parameter, you can specify it on the command line as a `ClientConfig` object or use PowerShell splatting to pass a collection of parameter values to a command as a unit\. These methods are shown in the following examples\. The examples assume that the `AWS.Tools.S3` module has been installed and imported, and that you have a `[default]` credentials profile with appropriate permissions\.

******Defining a `ClientConfig` object**

```
$s3Config = New-Object -TypeName Amazon.S3.AmazonS3Config
$s3Config.ForcePathStyle = $true
$s3Config.Timeout = [TimeSpan]::FromMilliseconds(150000)
Get-S3Object -BucketName <BUCKET_NAME> -ClientConfig $s3Config
```

**Adding `ClientConfig` properties by using PowerShell splatting**

```
$params=@{
    ClientConfig=@{
        ForcePathStyle=$true
        Timeout=[TimeSpan]::FromMilliseconds(150000)
    }
    BucketName="<BUCKET_NAME>"
}

Get-S3Object @params
```

## Using an undefined property<a name="clientconfig-undefined"></a>

When using PowerShell splatting, if you specify a `ClientConfig` property that doesn't exist, the AWS Tools for PowerShell doesn't detect the error until runtime, at which time it returns an exception\. Modifying the example from above:

```
$params=@{
    ClientConfig=@{
        ForcePathStyle=$true
        UndefinedProperty="Value"
        Timeout=[TimeSpan]::FromMilliseconds(150000)
    }
    BucketName="<BUCKET_NAME>"
}

Get-S3Object @params
```

This example produces an exception similar to the following:

```
Cannot bind parameter 'ClientConfig'. Cannot create object of type "Amazon.S3.AmazonS3Config". The UndefinedProperty property was not found for the Amazon.S3.AmazonS3Config object.
```

## Specifying the AWS Region<a name="clientconfig-region"></a>

You can use the `ClientConfig` parameter to set the AWS Region for the command\. The Region is set through the `RegionEndpoint` property\. The AWS Tools for PowerShell calculates the Region to use according to the following precedence:

1. The `-Region` parameter

1. The Region passed in the `ClientConfig` parameter

1. The PowerShell session state

1. The shared AWS `config` file

1. The environment variables

1. The Amazon EC2 instance metadata, if enabled\.