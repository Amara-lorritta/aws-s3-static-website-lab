## **AWS S3 Static Website Lab**

## **Overview**

This hands-on lab demonstrates how to create, configure, and host a static website using Amazon S3 and the AWS CLI. You will launch and connect to an EC2 instance using Systems Manager, configure the AWS CLI, create an S3 bucket for website hosting, upload static web files, and automate updates with a bash script.

## **Objectives & Learning Outcomes**

By the end of this lab, I will be able to:

Run AWS CLI commands for IAM and S3 management

Deploy and host a static website using Amazon S3

Configure IAM policies and user permissions

Create and run a bash automation script to update your site

Understand public access settings, ACLs, and S3 website hosting

## **Architecture**

<img width="800" height="400" alt="ca33aa0a-393f-4780-8c5e-5f38c083fd3f" src="https://github.com/user-attachments/assets/9c808373-bea6-45bc-81ef-96849719c787" />

## **Commands and Steps**
```bash
# Step 1: Connect to EC2 via Systems Manager
sudo su -l ec2-user
pwd

# Step 2: Configure AWS CLI with IAM credentials
aws configure
# (Provide Access Key, Secret Key, Region = us-west-2, Output = json)

# Step 3: Create a unique S3 bucket
aws s3api create-bucket --bucket myunique-bucket123 --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2

# Step 4: Create new IAM user with S3 access
aws iam create-user --user-name awsS3user
aws iam create-login-profile --user-name awsS3user --password Training123!
aws iam attach-user-policy --policy-arn arn:aws:iam::aws:policy/AmazonS3FullAccess --user-name awsS3user

# Step 5: Configure S3 bucket for public web hosting
aws s3 website s3://myunique-bucket123/ --index-document index.html
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://myunique-bucket123/ --recursive --acl public-read

# Step 6: Verify uploads
aws s3 ls myunique-bucket123

# Step 7: Create automation script
cd ~
touch update-website.sh
vi update-website.sh
# Add the following inside:
# !/bin/bash
# aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://myunique-bucket123/ --recursive --acl public-read

chmod +x update-website.sh
./update-website.sh

```

## **Screenshots**
Screenshot Name	Image Link

S3 Bucket Created
<img width="1411" height="200" alt="s3" src="https://github.com/user-attachments/assets/a34fc1e9-10c4-4441-82d7-d7e56c33ae1f" />

IAM User Created
<img width="1000" height="215" alt="IAM user created" src="https://github.com/user-attachments/assets/9ab02135-1f29-41a4-9196-c7e16c329d40" />

Static Website Upload Success
<img width="1000" height="200" alt="upload" src="https://github.com/user-attachments/assets/61d84242-841f-4568-966a-4ada23ce2bcf" />


Website Endpoint Live	

<img width="700" height="500" alt="end point" src="https://github.com/user-attachments/assets/987a0985-0478-4434-bf2b-a6e48193931a" />


## **Tools Used**

Amazon S3 – Website hosting and object storage

AWS CLI – Command-line management

Amazon EC2 (Linux) – Command host environment

AWS Identity and Access Management (IAM) – User and policy control

Systems Manager (Session Manager) – Secure shell access

Bash Scripting – Automation

## **Key Takeaways**

S3 can host full static websites using the --website configuration.

IAM policies define granular access control for bucket operations.

Public access settings and ACLs must be configured carefully for security.

CLI automation with scripts improves deployment efficiency.

aws s3 sync optimizes uploads by transferring only modified files.

## **What Actually Happened** 

Connected to an Amazon Linux EC2 instance through Systems Manager.

Configured AWS CLI with temporary credentials.

Created an S3 bucket using CLI commands.

Created an IAM user (awsS3user) and attached AmazonS3FullAccess policy.

Disabled “Block all public access” and enabled static website hosting.

Extracted and uploaded website files (index.html, css/, images/) to the S3 bucket.

Accessed the website via the public S3 endpoint.

Built and tested an update script to redeploy changes automatically.

## **Author**

Amarachi Emeziem

