.. _pstools-Amazon-S3-delete-website:

###############################
Delete |S3| Objects and Buckets
###############################

This section describes how to delete the website that you created in preceding sections. You can
simply delete the objects for the HTML files, and then delete the |S3| bucket for the site.

Run the :code:`Remove-S3Object` cmdlet to delete the objects for the HTML files from the |S3|
bucket.

.. code-block:: none

    PS C:\> foreach ( $obj in "index.html", "error.html" ) {
    >> Remove-S3Object -BucketName website-example -Key $obj
    >> }
    >>
    IsDeleteMarker
    --------------
    False

The :code:`False` response is an expected artifact of the way that |S3| processes the request. In
this context, it does not indicate an issue.

Run the :code:`Remove-S3Bucket` cmdlet to delete the now-empty |S3| bucket for the site.

.. code-block:: none

    PS C:\> Remove-S3Bucket -BucketName website-example
    
    RequestId      : E480ED92A2EC703D
    AmazonId2      : k6tqaqC1nMkoeYwbuJXUx1/UDa49BJd6dfLN0Ls1mWYNPHjbc8/Nyvm6AGbWcc2P
    ResponseStream :
    Headers        : {x-amz-id-2, x-amz-request-id, Date, Server}
    Metadata       : {}
    ResponseXml    :

In 1.1 and newer versions of the |TWP|, you can add the :code:`-DeleteBucketContent` parameter to
:code:`Remove-S3Bucket`, which first deletes all objects and object versions in the specified bucket
before trying to remove the bucket itself. Depending on the number of objects or object versions in
the bucket, this operation can take a substantial amount of time. In versions of the |TWP| older
than 1.1, the bucket had to be empty for :code:`Remove-S3Bucket` to delete it.

Note that unless you add the -Force parameter, you are prompted for confirmation before the cmdlet
runs.

.. _pstools-seealso-Amazon-S3-delete-website:

See Also
--------

* :ref:`pstools-using`

* `Delete Object (Amazon S3 API Reference) <http://docs.aws.amazon.com/AmazonS3/latest/API/RESTObjectDELETE.html>`_

* `DeleteBucket (Amazon S3 API Reference) <http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html>`_
