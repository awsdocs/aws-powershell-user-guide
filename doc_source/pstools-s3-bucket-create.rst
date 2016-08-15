.. _pstools-s3-bucket-create:

##################################################################
Create an |S3| Bucket, Verify Its Region, and Optionally Remove It
##################################################################

Use the :code:`New-S3Bucket` cmdlet to create a new |S3| bucket. The following examples creates a
bucket named website-example. The name of the bucket must be unique across all regions. The example
creates the bucket in the us-west-1 region.

.. code-block:: none

    PS C:\> New-S3Bucket -BucketName website-example -Region us-west-1
    
    BucketName                                                  CreationDate
    ----------                                                  ------------
    website-example                                       Mon, 26 Nov 2012 00:41:08 GMT

You can verify the region in which the bucket is located using the :code:`Get-S3BucketLocation`
cmdlet.

.. code-block:: none

    PS C:\> Get-S3BucketLocation -BucketName website-example
    us-west-1

You could use the following line to remove this bucket. We suggest that you leave this bucket in
place as we use it in subsequent examples.

.. code-block:: none

    Remove-S3Bucket -BucketName website-example

Note that the bucket removal process takes some time to finish. If you try to create a same-named
bucket immediately, the :code:`New-S3Bucket` cmdlet might fail for a period of time.

.. _pstools-seealso-s3-bucket-create:

See Also
--------

* :ref:`pstools-using`

* :s3-api:`Put Bucket (Amazon S3 Service Reference) <RESTBucketPUT>`

* :rande:`AWS PowerShell Regions for Amazon S3 <s3>`
