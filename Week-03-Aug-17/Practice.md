# AWS IAM & S3 – Practical Guide

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)](https://aws.amazon.com/)

This guide provides **step-by-step practice labs** you can follow while learning **IAM** and **S3** in AWS.  
Each section includes **hands-on instructions**, **sample policies**, and **best practices**.

---

## Table of Contents
1. [IAM Fundamentals](#iam-fundamentals)
   - [Create an IAM User](#create-an-iam-user)
   - [User Credentials (Console & CLI)](#user-credentials-console--cli)
   - [Groups and Policies](#groups-and-policies)
   - [IAM Roles](#iam-roles)
   - [Policies and Permissions](#policies-and-permissions)
   - [Deny Example](#deny-example)
   - [MFA & Security Best Practices](#mfa--security-best-practices)
   - [Permission Boundaries](#permission-boundaries)
   - [Cross-Account Access](#cross-account-access)
2. [Amazon S3 Labs](#amazon-s3-labs)
   - [Create a Bucket](#create-a-bucket)
   - [Upload & Download Objects](#upload--download-objects)
   - [Public vs Private Buckets](#public-vs-private-buckets)
   - [Bucket Policies](#bucket-policies)
   - [S3 Versioning](#s3-versioning)
   - [Lifecycle Rules](#lifecycle-rules)
   - [Retention Policies](#retention-policies)
   - [Delete a Bucket](#delete-a-bucket)
3. [Sample IAM Policies](#sample-iam-policies)
4. [References](#references)

---

# IAM Fundamentals

## Create an IAM User
1. Open **IAM Console** → **Users** → **Add user**.  
2. Enter a username (e.g., `student-user`).  
3. Select **Access type**:
   -  Console access → requires a password.
   -  Programmatic access → creates access key & secret key.  
4. Assign permissions:
   - Add user to an existing **Group** with policies, or
   - Attach policies directly.  
5. Download `.csv` with credentials.

---

## User Credentials (Console & CLI)
- Console login URL: `https://<account-id>.signin.aws.amazon.com/console`

---

## Groups and Policies
- Create a group `Developers`.  
- Attach managed policy `AmazonS3ReadOnlyAccess`.  
- Add students to this group.

---

## IAM Roles
- Used for **temporary access**.  
- Example: EC2 instance role with `AmazonS3FullAccess`.  
- Roles Anywhere → use IAM role credentials outside AWS.

---

## Policies and Permissions
Example – allow user creation:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "iam:CreateUser",
    "Resource": "*"
  }
}
```

---

## Deny Example
Explicitly deny S3 deletion:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "s3:DeleteObject",
      "Resource": "arn:aws:s3:::mybucket/*"
    }
  ]
}
```

---

## MFA & Security Best Practices
- Enable **MFA** for all IAM users.  
- Rotate keys regularly.  
- Principle of **Least Privilege**.  
- Use **Roles**, not long-term keys.  

---

## Permission Boundaries
Restrict user to **read-only S3**:

```json
{
  "Version": "2012-10-17",
  "Statement": {
    "Effect": "Allow",
    "Action": "s3:Get*",
    "Resource": "*"
  }
}
```

---

## Cross-Account Access
- Create role in **Account A** with trust policy.  
- Allow **Account B’s user** to assume it.  
- Attach necessary permissions.

---

# Amazon S3 Labs

## Create a Bucket
1. Go to **S3 Console** → Create bucket.  
2. Enter name: `student-bucket-<yourname>`.  
3. Select region.  
4. Block public access → keep enabled (best practice).  
5. Enable **versioning** if needed.

---

## Upload & Download Objects
```bash
aws s3 cp file.txt s3://student-bucket-yourname/
aws s3 cp s3://student-bucket-yourname/file.txt ./file.txt
```

---

## Public vs Private Buckets
- By default → **private**.  
- To make public:
  - Disable **Block Public Access**.  
  - Add **Bucket Policy** allowing `s3:GetObject`.

---

## Bucket Policies
Public read policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::student-bucket/*"
    }
  ]
}
```

---

## S3 Versioning
- Enable in **bucket properties**.  
- Upload same file twice → restores both versions.  
- Test deletion → restore using previous version.

---

## Lifecycle Rules
- Transition files to **Glacier** after 30 days.  
- Delete old versions after 365 days.

---

## Retention Policies
Use **S3 Object Lock**:
- **Governance mode** or **Compliance mode**.  
- Prevent object deletion until retention period expires.

---

## Delete a Bucket
```bash
aws s3 rb s3://student-bucket-yourname --force
```

---

# Sample IAM Policies

### Allow user to change their own password
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "iam:ChangePassword",
      "Resource": "arn:aws:iam::*:user/${aws:username}"
    }
  ]
}
```

### Read-only access to S3
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:Get*",
        "s3:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

---

# References
- [AWS IAM Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html)  
- [AWS S3 Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)  
- [IAM Best Practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)  
- [S3 Security Best Practices](https://aws.amazon.com/s3/security/)  
