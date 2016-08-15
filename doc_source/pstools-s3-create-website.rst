.. _pstools-s3-create-website:

########################################################
Configure an |S3| Bucket as a Website and Enable Logging
########################################################

Use the :code:`Write-S3BucketWebsite` cmdlet to configure an |S3| bucket as a static website. The
following example specifies a name of :file:`index.html` for the default content web page and a name
of :file:`error.html` for the default error web page. Note that this cmdlet does not create those
pages. They need to be :ref:`uploaded as Amazon S3 objects <pstools-s3-upload-object>`.

.. code-block:: none

    PS C:\> Write-S3BucketWebsite -BucketName website-example -WebsiteConfiguration_IndexDocumentSuffix index.html -WebsiteConfiguration_ErrorDocument error.html
    
    
    RequestId      : A1813E27995FFDDD
    AmazonId2      : T7hlDOeLqA5Q2XfTe8j2q3SLoP3/5XwhUU3RyJBGHU/LnC+CIWLeGgP0MY24xAlI
    ResponseStream :
    Headers        : {x-amz-id-2, x-amz-request-id, Content-Length, Date...}
    Metadata       : {}
    ResponseXml    :

.. _pstools-seealso-s3-create-website:

See Also
--------

* :ref:`pstools-using`

* :s3-api:`Put Bucket Website (Amazon S3 API Reference) <RESTBucketPUTwebsite>`

* :s3-api:`Put Bucket ACL (Amazon S3 API Reference) <RESTBucketPUTacl>`
