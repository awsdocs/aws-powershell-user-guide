# Using the AWS Tools for Windows PowerShell<a name="pstools-using"></a>

**Topics**
+ [PowerShell File Concatenation Encoding](#powershell-file-concatenation-encoding)
+ [Returned Objects for the Powershell Tools](#returned-objects-for-the-powershell-tools)
+ [See Also](#see-also)
+ [Amazon S3 and Tools for Windows PowerShell](pstools-s3.md)
+ [IAM and Tools for Windows PowerShell](pstools-iam.md)
+ [Amazon EC2 and Tools for Windows PowerShell](pstools-ec2.md)
+ [AWS Lambda and Tools for PowerShell](pstools-lambda.md)
+ [Amazon SQS, Amazon SNS and Tools for Windows PowerShell](pstools-sqs-queue-sns-topic.md)
+ [CloudWatch from the AWS Tools for Windows PowerShell](pstools-cw.md)

This section provides examples of using the AWS Tools for PowerShell to access AWS services\. These examples help demonstrate how to use the cmdlets to perform actual AWS tasks\.

## PowerShell File Concatenation Encoding<a name="powershell-file-concatenation-encoding"></a>

Some cmdlets in the Tools for PowerShell edit existing files or records that you have in AWS\. An example is `Edit-R53ResourceRecordSet`, built on the [ChangeResourceRecordSets](https://docs.aws.amazon.com/Route53/latest/APIReference/API_ChangeResourceRecordSets.html) API for Amazon Route 53\.

When you edit or concatenate files in PowerShell 5\.1 or older releases, PowerShell encodes the output in UTF\-16, not UTF\-8\. This can add unwanted characters and create results that are not valid\. A hexadecimal editor can reveal the unwanted characters\.

To avoid converting file output to UTF\-16, you can pipe your command into PowerShell's `Out-File` cmdlet, as shown in the following example\.

```
*file concatenation command* | Out-File filename.txt -Encoding utf8
```

If you are running AWS CLI commands in the PowerShell console, the same behavior applies\. You can also pipe an AWS CLI command into `Out-File` in the PowerShell console\. Other cmdlets, such as `Export-Csv` or `Export-Clixml`, also have an `Encoding` parameter\. For a complete list of cmdlets that have an `Encoding` parameter, and that allow you to correct the encoding of the output of a concatenated file, run the following command\.

```
PS> Get-Command -ParameterName "Encoding"
```

PowerShell 6\.0 retains UTF\-8 encoding for concatenated file output\. Because AWSPowerShell\.NetCore requires PowerShell 6\.0, you won't experience the encoding change behavior in the Tools for PowerShell Core\.

## Returned Objects for the Powershell Tools<a name="returned-objects-for-the-powershell-tools"></a>

In some cases, the object returned by an Tools for PowerShell cmdlet does not mirror what is returned from the corresponding API in the AWS SDK for \.NET\. For example, `Get-S3Bucket` emits a `Buckets` collection, not an Amazon S3 response object\. Similarly, `Get-EC2Instance` emits a `Reservation` collection, not a `DescribeEC2Instances` result object\. This behavior is by design and is intended to have the Tools for PowerShell experience be more consistent with idiomatic PowerShell\.

The actual service responses are stored in `note` properties on the returned objects and are therefore available if you need to access them\. For API actions that support `NextToken` fields, these are also attached as `note` properties\.

 [Amazon EC2](pstools-ec2.md) 

This section walks through the steps required to launch an Amazon EC2 instance including how to:
+ Retrieve a list of Amazon Machine Images \(AMIs\)\.
+ Create a key pair\.
+ Create and configure a security group\.
+ Launch the instance and retrieve information about it\.

 [Amazon S3](pstools-s3.md) 

The section walks through the steps required to create a static website hosted in Amazon S3\. It demonstrates how to:
+ Create and delete Amazon S3 buckets\.
+ Upload files to an Amazon S3 bucket as objects\.
+ Delete objects from an Amazon S3 bucket\.
+ Designate an Amazon S3 bucket as a website\.

 [AWS Identity and Access Management](pstools-iam.md) 

This section demonstrates basic operations in AWS Identity and Access Management \(IAM\) including how to:
+ Create an IAM group\.
+ Create an IAM user\.
+ Add an IAM user to an IAM group\.
+ Specify a policy for an IAM user\.
+ Set a password and credentials for an IAM user\.

 [AWS Lambda and Lambda Tools for PowerShell](pstools-lambda.md) 

This section provides a brief overview of the AWS Lambda Tools for PowerShell module and describes the required steps for setting up the module\.

 [Amazon SNS and Amazon SQS](pstools-sqs-queue-sns-topic.md) 

This section walks through the steps required to subscribe an Amazon SQS queue to an Amazon SNS topic\. It demonstrates how to:
+ Create an Amazon SNS topic\.
+ Create an Amazon SQS queue\.
+ Subscribe the queue to the topic\.
+ Send a message to the topic\.
+ Receive the message from the queue\.

 [CloudWatch](pstools-cw.md) 

This section provides an example of how to publish custom data to CloudWatch\.
+ Publish a Custom Metric to Your CloudWatch Dashboard\.

## See Also<a name="see-also"></a>
+  [Getting Started with the AWS Tools for Windows PowerShell](pstools-getting-started.md) 