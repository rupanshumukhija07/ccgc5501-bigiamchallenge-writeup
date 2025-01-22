# Challenge number 
2

## Challenge Statement
Google Analytics: We created our own analytics system specifically for this challenge. We think it's so good that we even used it on this page. What could go wrong?

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
* What do I have access to?
Send messages to the queue.
Receive messages from the queue.

* What don't I have access to?
Modify the queue or access other AWS resources.
See internal logs or configuration details of the queue.

* What do I find interesting?
The interesting part? The flag is likely hidden within one of the messages. To find it, I started by sending a test message and then receiving messages from the queue to see if any contained the flag. The key to finding the flag was interacting with the queue and carefully analyzing the responses
* etc
```

## Solution
Detailed steps to the flag
1. Understand the Permissions: The IAM policy allows sending and receiving messages from a specific AWS SQS queue. These are the primary actions needed to find the flag.
2. the receive-message command is used to pull messages from the queue and check if any contain the flag. 
aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2.
3. Look for the Flag: The flag is typically hidden in one of the messag Once it is found, the challenge is complete.
4. Found the flag.,



## Reflection
* What was your approach?
The approach was to directly interact with the AWS SQS queue by using the "receive message" command without sending any messages. The idea was to check the existing messages in the queue, as the flag might already be there.

* What was the biggest challenge?
The biggest challenge was not knowing what to expect. Since there was only one message in the queue, it took a bit of focus to realize that the flag was hidden within that single message.

* How did you overcome the challenges?
I overcame this by carefully reading the single message retrieved from the queue. Instead of overthinking or expecting multiple messages, I focused on the one message I had access to.

* What led to the break through?
The breakthrough came when I realized there was only one message in the queue, and the flag was hidden within it. Simply retrieving and thoroughly reviewing that message led to finding the flag.

* On the blue side, how can the learning be used to properly defend the important assets? 
This experience highlights the importance of monitoring and controlling access to sensitive resources like SQS queues. Ensuring that only authorized users can interact with resources and implementing proper logging can prevent unauthorized access. Also, understanding how messages are handled can help secure critical data in queues.
