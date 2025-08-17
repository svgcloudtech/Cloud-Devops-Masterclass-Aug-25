# Cloud Computing

[![AWS](https://img.shields.io/badge/AWS-Cloud-orange)](https://aws.amazon.com/)
[![Markdown](https://img.shields.io/badge/Markdown-Guide-green)](https://guides.github.com/features/mastering-markdown/)

---

## 📑 Table of Contents
1. [Introduction to Cloud Computing](#introduction-to-cloud-computing)
2. [What is Cloud Architecture?](#what-is-cloud-architecture)
   - [Cloud Architecture Best Practices](#cloud-architecture-best-practices)
   - [Benefits of Cloud Architecture](#the-benefits-of-cloud-architecture)
   - [Cloud Architecture Components](#cloud-architecture-components)
   - [Key Cloud Architecture Technologies](#key-cloud-architecture-technologies)
   - [Cloud Deployment Models](#cloud-deployment-models)
3. [Amazon Web Services (AWS)](#what-is-aws)
   - [Why Choose AWS?](#why-choose-aws)
4. [AWS Cloud Overview](#aws-cloud-overview)
5. [IAM Users, Roles & Policies](#iam-users-roles--policies)
6. [S3 Buckets: Create, Upload, Manage](#s3-buckets-create-upload-manage)
7. [S3 Versioning & Lifecycle Rules](#s3-versioning--lifecycle-rules)
8. [References](#references)

---

# Introduction to Cloud Computing
Cloud Computing is a technology that allows you to store and access data and applications over the internet instead of using your computer’s hard drive or a local server.

- **On-Demand Access:** Users can access services/resources instantly, scaling up or down.  
- **Infrastructure:** Depends on remote servers managed by CSPs.  
- **Benefits:** Cost saving, scalability, reliability, and accessibility.  

**Examples of data stored:** files, images, videos, documents.

---

# What is cloud architecture?
Cloud architecture defines the **front end, back end, networking, and delivery models** that combine to run applications.

- **Purpose:** Design strategy for connecting cloud infrastructure to meet business workloads & costs.  
- **Vendors:** AWS, Azure, Google Cloud, IBM Cloud, VMware.  
- **Business Value:** Lower infra costs, high scalability, faster innovation.  

👉 McKinsey predicts cloud could generate **USD 3 trillion EBITDA by 2030**.

---

## Cloud architecture best practices
- Automate operations (cost, reliability, security).  
- Respect data gravity (move compute to data).  
- Choose best platform per workload.  

---

## The benefits of cloud architecture
- **Custom migration strategies** (migrate DBs/apps).  
- **Accelerate modernization** (Kubernetes, self-service).  
- **Speed time-to-market** (Agile + DevOps).  
- **Innovation** (AI, ML, Quantum, Blockchain, IoT).  
- **Resiliency** (DR, uptime).  
- **Compliance & Security** (encryption, regulations).  

---

## Cloud architecture components
1. **Front-end** – GUIs, dashboards, client apps.  
2. **Back-end** – servers, storage, APIs, middleware, security tools.  
3. **Network** – internet, intranet, CDNs, SDN, load balancers.  
4. **Delivery models** – IaaS, PaaS, SaaS (and FaaS, BPaaS, Serverless).  

---

## Key cloud architecture technologies
- **Virtualization** → abstraction into VMs, containers.  
- **Automation** → CI/CD, provisioning, scaling.  

---

## Cloud deployment models
- **Public Cloud** → AWS, Azure, GCP.  
- **Private Cloud** → dedicated for one org.  
- **Hybrid Cloud** → mix of public/private/on-prem.  
- **Hybrid Multicloud** → multi-provider flexibility.  

---

# What is AWS?
Amazon Web Services (AWS) is the **world’s most comprehensive and widely adopted cloud platform**.  
It provides compute, storage, databases, networking, analytics, ML, AI, IoT, and more.

---

# Why Choose AWS?
- **Comprehensive Service Portfolio** → 200+ services.  
- **Scalability & Flexibility** → elastic resources.  
- **Cost-Effective** → pay-as-you-go pricing.  
- **Global Reach** → 37 regions, 99+ availability zones.  
- **Robust Security** → encryption, compliance, IAM.  

---

## AWS Cloud Overview
- Intro to AWS Global Infrastructure.  
- **37 Regions, 99+ AZs, 600+ Edge Locations.**  
- Local Zones & Wavelength Zones (low latency).  
- Used by startups, enterprises, governments.  

---

## IAM Users, Roles & Policies
- **Users:** Identities with credentials.  
- **Groups:** Shared policies for users.  
- **Roles:** Temporary access (cross-account, federated, **Roles Anywhere**).  
- **Policies:** JSON docs for allow/deny.  
- **Best Practices:** Least privilege, MFA, rotate keys.  

---

## S3 Buckets: Create, Upload, Manage
- **Durability:** 11 nines.  
- **Storage Classes:** Standard, IA, Glacier, **Express One Zone (new)**.  
- **Demo:** Create bucket, upload file, enable encryption.  
- **Management:** Organize with prefixes, event notifications.  

---

## S3 Versioning & Lifecycle Rules
- **Versioning:** Multiple object versions, restore deleted files.  
- **Lifecycle Rules:** Transition to Glacier, delete old versions.  
- **Demo:** Archive objects after 30 days.  
- **Outcome:** Cost optimization + data protection.  

---

## References
- [AWS Official Site](https://aws.amazon.com/)  
- [What is AWS?](https://aws.amazon.com/what-is-aws/)  
- [AWS Global Infrastructure](https://aws.amazon.com/about-aws/global-infrastructure/localzones/features/?pg=localzones&sec=hs)  
- [IAM Roles Anywhere](https://spacelift.io/blog/aws-iam-roles-anywhere)  
- [S3 Express One Zone](https://chaossearch.io/blog/amazon-s3-express-one-zone)  
