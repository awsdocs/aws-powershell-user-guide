.. _pstools-using:

###################
Using the |TWPlong|
###################

This section provides examples of using the |TWPlong| to access AWS services. These examples are
intended to demonstrate how to use the cmdlets to perform actual administrative tasks.

*Note Regarding Returned Objects for the Powershell Tools*

In some cases, the object returned from an |TWP| cmdlet does not mirror what is returned from the
corresponding API in the AWS SDK for .NET. For example, :code:`List-S3Buckets` emits a
:code:`Buckets` collection, not an |S3| response object. Similarly, :code:`Get-EC2Instance` emits a
:code:`Reservation` collection, not a :code:`DescribeEC2Instances` result object. This behavior is
by design and is intended to have the |TWP| experience be more consistent with idiomatic PowerShell.

The actual service responses are stored in :code:`note` properties on the returned objects and are
therefore available if you need to access them. For API actions that support :code:`NextToken`
fields, these are also attached as :code:`note` properties.

:ref:`Amazon EC2 <pstools-ec2>`

This section walks through the steps required to launch an |EC2| instance including how to:

 * Retrieve a list of Amazon Machine Images (AMIs).
 
 * Create a key pair.
 
 * Create and configure a security group.
 
 * Launch the instance and retrieve information about it.

:ref:`Amazon S3 <pstools-s3>`

The section walks through the steps required to create a static website hosted in |S3|. It 
demonstrates how to:

 * Create and delete |S3| buckets.
 
 * Upload files to an |S3| bucket as objects.
 
 * Delete objects from an |S3| bucket.
 
 * Designate an |S3| bucket as a website.

:ref:`AWS Identity and Access Management <pstools-iam>`

This section demonstrates basic operations in |IAMlong| (|IAM|) including how to:

 * Create an |IAM| group.
 
 * Create an |IAM| user.
 
 * Add an |IAM| user to an |IAM| group.
 
 * Specify a policy for an |IAM| user.
 
 * Set a password and credentials for an |IAM| user.

:ref:`Amazon SNS and Amazon SQS <pstools-sqs-queue-sns-topic>`

This section walks through the steps required to subscribe an |SQS| queue to an |SNS| topic. It 
demonstrates how to:

 * Create an |SNS| topic.
 
 * Create an |SQS| queue.
 
 * Subscribe the queue to the topic.
 
 * Send a message to the topic.
 
 * Receive the message from the queue.

:ref:`CloudWatch <pstools-cw>`

This section provides an example of how to publish custom data to |CW|. 

 * Publish a Custom Metric to Your |CW| Dashboard.

See Also
========

* :ref:`pstools-getting-started`


.. toctree::
     :titlesonly:
     :maxdepth: 1

     Amazon EC2 <pstools-ec2>
     Amazon S3 <pstools-s3>
     Amazon Identity and Access Management <pstools-iam>
     Amazon SNS and SQS <pstools-sqs-queue-sns-topic>
     CloudWatch <pstools-cw>



