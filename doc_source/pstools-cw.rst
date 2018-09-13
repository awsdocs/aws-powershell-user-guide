.. _pstools-cw:

#######################
|CW| from the |TWPlong|
#######################

This section shows an example of how to use the |TWP| to publish custom metric data to |CW|.

This example assumes that you have set default credentials and a default region for your PowerShell
session. Therefore, credentials and a region are not included in the invocation of the cmdlets.

.. _pstools-cw-custom-metric-publish:

Publish a Custom Metric to Your |CW| Dashboard
==============================================

The following PowerShell code initializes an |CW| MetricDatum object and posts it to the service.
You can see the result of this operation by navigating to the :console:`CloudWatch console <cloudwatch>`.

.. code-block:: none

    $dat = New-Object Amazon.CloudWatch.Model.MetricDatum
    $dat.Timestamp = (Get-Date).ToUniversalTime() 
    $dat.MetricName = "New Posts"
    $dat.Unit = "Count"
    $dat.Value = ".50"
    Write-CWMetricData -Namespace "Usage Metrics" -MetricData $dat

Note the following:

* The date-time information that you use to initialize :code:`$dat.Timestamp` must be in Universal
  Time (UTC).

* The value that you use to initialize :code:`$dat.Value` can be either a string value enclosed in
  quotes, or a numeric value (no quotes). A string value is shown previously.


See Also
========

* :ref:`pstools-using`

* :sdk-net-api:`AmazonCloudWatchClient.PutMetricData <CloudWatch/MCloudWatchPutMetricDataPutMetricDataRequest>` (.NET SDK Reference)

* `MetricDatum <http://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html>`_ (Service API Reference)

* :console:`Amazon CloudWatch Console <cloudwatch>`



