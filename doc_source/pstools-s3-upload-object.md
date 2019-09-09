# Upload Objects to an Amazon S3 Bucket<a name="pstools-s3-upload-object"></a>

Use the `Write-S3Object` cmdlet to upload files from your local file system to an Amazon S3 bucket as objects\. The example below creates and uploads two simple HTML files to an Amazon S3 bucket, and verifies the existence of the uploaded objects\. The `-File` parameter to `Write-S3Object` specifies the name of the file in the local file system\. The `-Key` parameter specifies the name that the corresponding object will have in Amazon S3\.

Amazon infers the content\-type of the objects from the file extensions, in this case, "\.html"\.

```
PS > # Create the two files using here-strings and the Set-Content cmdlet
PS > $index_html = @"
>> <html>
>>   <body>
>>     <p>
>>       Hello, World!
>>     </p>
>>   </body>
>> </html>
>> "@
>>
PS > $index_html | Set-Content index.html
PS > $error_html = @"
>> <html>
>>   <body>
>>     <p>
>>       This is an error page.
>>     </p>
>>   </body>
>> </html>
>> "@
>>
>>$error_html | Set-Content error.html
>># Upload the files to Amazon S3 using a foreach loop
>>foreach ($f in "index.html", "error.html") {
>> Write-S3Object -BucketName website-example -File $f -Key $f -CannedACLName public-read
>> }
>>
PS > # Verify that the files were uploaded
PS > Get-S3BucketWebsite -BucketName website-example

IndexDocumentSuffix                                         ErrorDocument
-------------------                                         -------------
index.html                                                  error.html
```

 *Canned ACL Options* 

The values for specifying canned ACLs with the Tools for Windows PowerShell are the same as those used by the AWS SDK for \.NET\. Note, however, that these are different from the values used by the Amazon S3`Put Object` action\. The Tools for Windows PowerShell support the following canned ACLs:
+ NoACL
+ private
+ public\-read
+ public\-read\-write
+ aws\-exec\-read
+ authenticated\-read
+ bucket\-owner\-read
+ bucket\-owner\-full\-control
+ log\-delivery\-write

For more information about these canned ACL settings, see [Access Control List Overview](https://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl)\.

## Note Regarding Multipart Upload<a name="note-regarding-multipart-upload"></a>

If you use the Amazon S3 API to upload a file that is larger than 5 GB in size, you need to use multipart upload\. However, the `Write-S3Object` cmdlet provided by the Tools for Windows PowerShell can transparently handle file uploads that are greater than 5 GB\.

### Test the Website<a name="pstools-amazon-s3-test-website"></a>

At this point, you can test the website by navigating to it using a browser\. URLs for static websites hosted in Amazon S3 follow a standard format\.

```
http://<bucket-name>.s3-website-<region>.amazonaws.com
```

For example:

```
http://website-example.s3-website-us-west-1.amazonaws.com
```

### See Also<a name="pstools-seealso-amazon-s3-test-website"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Put Object \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectPUT.html) 
+  [Canned ACLs \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/ACLOverview.html#CannedACL) 