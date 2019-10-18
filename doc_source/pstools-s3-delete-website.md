# Delete Amazon S3 Objects and Buckets<a name="pstools-s3-delete-website"></a>

This section describes how to delete the website that you created in preceding sections\. You can simply delete the objects for the HTML files, and then delete the Amazon S3 bucket for the site\.

First, run the `Remove-S3Object` cmdlet to delete the objects for the HTML files from the Amazon S3 bucket\.

```
PS > foreach ( $obj in "index.html", "error.html" ) {
>> Remove-S3Object -BucketName website-example -Key $obj
>> }
>> 
IsDeleteMarker
--------------
False
```

The `False` response is an expected artifact of the way that Amazon S3 processes the request\. In this context, it does not indicate an issue\.

Now you can run the `Remove-S3Bucket` cmdlet to delete the now\-empty Amazon S3 bucket for the site\.

```
PS > Remove-S3Bucket -BucketName website-example

RequestId      : E480ED92A2EC703D
AmazonId2      : k6tqaqC1nMkoeYwbuJXUx1/UDa49BJd6dfLN0Ls1mWYNPHjbc8/Nyvm6AGbWcc2P
ResponseStream :
Headers        : {x-amz-id-2, x-amz-request-id, Date, Server}
Metadata       : {}
ResponseXml    :
```

In 1\.1 and newer versions of the AWS Tools for PowerShell, you can add the `-DeleteBucketContent` parameter to `Remove-S3Bucket`, which first deletes all objects and object versions in the specified bucket before trying to remove the bucket itself\. Depending on the number of objects or object versions in the bucket, this operation can take a substantial amount of time\. In versions of the Tools for Windows PowerShell older than 1\.1, the bucket had to be empty before `Remove-S3Bucket` could delete it\.

**Note**  
Unless you add the `-Force` parameter, AWS Tools for PowerShell prompts you for confirmation before the cmdlet runs\.

## See Also<a name="pstools-seealso-amazon-s3-delete-website"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [Delete Object \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectDELETE.html) 
+  [DeleteBucket \(Amazon S3 API Reference\)](https://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html) 