# Challenge number 
4

## Challenge Statement
Admin Only- We learned from our mistakes from the past. Now our bucket only allows access to one specific admin user. Or does it?

## IAM Policy
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-admin-storage-abf1321",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                },
                "ForAllValues:StringLike": {
                    "aws:PrincipalArn": "arn:aws:iam::133713371337:user/admin"
                }
            }
        }
    ]
}

### write a short analysis about the IAM policy here
```
* What do I have access to?
I have access to download (get) objects from the S3 bucket `thebigiamchallenge-admin-storage-abf1321` as long as I know the object path, thanks to the public `s3:GetObject` permission.
I can also list objects in the `files/*` prefix, but only if I'm the `admin` user due to the `aws:PrincipalArn` condition.

* What don't I have access to?
I cannot list objects unless I am the `admin` user.
I cannot modify the bucket or its contents, nor can I upload new objects.

* What do I find interesting?
The interesting part is the combination of public `s3:GetObject` access for all users (which allows downloading any file if you know the path) and the restricted `s3:ListBucket` permission that only allows the `admin` user to list the `files/*` prefix.
```

## Solution

Detailed Steps to Find and Download the Flag

Step 1: Review IAM Policy
The IAM policy clearly shows that while listing objects is restricted to the admin user, I can still download any file from the bucket if I know the file path. The s3:GetObject permission is public for all users.

Step 2: List Objects in the files/ Folder
Since I couldn't list the objects normally due to the restriction on s3:ListBucket, I used the --no-sign-request flag to bypass the need for AWS credentials and list the files in the files/ prefix. Here's the command I used:

aws s3 ls s3://thebigiamchallenge-admin-storage-abf1321/files/ --no-sign-request

The output showed:
2023-06-07 19:15:43         42 flag-as-admin.txt
2023-06-08 19:20:01      81889 logo-admin.png
I found that the flag-as-admin.txt file existed in the files/ folder.

Step 3: Download the flag-as-admin.txt File
Next, I used the aws s3 cp command to download the flag-as-admin.txt file. Since I knew the full path, I could download it without needing any credentials. The command I ran was:

aws s3 cp s3://thebigiamchallenge-admin-storage-abf1321/files/flag-as-admin.txt - --no-sign-request

The content of the file was printed as follows:
{wiz:principal-arn-is-not-what-you-think}

Step 4: Analyze the Flag
The flag displayed is {wiz:principal-arn-is-not-what-you-think}, which suggests there is some confusion or trickery in the IAM policy, particularly regarding the expected principal ARN that should be used to access resources.


## Reflection
What was my approach?
I started by analyzing the IAM policy to understand the permissions. I noticed that I couldn’t list the objects under files/*, but I could still access them if I knew their exact paths. I listed the objects with the --no-sign-request flag, found the flag-as-admin.txt file, and then downloaded it.

What was the biggest challenge?
The biggest challenge was figuring out how to list the objects in a bucket with restricted s3:ListBucket permissions. Since I couldn’t use the aws s3 ls command without credentials, I had to use the --no-sign-request flag to list the objects anonymously.

How did I overcome the challenges?
I bypassed the need for AWS credentials by using the --no-sign-request flag. This allowed me to list and download the files directly from the S3 bucket.

What led to the breakthrough?
The breakthrough came when I successfully listed the objects in the files/ folder using the --no-sign-request flag. Once I had the file path, I could easily download the file and retrieve the flag.

On the blue side, how can the learning be used to properly defend the important assets?
This exercise highlights the potential risks of using public s3:GetObject permissions for files that are meant to be private. Even though s3:ListBucket was restricted, I could still download the files if I knew the paths. To properly secure sensitive data, access controls for both listing and retrieving files should be carefully managed, ensuring that even those who can’t list the objects don’t have an easy way to guess their paths.