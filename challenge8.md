# Challenge Overview
The primary goal of this challenge was to access a secret stored in AWS Systems Manager (SSM) and retrieve the flag. As part of the challenge, we were provided with a starting IAM user, ctf-starting-user, with specific IAM policies granting us permissions to retrieve the secret. The challenge was focused on navigating through AWS permissions and tools like CloudFox to successfully retrieve the flag

### write a short analysis about the Assignment here
The challenge setup included the following key points:

User Permissions: The ctf-starting-user had three IAM policies attached:

SecurityAudit (AWS Managed): Allowed read-only access to AWS resources.
CloudFox (Customer Managed): Allowed usage of CloudFox to interact with AWS services.
its-a-secret (Customer Managed): Allowed access to retrieve a secret stored in AWS Secrets Manager and SSM.
Challenge Setup: Using CloudFox with the cloudfoxable profile, the goal was to identify the secret named its-a-secret stored within AWS SSM and Secrets Manager.

Primary Goal: Retrieve the secret and flag by leveraging the cloudfoxable profile to check the IAM permissions and access the secret.

### Steps Taken to Solve the Challenge

1. I began by confirming the identity of the user executing the commands using the following AWS CLI command:
aws --profile cloudfoxable sts get-caller-identity

2. Enumerated Secrets Using CloudFox
I then used CloudFox to enumerate the secrets stored in the AWS account with the following command:
cloudfox aws -p cloudfoxable secrets -v2
The command returned several secrets, including the one that we were interested in: /cloudfoxable/flag/its-a-secret. This gave me the necessary details to access the secret.

3. Retrieved the Secret from SSM
To retrieve the secret, I ran the following command using the AWS CLI:
aws --profile cloudfoxable --region us-west-2 ssm get-parameter --with-decryption --name /cloudfoxable/flag/its-a-secret

This successfully returned the flag, which was:
FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}

4. Checked Permissions Using CloudFox
I also used CloudFox to check the IAM permissions of ctf-starting-user and confirm why I had access to the secret. This step was crucial in understanding the role of IAM policies in granting access to sensitive resources.

cloudfox aws -p cloudfoxable permissions --principal ctf-starting-user -v2



### Cleanup instructions: 
```
No specific cleanup tasks were required for this challenge as it involved accessing a predefined secret rather than creating new resources. However, I ensured that the CloudFoxable profile remained intact for further challenges.

```

## Flag Retrieval
After successfully querying the secret, the flag was retrieved in the format:FLAG{ItsASecret::IsASecretASecretIfTooManyPeopleHaveAccessToIt?}



## Reflection
What was my approach?
The process involved following the instructions step by step, focusing on correctly configuring the environment and using the CloudFox tool to gather essential information. I leveraged the CloudFoxable profile to interact with AWS services and retrieve the secret.

What was the biggest challenge?
The most challenging part of the challenge was understanding and navigating the IAM policies to access the secret. Ensuring that I had the correct permissions to retrieve the secret required paying attention to the details provided by CloudFox, which identified the resources I had access to.

How did I overcome the challenges?
I overcame the challenges by carefully analyzing the IAM permissions and leveraging CloudFox to inspect and verify the permissions for ctf-starting-user. Once I understood the permissions, I was able to query and retrieve the secret successfully.

What led to the breakthrough?
The breakthrough came when I understood how the its-a-secret policy allowed the user to access the SSM parameter and retrieve the flag. Once I executed the correct AWS CLI command, I was able to pull the flag successfully.

On the blue side, how can the learning be used to properly defend the important assets?
In a real-world scenario, the key takeaway from this challenge would be the importance of securing sensitive data with appropriate IAM policies and ensuring that only authorized users can access critical secrets.

Principle of Least Privilege: Ensure that users and roles are only granted the permissions they need to perform their tasks, limiting access to sensitive resources like secrets.
Monitoring Access: Regularly audit IAM roles and permissions to detect and mitigate any potential unauthorized access.
Secure Access Methods: Use secure methods like AWS Systems Manager (SSM) for accessing resources, rather than exposing sensitive data through insecure methods.
By adhering to these best practices, sensitive cloud assets can be better protected, ensuring that only authorized personnel have access to critical resources.

