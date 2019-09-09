# Configure an Amazon S3 Bucket as a Website and Enable Logging<a name="pstools-s3-create-website"></a>

Use the `Write-S3BucketWebsite` cmdlet to configure an Amazon S3 bucket as a static website\. The following example specifies a name of `index.html` for the default content web page and a name of `error.html` for the default error web page\. Note that this cmdlet does not create those pages\. They need to be [uploaded as Amazon S3 objects](pstools-s3-upload-object.md)\.

```
PS > Write-S3BucketWebsite -BucketName website-example -WebsiteConfiguration_IndexDocumentSuffix index.html -WebsiteConfiguration_ErrorDocument error.html
RequestId      : A1813E27995FFDDD
AmazonId2      : T7hlDOeLqA5Q2XfTe8j2q3SLoP3/5XwhUU3RyJBGHU/LnC+CIWLeGgP0MY24xAlI
ResponseStream :
Headers        : {x-amz-id-2, x-amz-request-id, Content-Length, Date...}
Metadata       : {}
ResponseXml    :
```

## See Also<a name="pstools-seealso-s3-create-website"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Put Bucket Website \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTwebsite.html) 
+  [Put Bucket ACL \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTacl.html) 