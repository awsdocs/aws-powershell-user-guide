.. _pstools-sqs-queue-sns-topic:

######################
|SQS|, |SNS| and |TWP|
######################

This section provides instructions to:

* Create an |SQS| queue and get queue ARN (Amazon Resource Name).

* Create an |SNS| topic.

* Give permissions to the SNS topic so that it can send messages to the queue.

* Subscribe the queue to the SNS topic

* Give |IAM| users or AWS accounts permissions to publish to the SNS topic and read messages from the SQS queue.

* Verify results by publishing a message to the topic and reading the message from the queue.

.. _pstools-create-sqs-queue:

Create an |SQS| queue and get queue ARN
=======================================

The following command creates an SQS queue:

.. code-block:: none

    New-SQSQueue -QueueName MyQueue -Region us-west-2

The URL of the created queue is returned:

.. code-block:: none

    https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue

The following command gets the queue ARN

.. code-block:: none

    Get-SQSQueueAttribute -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -AttributeName QueueArn -Region us-west-2

The ARN of the created queue is returned:

.. code-block:: none

    ...
    QueueARN : arn:aws:sqs:us-west-2:123456789012:MyQueue
    ...


.. _pstools-create-sns-topic:

Create an |SNS| topic
=====================

The following command creates an SNS topic:

.. code-block:: none

    New-SNSTopic -Name MyTopic -Region us-west-2

The ARN of the created topic is returned:

.. code-block:: none

    arn:aws:sns:us-west-2:123456789012:MyTopic


.. _pstools-permissions-sns-topic:

Give permissions to the SNS topic
=================================

The following command gives permissions to the SNS topic so that it can send messages to the queue:

.. code-block:: none

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

The following is returned:

.. code-block:: none

    ServiceResponse
    ---------------
    <?xml version="1.0" encoding="utf-16"?>...


.. _pstools-subscribe-queue-topic:

Subscribe the queue to the SNS topic
====================================

The following command subscribes the queue *MyQueue* to the SNS topic *MyTopic*:

.. code-block:: none

    Connect-SNSNotification -TopicARN arn:aws:sns:us-west-2:123456789012:MyTopic -Protocol SQS -Endpoint arn:aws:sqs:us-west-2:123456789012:MyQueue -Region us-west-2

The Subscription Id is returned:

.. code-block:: none

    arn:aws:sns:us-west-2:123456789012:ps-cmdlet-topic:f8ff77c6-e719-4d70-8e5c-a54d41feb754


.. _pstools-permissions-publish-read:

Give permissions
================

The following command gives permission to perform the :code:`sns:Publish` action on the topic
*MyTopic*

.. code-block:: none

    Add-SNSPermission -TopicArn arn:aws:sns:us-west-2:123456789012:MyTopic -Label ps-cmdlet-topic -AWSAccountIds 123456789012 -ActionNames publish -Region us-west-2

The following is returned:

.. code-block:: none

    ServiceResponse
    ---------------
    <?xml version="1.0" encoding="utf-16"?>...

The following command gives permission to perform the :code:`sqs:ReceiveMessage` and
:code:`sqs:DeleteMessage` actions on the queue *MyQueue*

.. code-block:: none

    Add-SQSPermission -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -Region US-West-2 -AWSAccountId "123456789012" -Label queue-permission -ActionName SendMessage, ReceiveMessage

The following is returned:

.. code-block:: none

    ServiceResponse
    ---------------
    <?xml version="1.0" encoding="utf-16"?>...


.. _pstools-verify-publish-read:

Verify results
==============

The following command publishes a message to the SNS topic *MyTopic*

.. code-block:: none

    Publish-SNSMessage -TopicArn arn:aws:sns:us-west-2:123456789012:MyTopic -Message "Have A Nice Day!" -Region us-west-2

The :code:`MessageId` is returned:

.. code-block:: none

    4914beb6-f8d2-5568-989f-f7909cefab79

The following command retrieves the message from the SQS queue *MyQueue*

.. code-block:: none

    Receive-SQSMessage -QueueUrl https://sqs.us-west-2.amazonaws.com/123456789012/MyQueue -Region us-west-2

The following is returned:

.. code-block:: none

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



