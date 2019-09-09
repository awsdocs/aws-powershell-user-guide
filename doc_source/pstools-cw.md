# CloudWatch from the AWS Tools for Windows PowerShell<a name="pstools-cw"></a>

This section shows an example of how to use the Tools for Windows PowerShell to publish custom metric data to CloudWatch\.

This example assumes that you have set default credentials and a default region for your PowerShell session\. 

## Publish a Custom Metric to Your CloudWatch Dashboard<a name="pstools-cw-custom-metric-publish"></a>

The following PowerShell code initializes an CloudWatch `MetricDatum` object and posts it to the service\. You can see the result of this operation by navigating to the [CloudWatch console](https://console.aws.amazon.com/cloudwatch/home)\.

```
$dat = New-Object Amazon.CloudWatch.Model.MetricDatum
$dat.Timestamp = (Get-Date).ToUniversalTime()
$dat.MetricName = "New Posts"
$dat.Unit = "Count"
$dat.Value = ".50"
Write-CWMetricData -Namespace "Usage Metrics" -MetricData $dat
```

Note the following:
+ The date\-time information that you use to initialize `$dat.Timestamp` must be in Universal Time \(UTC\)\.
+ The value that you use to initialize `$dat.Value` can be either a string value enclosed in quotes, or a numeric value \(no quotes\)\. The example shows a string value\.

## See Also<a name="see-also"></a>
+  [Using the AWS Tools for Windows PowerShell](pstools-using.md) 
+  [AmazonCloudWatchClient\.PutMetricData](https://docs.aws.amazon.com/sdkfornet/v3/apidocs/items/CloudWatch/MCloudWatchPutMetricDataPutMetricDataRequest.html) \(\.NET SDK Reference\)
+  [MetricDatum](https://docs.aws.amazon.com/AmazonCloudWatch/latest/APIReference/API_MetricDatum.html) \(Service API Reference\)
+  [Amazon CloudWatch Console](https://console.aws.amazon.com/cloudwatch/home) 