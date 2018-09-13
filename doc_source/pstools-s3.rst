.. _pstools-s3:

##############
|S3| and |TWP|
##############

.. toctree::
   :titlesonly:
   :caption: S3 Topics
   :maxdepth: 1
    
   pstools-s3-bucket-create
   pstools-s3-create-website
   pstools-s3-upload-object
   pstools-s3-delete-website
   pstools-s3-upload-in-line-text


In this section, we create a static website using the |TWPlong| using |S3| and |CF|. In the process, we
demonstrate a number of common tasks with these services. This walkthrough is modeled after the
Getting Started Guide for `Host a Static Website <https://aws.amazon.com/getting-started/projects/host-static-website/>`, which 
describes a similar process using the :console:`AWS Management Console <s3>`.

The commands shown here assume that you have set default credentials and a default region for your
PowerShell session. Therefore, credentials and regions are not included in the invocation of the
cmdlets.

There is currently no |S3| API for renaming buckets and objects, and therefore, no single |TWP| 
cmdlet for performing this task. To rename objects in S3, we recommend that you copy the object using a new name, 
by running the `Copy-S3Object <http://docs.aws.amazon.com/powershell/latest/reference/items/Copy-S3Object.html>`_ 
cmdlet, and then delete the original object by running the `Remove-S3Object <http://docs.aws.amazon.com/powershell/latest/reference/items/Remove-S3Object.html>`_ cmdlet.

See Also
========

* :ref:`pstools-using`

* `Hosting a Static Website on Amazon S3 <http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html>`_

* :console:`Amazon S3 Console <s3>`

