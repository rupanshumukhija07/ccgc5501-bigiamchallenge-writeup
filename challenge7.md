# Challenge Overview
The primary goal of the challenge was to set up a bastion host that can be used to access other resources in the "Assumed Breach: Application Compromise/Network Access" category.

### write a short analysis about the Assignment here
The instructions outlined several key steps:

Accessing the EC2 Instance
The easiest way to access the EC2 instance was to use AWS Systems Manager (SSM). This was accomplished by running the following command to find the instance ID of the newly deployed EC2 instance:
cloudfox aws -p cloudfoxable instances -v2

Install the Session Manager Plugin
I then followed the instructions to install the Session Manager plugin to be able to use SSM to connect to the instance.

Loot File and Connection Command
I checked the loot file generated from the instances command to find the necessary command to connect to the EC2 instance using SSM.

Using CloudFox for IAM Permissions
Once connected to the instance, I used CloudFox to analyze the IAM permissions that I had access to. This was a key step in ensuring that I had the right permissions to retrieve the flag.
Commands used:
 cloudfox aws -p cloudfoxable permissions
 cloudfox aws -p cloudfoxable permissions --principal reyna

It gave me the followinf S3 bucket resource that I used:
 arn:aws:s3:::cloudfoxable-bastion-81l6y  

aws s3 ls s3://cloudfoxable-bastion-81l6y
2025-03-03 10:04:17         70 flag.txt

aws s3 cp s3://cloudfoxable-bastion-81l6y/flag.txt -
{FLAG:bastion::ifYouHaveAccessToAnEC2YouHaveAccessToItsIamPermissions}

### Cleanup instructions: 
```
Once the flag was retrieved, I completed the cleanup tasks to disable the challenge:

Edit CloudFoxable Configuration
I edited the cloudfoxable/aws/terraform.tfvars file again, switching the challenge flag to false:bastion_enabled = false

Reapply Terraform Configuration
Finally, I ran the following command to apply the updated configuration and tear down the resources:terraform apply


```

## Flag Retrieval

After identifying the correct IAM permissions and executing the necessary commands, I successfully located the flag. The flag format followed the standard:FLAG{bastion::ifYouHaveAccessToAnEC2YouHaveAccessToItsIamPermissions}


## Reflection
What was my approach?
I followed the instructions step by step, focusing on correctly configuring my environment and using CloudFox to gather information. After deploying the resources, I ensured that I had the necessary tools (like the Session Manager plugin) and permissions to access the EC2 instance.


What was the biggest challenge?
The most challenging part was ensuring that I correctly understood and configured the IAM permissions to allow access to the EC2 instance and retrieving the flag. It required paying close attention to the information provided by CloudFox and Terraform outputs.

How did I overcome the challenges?
I overcame the challenges by carefully reviewing the Terraform outputs and the loot file to gather the necessary details. Once I had all the information, I used CloudFox to analyze my IAM permissions and successfully retrieved the flag.

What led to the breakthrough?
The breakthrough came when I was able to install the Session Manager plugin and use SSM to access the EC2 instance. This gave me the ability to investigate the permissions and eventually find the flag.

On the blue side, how can the learning be used to properly defend the important assets?
To properly defend cloud resources like the EC2 instance in this challenge, the key takeaway is the importance of:

Proper IAM Permissions: Ensuring that only authorized users have the necessary access to sensitive resources, such as EC2 instances.
Monitoring Access: Regularly reviewing and auditing IAM roles and permissions to detect any potential misconfigurations or unauthorized access.
Secure Access Mechanisms: Using secure access methods like SSM instead of direct SSH, which can be a more secure option for accessing EC2 instances.
By implementing these best practices, I would be able to ensure that sensitive cloud assets are well protected and only accessible by authorized users. This is critical for maintaining the integrity and security of cloud environments.

