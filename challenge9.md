# Challenge Overview

The primary goal of this challenge was to access a secret stored in AWS Systems Manager (SSM) and retrieve the flag. The challenge focused on AWS IAM role assumption and permissions management, specifically navigating the steps involved in assuming a role, analyzing IAM permissions, and retrieving a secret from SSM.

### write a short analysis about the Assignment here

The challenge setup involved assuming the Ertz IAM role from an initial user (non-root-user). Key tasks included understanding the permissions attached to the user and role, assuming the role, and retrieving a specific secret from SSM (AWS Systems Manager). The challenge required using AWS CLI commands and configuring IAM roles to access the required resources.

User Permissions: The non-root-user had AdministratorAccess, but was initially not permitted to assume the Ertz role. By updating the role's assume policy to include this user, access was granted.

Ertz Role: The Ertz role had an AssumeRolePolicyDocument that allowed only the ctf-starting-user to assume it. This required modifying the trust relationship to include non-root-user as a valid principal.

Secret Access: The primary goal was to access a secret named its-another-secret stored in SSM and retrieve the flag.

### Steps Taken to Solve the Challenge

1. Initial Role Assumption Attempt:
 I began by attempting to assume the Ertz role using the AWS CLI with the following command:
aws --profile cloudfoxable sts assume-role --role-arn arn:aws:iam::307946660251:role/Ertz --role-session-name Ertz

However, this failed as the non-root-user was not initially permitted to assume the role.

2. Modified Trust Relationship: 
Upon discovering the issue, I modified the AssumeRolePolicyDocument of the Ertz role to allow non-root-user to assume the role. This was done using the following policy update:{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "AWS": [
          "arn:aws:iam::307946660251:user/ctf-starting-user",
          "arn:aws:iam::307946660251:user/non-root-user"
        ]
      },
      "Action": "sts:AssumeRole"
    }
  ]
}


3. Assumed Role Access:
 After updating the trust policy, I successfully assumed the Ertz role by running:
 aws --profile cloudfoxable sts assume-role --role-arn arn:aws:iam::307946660251:role/Ertz --role-session-name Ertz

This returned temporary security credentials for further actions.

4. Secret Retrieval: 
Using the AWS CLI, I retrieved the secret from AWS Systems Manager (SSM) with the following command:
aws ssm get-parameter --name "/cloudfoxable/flag/its-another-secret" --with-decryption --region us-west-2

This successfully returned the flag:
FLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}

5. Analysis of Permissions: 
I also checked the Ertz role’s policies, which revealed the its-another-secret-policy granting permission to retrieve the secret. This allowed me to conclude that the role’s permissions were correctly aligned with the challenge’s objective.


### Cleanup instructions: 
```
No specific cleanup tasks were required for this challenge as it involved assuming an existing role and retrieving a predefined secret. However, I ensured the CloudFoxable profile remained intact for use in future challenges.


```

## Flag Retrieval
After successfully querying the secret, the flag was retrieved in the format:FLAGFLAG{ItsAnotherSecret::ThereWillBeALotOfAssumingRolesInThisCTF}



## Reflection
What was my approach?
The approach involved carefully following the instructions to assume the Ertz role and then accessing the secret stored in SSM. By using the AWS CLI and analyzing the IAM permissions and roles, I was able to retrieve the flag.

What was the biggest challenge?
The biggest challenge was understanding and modifying the IAM trust policies. The default AssumeRolePolicyDocument only allowed ctf-starting-user to assume the Ertz role, which initially prevented me from accessing it. Modifying this trust policy required careful analysis and configuration.

How did I overcome the challenges?
I overcame this challenge by modifying the role’s assume policy, allowing non-root-user to assume the role. After assuming the role, I leveraged AWS CLI to retrieve the secret and analyze the permissions attached to the role.

What led to the breakthrough?
The breakthrough came when I successfully updated the trust relationship for the Ertz role, enabling non-root-user to assume it and proceed with the task.

On the blue side, how can the learning be used to properly defend the important assets?
In a real-world scenario, the following defensive practices would be vital to protect sensitive cloud assets:

Principle of Least Privilege: Ensure that only necessary users and roles have the permissions to perform specific tasks, such as assuming roles or accessing sensitive data like secrets.

Monitoring Access: Regularly audit IAM roles and policies to ensure that unauthorized users do not have access to critical resources.

Secure Access Methods: Avoid using insecure methods to share sensitive information and prefer secure solutions like AWS SSM to manage secrets.

By adhering to these practices, cloud environments can be better secured, ensuring that only authorized personnel have access to critical resources and data.
