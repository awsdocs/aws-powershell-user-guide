.. _pstools-pipelines:

##########################
Pipelining and $AWSHistory
##########################

For service calls that return collections, the objects within the collection are now always
enumerated to the pipeline. Result objects that contain additional fields beyond the collection and
which are not paging control fields have these fields added as Note properties for the calls. These
Note properties are logged in the new :code:`$AWSHistory` session variable, should you need to
access this data. The :code:`$AWSHistory` variable is described in the next section.

.. note:: In versions of the |TWP| prior to v1.1, the collection object itself was emitted, which required the
   use of foreach {$_.getenumerator()} to continue pipelining.

Examples

Return a collection of Amazon EC2 machine images (AMIs) across all regions.

.. code-block:: none

    PS C:\> Get-AWSRegion | % { Get-EC2Image -Owner self -Region $_ }

Stop all Amazon EC2 instances in the current default region.

.. code-block:: none

    PS C:\> Get-EC2Instance | Stop-EC2Instance

Because collections enumerate to the pipeline, the output from a given cmdlet might be $null, a
single object, or a collection. If it is a collection, you can use the :code:`.Count` property to
determine the size of the collection. However, the :code:`.Count` property is not present when only
a single object is emitted. If your script needs to determine, in a consistent way, how many objects
were emitted, use the new :code:`EmittedObjectsCount` property of the last command value in
:code:`$AWSHistory`.

.. _pstools-awshistory:

$AWSHistory
-----------

To better support pipelining, output from AWS cmdlets is no longer reshaped to include the service
(AWS SDK) response and result instances as Note properties on the emitted collection object.
Instead, for those calls that emit a single collection as output, the collection is now enumerated
to the PowerShell pipeline. This means that the AWS SDK response and result data cannot exist in the
pipe, because there is no containing collection object to which it can be attached.

Although most users probably won't need this data, it can be useful for diagnostic purposes, because
you can see exactly what was sent to and received from the underlying AWS service calls made by the
cmdlet.

Starting with version 1.1, this data and more is now available in a new shell variable named
:code:`$AWSHistory`. This variable maintains a record of AWS cmdlet invocations and the service
responses that were received for each invocation. Optionally, this history can be configured to also
record the service requests that each cmdlet made. Additional useful data, such as the overall
execution time of the cmdlet, can also be obtained from each entry.

Each entry in the :code:`$AWSHistory.Commands` list is of type :code:`AWSCmdletHistory`. This type
has the following useful members:

*CmdletName* 
     Name of the cmdlet.

*CmdletStart* 
     DateTime that the cmdlet was run.

*CmdletEnd* 
     DateTime that the cmdlet finished all processing.

*Requests* 
     If request recording is enabled, list of last service requests.

*Responses* 
     List of last service responses received.

*LastServiceResponse* 
     Helper to return the most recent service response.

*LastServiceRequest* 
     Helper to return the most recent service response, if available.

Note that the :code:`$AWSHistory` variable is not created until an AWS cmdlet making a service call
is used. It evaluates to $null until that point.

.. note:: Earlier versions of the |TWP| emitted data related to service responses as :code:`Note` properties
   on the returned object. These are now found on the response entries that are recorded for each
   invocation in the list.

.. _pstools-setawshistoryconfiguration:

Set-AWSHistoryConfiguration
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A cmdlet invocation can hold zero or more service request and response entries. To limit memory
impact, the :code:`$AWSHistory` list keeps a record of only the last five cmdlet executions by
default; and for each, the last five service responses (and if enabled, last five service requests).
You can change these default limits by running the :code:`Set-AWSHistoryConfiguration` cmdlet. It
allows you to both control the size of the list, and whether service requests are also logged:

.. code-block:: none

    PS C:\> Set-AWSHistoryConfiguration -MaxCmdletHistory <value> -MaxServiceCallHistory <value> -RecordServiceRequests

The :code:`-MaxCmdletHistory` parameter sets the maximum number of cmdlets that can be tracked at
any time. A value of 0 turns off recording of AWS cmdlet activity. The
:code:`-MaxServiceCallHistory` parameter sets the maximum number of service responses (and/or
requests) that are tracked for each cmdlet. The :code:`-RecordServiceRequests` parameter, if
specified, turns on tracking of service requests for each cmdlet. All parameters are optional.

If run with no parameters, :code:`Set-AWSHistoryConfiguration` simply turns off any prior request
recording, leaving the current list sizes unchanged.

To clear all entries in the current history list, run the :code:`Clear-AWSHistory` cmdlet.


.. _pstools-awshistory-examples:

:code:`$AWSHistory` Examples
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Enumerate the details of the AWS cmdlets that are being held in the list to the pipeline.

.. code-block:: none

    PS C:\> $AWSHistory.Commands

Access the details of the last AWS cmdlet that was run:

.. code-block:: none

    PS C:\> $AWSHistory.LastCommand

Access the details of the last service response received by the last AWS cmdlet that was run. If an
AWS cmdlet is paging output, it may make multiple service calls to obtain either all data or the
maximum amount of data (determined by parameters on the cmdlet).

.. code-block:: none

    PS C:\> $AWSHistory.LastServiceResponse

Access the details of the last request made (again, a cmdlet may make more than one request if it is
paging on the user's behalf). Yields $null unless service request tracing is enabled.

.. code-block:: none

    PS C:\> $AWSHistory.LastServiceRequest


.. _pstools-page-to-completion:

Automatic Page-to-Completion for Operations that Return Multiple Pages
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For service APIs that impose a default maximum object return count for a given call or that support
pageable result sets, all cmdlets "page-to-completion" by default. Each cmdlet makes as many calls
as necessary on your behalf to return the complete data set to the pipeline.

In the following example, which uses :code:`Get-S3Object`, the :code:`$c` variable contains
:code:`S3Object` instances for *every* key in the bucket :code:`test`, potentially a very large data
set.

.. code-block:: none

    $c = Get-S3Object -BucketName test

If you want to retain control of the amount of data returned, you can continue to use parameters on
the individual cmdlets (e.g. :code:`MaxKey` on :code:`Get-S3Object`) or you can explicitly handle
paging yourself by using a combination of paging parameters on the cmdlets, and data placed in the
:code:`$AWSHistory` variable to get the service's next token data. The following example uses the
MaxKeys parameter to limit the number of :code:`S3Object` instances returned to no more than the
first 500 found in the bucket.

.. code-block:: none

    $c = Get-S3Object -BucketName test -MaxKey 500

To know if more data was available but not returned, use the :code:`$AWSHistory` session variable
entry that recorded the service calls made by the cmdlet.

If the following expression evaluates to $true, you can find the :code:`next` marker for the next
set of results using :code:`$AWSHistory.LastServiceResponse.NextMarker`.

.. code-block:: none

    $AWSHistory.LastServiceResponse -ne $null && $AWSHistory.LastServiceResponse.IsTruncated

To manually control paging with :code:`Get-S3Object`, use a combination of the :code:`MaxKey` and
:code:`Marker` parameters for the cmdlet and the :code:`IsTruncated`/:code:`NextMarker` notes on the
last recorded response. In the following example, the variable :code:`$c` contains up to a maximum
of 500 :code:`S3Object` instances for the next 500 objects that are found in the bucket after the
start of the specified key prefix marker.

.. code-block:: none

    $c = Get-S3Object -BucketName test -MaxKey 500 -Marker $AWSHistory.LastServiceResponse.NextMarker



.. _getting-started-see-also:

See Also
--------

* :ref:`pstools-using`
