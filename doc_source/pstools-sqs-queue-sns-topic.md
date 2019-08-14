# Amazon SQS, Amazon SNS and Tools for Windows PowerShell<a name="pstools-sqs-queue-sns-topic"></a>

This section provides instructions to:
+ Create an Amazon SQS queue and get queue ARN \(Amazon Resource Name\)\.
+ Create an Amazon SNS topic\.
+ Give permissions to the SNS topic so that it can send messages to the queue\.
+ Subscribe the queue to the SNS topic
+ Give IAM users or AWS accounts permissions to publish to the SNS topic and read messages from the SQS queue\.
+ Verify results by publishing a message to the topic and reading the message from the queue\.

## Create an Amazon SQS queue and get queue ARN<a name="pstools-create-sqs-queue"></a>

The following command creates an SQS queue:

```
New-SQSQueue -QueueName MyQueue -Region us-west-2
```

The URL of the created queue is returned:

```
https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue
```

The following command gets the queue ARN

```
Get-SQSQueueAttribute -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -AttributeName QueueArn -Region us-west-2
```

The ARN of the created queue is returned:

```
...
QueueARN : arn:aws:sqs:us-west-2:123456789012:MyQueue
...
```

## Create an Amazon SNS topic<a name="pstools-create-sns-topic"></a>

The following command creates an SNS topic:

```
New-SNSTopic -Name MyTopic -Region us-west-2
```

The ARN of the created topic is returned:

```
arn:aws:sns:us-west-2:123456789012:MyTopic
```

## Give permissions to the SNS topic<a name="pstools-permissions-sns-topic"></a>

The following command gives permissions to the SNS topic so that it can send messages to the queue:

```
# create the queue and topic to be associated
   $qurl = New-SQSQueue -QueueName "myQueue"
   $topicarn = New-SNSTopic -Name "myTopic"

   # get the queue ARN to inject into the policy; it will be returned
   # in the output's QueueARN member but we need to put it into a variable
   # so text expansion in the policy string takes effect
   $qarn = (Get-SQSQueueAttribute -QueueUrl $qurl -AttributeNames "QueueArn").QueueARN

   # construct the policy and inject arns
   $policy = @"
   {
       "Version": "2012-10-17",
       "Id": "$qarn/SQSPOLICY",
       "Statement": [
           {
               "Sid": "1",
               "Effect": "Allow",
               "Principal": "*"
               },
               "Action": "SQS:SendMessage",
               "Resource": "$qarn",
               "Condition": {
                   "ArnEquals": {
                       "aws:SourceArn": "$topicarn"
                   }
               }
           }
       ]
   }
   "@

   # set the policy
   Set-SQSQueueAttribute -QueueUrl $qurl -Attribute @{ Policy=$policy }
```

The following is returned:

```
ServiceResponse
---------------
<?xml version="1.0" encoding="utf-16"?>...
```

## Subscribe the queue to the SNS topic<a name="pstools-subscribe-queue-topic"></a>

The following command subscribes the queue *MyQueue* to the SNS topic *MyTopic*:

```
Connect-SNSNotification -TopicARN arn:aws:sns:us-west-2:123456789012:MyTopic -Protocol SQS -Endpoint arn:aws:sqs:us-west-2:123456789012:MyQueue -Region us-west-2
```

The Subscription Id is returned:

```
arn:aws:sns:us-west-2:123456789012:ps-cmdlet-topic:f8ff77c6-e719-4d70-8e5c-a54d41feb754
```

## Give permissions<a name="pstools-permissions-publish-read"></a>

The following command gives permission to perform the `sns:Publish` action on the topic *MyTopic* 

```
Add-SNSPermission -TopicArn arn:aws:sns:us-west-2:123456789012:MyTopic -Label ps-cmdlet-topic -AWSAccountIds 123456789012 -ActionNames publish -Region us-west-2
```

The following is returned:

```
ServiceResponse
---------------
<?xml version="1.0" encoding="utf-16"?>...
```

The following command gives permission to perform the `sqs:ReceiveMessage` and `sqs:DeleteMessage` actions on the queue *MyQueue* 

```
Add-SQSPermission -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -Region US-West-2 -AWSAccountId "123456789012" -Label queue-permission -ActionName SendMessage, ReceiveMessage
```

The following is returned:

```
ServiceResponse
---------------
<?xml version="1.0" encoding="utf-16"?>...
```

## Verify results<a name="pstools-verify-publish-read"></a>

The following command publishes a message to the SNS topic *MyTopic* 

```
Publish-SNSMessage -TopicArn arn:aws:sns:us-west-2:123456789012:MyTopic -Message "Have A Nice Day!" -Region us-west-2
```

The `MessageId` is returned:

```
4914beb6-f8d2-5568-989f-f7909cefab79
```

The following command retrieves the message from the SQS queue *MyQueue* 

```
Receive-SQSMessage -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -Region us-west-2
```

The following is returned:

```
MessageId     : 03204f1d-1d65-4733-9eed-fc9cd514873a
ReceiptHandle : uUk89DYFzt3SjcTMtVq9VLAxpcJU5hHOKkInt+Hq6AxnWLGl1Eg1RLnPlIrkrflNmujk8+p2HrTCw0+1nLHAA+rfcy0m0f7Hxvm9iGR
              WMcFcCp4woccvYwQJW/if62D8R14v4JtSltEiY2ukxl/Zb4xqC9WN3+M0YZ/HW1euFb/tIE0qLQnKcOyoQ4Hj1d5WGc/IFo0cYNvOuM
              x8pRxeyOHKpah8OTrFiQFcCXbMKiuTqOI6yceInyAJ8YWwfKpjatc2zUcq5PqcrYMtbs4jK/zJc4uVhZNMUmCu2fA5EM4=
MD5OfBody     : 60509281ad1bfd6980e84f9d64bbf9ab
Body          : {
                "Type" : "Notification",
                "MessageId" : "4914beb6-f8d2-5568-989f-f7909cefab79",
                "TopicArn" : "arn:aws:sns:us-west-2:803981987763:MyTopic",
                "Message" : "Have A Nice Day!",
                "Timestamp" : "2012-11-21T05:09:17.905Z",
                "SignatureVersion" : "1",
                "Signature" : "GpF4Dhb5GotbtK883ccm1s59+7vnZMdcjxrAVYU7+igDFVWrvI6/bDfws5GcjT/IP9GxG6UJ55b8pu1+jzujaN
              YhZpr52mJfQHGRtM8FN0IAcCDDRQ00tXCHlOa6GP1s7RVIUNgCOzR/tbCCpJolGace+j0F1uf26LN4453RR6o=",
                "SigningCertURL" : "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7b
              b5f59f96de52f.pem",
                "UnsubscribeURL" : "https://sns.us-west-2.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:s
              ns:us-west-2:803981987763:ps-cmdlet-topic:f8ff77c6-e719-4d70-8e5c-a54d41feb754"
              }
Attribute     : {}
```