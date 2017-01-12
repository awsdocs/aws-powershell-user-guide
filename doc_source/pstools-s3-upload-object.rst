.. _pstools-s3-upload-object:

################################
Upload Objects to an |S3| Bucket
################################

Use the :code:`Write-S3Object` cmdlet to upload files from your local file system to an |S3| bucket
as objects. The example below creates and uploads two simple HTML files to an |S3| bucket, and
verifies the existence of the uploaded objects. The :code:`-File` parameter to
:code:`Write-S3Object` specifies the name of the file in the local file system. The :code:`-Key`
parameter specifies the name that the corresponding object will have in |S3|.

Amazon infers the content-type of the objects from the file extensions, in this case, ".html".

.. code-block:: none

    PS C:\> # Create the two files using here-strings and the Set-Content cmdlet
    PS C:\> $index_html = @"
    >> <html>
    >>   <body>
    >>     <p>
    >>       Hello, World!
    >>     </p>
    >>   </body>
    >> </html>
    >> "@
    >>
    PS C:\> $index_html | Set-Content index.html
    PS C:\> $error_html = @"
    >> <html>
    >>   <body>
    >>     <p>
    >>       This is an error page.
    >>     </p>
    >>   </body>
    >> </html>
    >> "@
    >>
    PS C:\> $error_html | Set-Content error.html
    PS C:\> # Upload the files to Amazon S3 using a foreach loop
    PS C:\> foreach ($f in "index.html", "error.html") {
    >> Write-S3Object -BucketName website-example -File $f -Key $f -CannedACLName public-read
    >> }
    >>
    PS C:\> # Verify that the files were uploaded
    PS C:\> Get-S3BucketWebsite -BucketName website-example
    
    IndexDocumentSuffix                                         ErrorDocument
    -------------------                                         -------------
    index.html                                                  error.html

*Canned ACL Options*

The values for specifying canned ACLs with the |TWP| are the same as those used by the AWS SDK for
.NET. Note, however, that these are different from the values used by the |S3| :code:`Put Object`
action. The |TWP| support the following canned ACLs:

* NoACL

* private

* public-read

* public-read-write

* aws-exec-read

* authenticated-read

* bucket-owner-read

* bucket-owner-full-control

* log-delivery-write

For more information about these canned ACL settings, see 
:s3-dg-deep:`Access Control List Overview <acl-overview.html#canned-acl>`.

Note Regarding Multipart Upload
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you use the |S3| API to upload a file that is larger than 5 GB in size, you need to use
multipart upload. However, the :code:`Write-S3Object` cmdlet provided by the |TWP| can transparently
handle file uploads that are greater than 5 GB.

.. _pstools-Amazon-S3-test-website:

Test the Website
----------------

At this point, you can test the website by navigating to it using a browser. URLs for static
websites hosted in |S3| follow a standard format.

.. code-block:: none

    http://<bucket-name>.s3-website-<region>.amazonaws.com

For example:

.. code-block:: none

    http://website-example.s3-website-us-west-1.amazonaws.com


.. _pstools-seealso-Amazon-S3-test-website:

See Also
--------

* :ref:`pstools-using`

* :s3-api:`Put Object (Amazon S3 API Reference) <RESTObjectPUT>`

* :s3-api-deep:`Canned ACLs (Amazon S3 API Reference) <ACLOverview.html#CannedACL>`
