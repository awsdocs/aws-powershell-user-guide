# Amazon S3 and Tools for Windows PowerShell<a name="pstools-s3"></a>

**Topics**
+ [See Also](#see-also)
+ [Create an Amazon S3 Bucket, Verify Its Region, and Optionally Remove It](pstools-s3-bucket-create.md)
+ [Configure an Amazon S3 Bucket as a Website and Enable Logging](pstools-s3-create-website.md)
+ [Upload Objects to an Amazon S3 Bucket](pstools-s3-upload-object.md)
+ [Delete Amazon S3 Objects and Buckets](pstools-s3-delete-website.md)
+ [Upload In\-Line Text Content to Amazon S3](pstools-s3-upload-in-line-text.md)

In this section, we create a static website using the AWS Tools for Windows PowerShell using Amazon S3 and CloudFront\. In the process, we demonstrate a number of common tasks with these services\. This walkthrough is modeled after the Getting Started Guide for [Host a Static Website](https://aws.amazon.com/getting-started/projects/host-static-website/), which describes a similar process using the [AWS Management Console](https://console.aws.amazon.com/s3/home)\.

The commands shown here assume that you have set default credentials and a default region for your PowerShell session\. Therefore, credentials and regions are not included in the invocation of the cmdlets\.

**Note**  
There is currently no Amazon S3 API for renaming a bucket or object, and therefore, no single Tools for Windows PowerShell cmdlet for performing this task\. To rename an object in S3, we recommend that you copy the object to one with a new name, by running the [Copy\-S3Object](https://docs.aws.amazon.com/powershell/latest/reference/items/Copy-S3Object.html) cmdlet, and then delete the original object by running the [Remove\-S3Object](https://docs.aws.amazon.com/powershell/latest/reference/items/Remove-S3Object.html) cmdlet\.

## See Also<a name="see-also"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) 
+  [Amazon S3 Console](https://console.aws.amazon.com/s3/home) 