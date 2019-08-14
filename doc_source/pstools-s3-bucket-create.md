# Create an Amazon S3 Bucket, Verify Its Region, and Optionally Remove It<a name="pstools-s3-bucket-create"></a>

Use the `New-S3Bucket` cmdlet to create a new Amazon S3 bucket\. The following examples creates a bucket named website\-example\. The name of the bucket must be unique across all regions\. The example creates the bucket in the us\-west\-1 region\.

```
PS > New-S3Bucket -BucketName website-example -Region us-west-1

BucketName                                                  CreationDate
----------                                                  ------------
website-example                                       Mon, 26 Nov 2012 00:41:08 GMT
```

You can verify the region in which the bucket is located using the `Get-S3BucketLocation` cmdlet\.

```
PS > Get-S3BucketLocation -BucketName website-example
us-west-1
```

You could use the following line to remove this bucket\. We suggest that you leave this bucket in place as we use it in subsequent examples\.

```
Remove-S3Bucket -BucketName website-example
```

Note that the bucket removal process takes some time to finish\. If you try to create a same\-named bucket immediately, the `New-S3Bucket` cmdlet might fail for a period of time\.

## See Also<a name="pstools-seealso-s3-bucket-create"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Put Bucket \(Amazon S3 Service Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUT.html) 
+  [AWS PowerShell Regions for Amazon S3](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) 