# Amazon S3 Bucket Creation Guide

Amazon S3 buckets are containers for storing objects (files, folders, and data).  
Every object in S3 must be stored inside a bucket, and bucket-level settings control security, versioning, and access.

---

## 1. General Configuration

- **AWS Region**:  
  Select the region closest to your users or applications.  
  Example: Asia Pacific (Mumbai) `ap-south-1`.

- **Bucket Type**:  
  - General Purpose: Recommended for most use cases. Stores data across multiple Availability Zones.  
  - Directory: Optimized for low-latency workloads. Stores data in a single Availability Zone.

- **Bucket Name Rules**:  
  - Must be unique across all AWS accounts.  
  - Length: 3 to 63 characters.  
  - Allowed characters: lowercase letters (a–z), numbers (0–9), periods (.), and hyphens (-).  
  - Example: `student-practice-bucket-001`

---

## 2. Object Ownership

- **ACLs Disabled (Recommended)**: All objects are owned by the bucket owner. Access is managed using only policies.  
- **ACLs Enabled**: Objects can be owned by other AWS accounts. Access is managed through ACLs and policies.  

The recommended option is **Bucket owner enforced**.

---

## 3. Block Public Access Settings

- **Block all public access**: Ensures no public or cross-account access.  
- If public access is required (e.g., for a static website), the settings can be customized.  

Important: Always validate that your applications will work correctly without public access before enabling this.

---

## 4. Bucket Versioning

- **Disabled (Default)**: Only the latest version of an object is kept.  
- **Enabled**: Maintains multiple versions of objects. Useful for data recovery and rollback.

Enabling versioning is recommended for critical workloads.

---

## 5. Tags (Optional)

Tags help organize buckets and track costs.  
Example:  
- Key: `Environment`, Value: `Development`  
- Key: `Project`, Value: `CloudTraining`

---

## 6. Default Encryption

- **SSE-S3**: Amazon-managed keys (default and recommended for beginners).  
- **SSE-KMS**: AWS Key Management Service-managed keys.  
- **DSSE-KMS**: Dual-layer encryption using AWS KMS.  

For most training scenarios, select **SSE-S3**.  

---

## 7. Advanced Settings

After creating the bucket, you can configure additional features:  
- Lifecycle Policies to move data to cheaper storage classes automatically.  
- Cross-Region Replication to maintain copies in another AWS region.  
- Event Notifications to trigger Lambda functions, SQS, or SNS when objects are uploaded.

---

## Step-by-Step Demo

1. Open the [Amazon S3 Console](https://s3.console.aws.amazon.com/s3/home).  
2. Click **Create Bucket**.  
3. Enter a bucket name (e.g., `student-demo-bucket-yourname`).  
4. Select region `ap-south-1 (Mumbai)`.  
5. Keep bucket type as **General Purpose**.  
6. Leave **ACLs Disabled**.  
7. Ensure **Block all public access** is enabled.  
8. (Optional) Enable **Versioning**.  
9. Add tags if required.  
10. Select encryption type (default **SSE-S3**).  
11. Click **Create Bucket**.

<img width="953" height="468" alt="Screenshot 2025-08-18 at 10 58 52 AM" src="https://github.com/user-attachments/assets/450475ea-0f79-449c-9cef-bd75dd85079f" />

<img width="1432" height="561" alt="Screenshot 2025-08-18 at 11 01 47 AM" src="https://github.com/user-attachments/assets/27d8c9cc-9782-45fd-8eee-051c89cf7cda" />

---

## References

- [Getting started with Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GetStartedWithS3.html)  
- [Uploading objects](https://docs.aws.amazon.com/AmazonS3/latest/userguide/upload-objects.html)  
- [S3 Versioning](https://docs.aws.amazon.com/AmazonS3/latest/userguide/manage-versioning-examples.html)  
