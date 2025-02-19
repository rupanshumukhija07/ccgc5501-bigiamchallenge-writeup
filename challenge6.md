# Challenge Overview
The CloudFoxable challenge requires deploying a series of Terraform-based AWS resources. After setting up the environment, I’ll need to find a flag that is part of a Capture the Flag (CTF) challenge. I’ll go through the setup and steps needed to get there.

### write a short analysis about the Assignment here
Key aspects of the challenge include:

AWS Setup: The assignment requires users to set up a new or existing AWS account, create a non-root user with administrative privileges, and configure their AWS CLI for interaction.

Terraform Deployment: By using Terraform, the user deploys the CloudFoxable infrastructure. The challenge encourages best practices such as using non-root accounts and leveraging IAM roles to minimize the attack surface.

CTF Challenge: At the heart of the exercise is the CTF (Capture The Flag) format, where flags are hidden within the AWS resources. The user must understand how to interpret Terraform outputs, properly configure access credentials, and work with cloud resources to locate and retrieve flags.

Security Learning: Through the process, the user learns important cloud security principles, such as ensuring the principle of least privilege, managing access to sensitive data, and using tools like Terraform to automate cloud infrastructure deployment. Additionally, it highlights how easy it is for sensitive information to be exposed if proper security measures aren’t followed.


### Setup instructions: 
```
Setup Instructions
1. Create an AWS Account
I created a new AWS account for this challenge to avoid interfering with any production resources.

2. Create a Non-root User
For security reasons, I created a non-root user with administrative access. This account is specifically for running Terraform to deploy CloudFoxable.

3. Generate an Access Key
I generated an access key for the user I just created. This key was used to configure my AWS CLI.

4. Install the AWS CLI
I installed the AWS CLI to configure my account and interact with AWS services. Once installed, I confirmed the setup was working using:
aws sts get-caller-identity

5. Install Terraform
I installed Terraform, which is required to deploy CloudFoxable. The setup is simple, and I added the binary location to my system's path.

6. Clone CloudFoxable Repository
I cloned the CloudFoxable repo from GitHub:
git clone https://github.com/BishopFox/cloudfoxable

7. Set Up the Environment
Next, I navigated to the cloudfoxable/aws directory and copied the example Terraform variables file:
cd cloudfoxable/aws
cp terraform.tfvars.example terraform.tfvars

I edited the terraform.tfvars file to point to the correct AWS profile, replacing YOUR_PROFILE with my profile name.

8. Run Terraform Commands
I ran the following commands to deploy the resources:
terraform init
terraform apply

```

## Flag Retrieval

After completing the setup, I carefully reviewed the output of the terraform apply command. It contained an important message showing how to set up my AWS CLI with the ctf-starting-user. This user was crucial for retrieving the first flag.

### Flag Syntax
The syntax for the flag is always: 
FLAG{challengeName::CamelCaseText}



## Reflection
What was my approach?
I followed the instructions carefully to set up my AWS environment using Terraform. Once the resources were deployed, I reviewed the output of terraform apply and configured my AWS CLI with the appropriate user. I then retrieved the first flag by reading through the Terraform output.

What was the biggest challenge?
The most challenging part was making sure I had the correct AWS profile and user set up properly. The challenge required close attention to the Terraform output, as it contained vital information to successfully configure the AWS CLI and access the flag.

How did I overcome the challenges?
I overcame the challenge by following each step precisely and verifying the details in the terraform apply output. It was essential to pay attention to all the instructions to configure my AWS CLI with the correct profile and user.

What led to the breakthrough?
The breakthrough came when I successfully set up my AWS CLI, which allowed me to access the resources deployed by Terraform. Once I confirmed everything was set up, I retrieved the first flag from the output.

On the blue side, how can the learning be used to properly defend the important assets?
The biggest takeaway here is that proper setup and access control are crucial when deploying cloud resources. If I were defending sensitive cloud assets, I'd ensure proper IAM roles and policies were in place to prevent unauthorized access. Additionally, monitoring resources like Terraform output and AWS CLI configurations would help ensure that sensitive data or flags are not exposed.

