# Challenge number 
1

## Challenge Statement
Buckets of Fun

## IAM Policy
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b/*"
        },
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:ListBucket",
            "Resource": "arn:aws:s3:::thebigiamchallenge-storage-9979f4b",
            "Condition": {
                "StringLike": {
                    "s3:prefix": "files/*"
                }
            }
        }
    ]
}
```
### write a short analysis about the IAM policy here
```
For my "bigiamchallenge" assignment, I was given a set of IAM policies that helped me figure out what I had access to and what I didn’t.

What do I have access to?
The policies granted me the ability to do two main things. First, I could list the files in the files/ folder of the S3 bucket, which was done with the s3:ListBucket permission. This meant I could see which files were available within that folder. Second, I had permission to download any file from the S3 bucket using the s3:GetObject permission. These two permissions were crucial because they allowed me to find and access the file that contained the flag.

What don’t I have access to?
There were a few things I couldn’t do. For example, I didn’t have permission to upload or delete any files from the S3 bucket. I couldn’t view the files outside of the files/ folder either, because the s3:ListBucket permission was limited to that specific directory. Also, I couldn’t perform any other actions like modifying the bucket settings or managing other permissions.

What do I find interesting?
What I found interesting was how the policies were designed to allow just enough access for me to complete the challenge without giving me full control over the bucket. I could only list and download files, which meant that I had to figure out exactly where to look and how to use those permissions to find the flag. The fact that the flag was hidden in a file (flag1.txt) in the files/ folder made it a pretty straightforward process once I understood the policies, but it also showed me how important understanding IAM policies is when it comes to controlling access in cloud environments.
```

## Solution
Detailed steps to the flag
Step 1: Reviewing IAM Policies
I started by checking the IAM policies. They gave me two key permissions:

List files in the files/ folder (s3:ListBucket).
Download files from the bucket (s3:GetObject).
Step 2: Listing Files
I used the s3:ListBucket permission to list files in the files/ directory. I found a file named flag1.txt.

Step 3: Downloading the Flag
Using the s3:GetObject permission, I downloaded the flag1.txt file and found the flag inside.

Step 4: Understanding the Permissions
The policies gave just enough access for me to complete the task—listing and downloading files, but not modifying anything else.

## Reflection
* What was your approach?
I reviewed the IAM policies to understand the permissions I had: listing files in the files/ folder and downloading them. I used these permissions to list the files and found flag1.txt, which I then downloaded to get the flag.

* What was the biggest challenge?
The biggest challenge was figuring out exactly what I could do with the permissions. At first, I wasn’t sure if I had access to the right files or if I could view anything outside the files/ folder.

* How did you overcome the challenges?
I overcame this by carefully analyzing the policies and realizing I could only list and download files from the files/ folder. This narrowed down my search to that specific area.

* What led to the break through?
The breakthrough happened when I figured out that the IAM policies only allowed access to the files/ folder. Once I focused there, I found flag1.txt and downloaded it.

* On the blue side, how can the learning be used to properly defend the important assets? 
This exercise highlighted the importance of least privilege access—only granting necessary permissions. In real-world scenarios, tightly scoped IAM policies can prevent unauthorized access and help protect sensitive data from accidental or malicious actions.

