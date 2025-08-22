# Week 4 – AWS EC2, Load Balancing, Auto Scaling & Security

## 📅 Schedule
**Saturday**
- 9:30 – 10:20 → EC2 Basics & EBS Volumes/Snapshots  
- 10:20 – 11:00 → Elastic Load Balancer (ELB)  
- 11:10 – 11:50 → Auto Scaling Basics  
- 11:50 – 12:30 → Security Best Practices in AWS  

---

## 9:30 – 10:20 → EC2 Basics & EBS Volumes/Snapshots

### What is Amazon EC2?
Amazon Elastic Compute Cloud (EC2) provides secure, resizable compute capacity in the cloud. It reduces upfront costs and enables scaling on-demand.

- **Instances** – Virtual servers in the cloud  
- **AMIs (Amazon Machine Images)** – Preconfigured templates with OS + software  
- **Instance Types** – Different CPU, memory, storage, networking balance  
- **Key Pairs** – For SSH authentication  
- **Security Groups** – Virtual firewalls to control inbound/outbound traffic  
- **Pricing Models** – On-Demand, Reserved, Spot, Savings Plans  

### Amazon EBS (Elastic Block Store)
- Persistent block storage for EC2 instances  
- Volume Types: gp3 (general), io2 (high IOPS), st1 (throughput), sc1 (cold HDD)  
- **Snapshots**: Incremental backups stored in S3  
  - Use cases: backups, disaster recovery, migration  

**Hands-On (CLI Examples):**
```bash
# Create a 10GB gp3 EBS volume
aws ec2 create-volume --size 10 --volume-type gp3 --availability-zone us-east-1a

# Attach volume to an instance
aws ec2 attach-volume --volume-id vol-123456 --instance-id i-123456 --device /dev/sdf

# Create snapshot from volume
aws ec2 create-snapshot --volume-id vol-123456 --description "Backup"
```

---

## 10:20 – 11:00 → Elastic Load Balancer (ELB)

### Types of Load Balancers
- **ALB (Application Load Balancer)** – Layer 7, HTTP/HTTPS, path/host-based routing  
- **NLB (Network Load Balancer)** – Layer 4, TCP/UDP, extreme performance  
- **GWLB (Gateway Load Balancer)** – Integrates with 3rd-party appliances  

### Core Concepts
- **Listeners** – Ports/protocols for incoming traffic  
- **Target Groups** – Instances/containers receiving traffic  
- **Health Checks** – Monitor instance health and reroute traffic  

**Hands-On (CLI Example):**
```bash
# Create an ALB
aws elbv2 create-load-balancer --name my-alb --subnets subnet-123 subnet-456

# Create target group
aws elbv2 create-target-group --name my-tg --protocol HTTP --port 80 --vpc-id vpc-123456

# Register targets
aws elbv2 register-targets --target-group-arn arn:aws:elasticloadbalancing:... --targets Id=i-123456
```

---

## 11:10 – 11:50 → Auto Scaling Basics

### What is Auto Scaling?
Automatically adjusts EC2 instance count to handle demand while minimizing cost.

### Components
- **Launch Template** – Defines AMI, instance type, security groups  
- **Auto Scaling Group (ASG)** – Manages EC2 fleet across AZs  
- **Scaling Policies** – Target tracking, Step, Scheduled  

### Benefits
- High availability  
- Elasticity (scale in/out based on load)  
- Cost optimization  

**Hands-On (CLI Example):**
```bash
# Create Auto Scaling group
aws autoscaling create-auto-scaling-group   --auto-scaling-group-name my-asg   --launch-template LaunchTemplateName=my-template,Version=1   --min-size 1 --max-size 5 --desired-capacity 2   --vpc-zone-identifier "subnet-123,subnet-456"
```

---

## 11:50 – 12:30 → Security Best Practices in AWS (Focus: EC2)

### Network Security
- Use **Security Groups** (stateful) and **NACLs** (stateless) correctly  
- Restrict SSH access using CIDR or Systems Manager Session Manager  

### IAM Best Practices
- Use **IAM Roles** instead of access keys  
- Apply **least privilege** policies  
- Rotate credentials regularly  

### Data Protection
- Encrypt **EBS volumes** and **snapshots** with AWS KMS  
- Enable backups with AWS Backup  

### Monitoring & Logging
- **CloudWatch** – Monitor EC2 metrics and set alarms  
- **CloudTrail** – Log and audit API calls  
- **GuardDuty** – Threat detection and alerts  

**Hands-On (CLI Examples):**
```bash
# Enable detailed monitoring on EC2
aws ec2 monitor-instances --instance-ids i-123456

# Enable encryption on new EBS volume
aws ec2 create-volume --size 20 --encrypted --kms-key-id alias/aws/ebs --availability-zone us-east-1a
```

---

## ✅ By the end of this session, you should be able to:

### 1. Launch and Secure EC2 Instances
- Log in to AWS Management Console and navigate to **EC2 Dashboard**
- Launch an EC2 instance using an **Amazon Machine Image (AMI)**
- Choose the right **instance type** (t2.micro for free tier, others for workloads)
- Configure **key pairs** for SSH access
- Assign a **security group** with rules (allow SSH :22, HTTP :80, HTTPS :443 as needed)
- Connect to the instance using SSH (`ssh -i mykey.pem ec2-user@<public-ip>`)
- Apply **IAM roles** to securely allow access to S3, CloudWatch, etc.

### 2. Manage EBS Volumes and Snapshots
- Create and attach a new **EBS volume** to an EC2 instance
- Format and mount the volume on Linux (`mkfs -t ext4 /dev/xvdf`, `mount /dev/xvdf /data`)
- Extend an existing volume (increase size) and resize filesystem
- Create an **EBS snapshot** for backup and verify in AWS console
- Restore a volume from a snapshot and attach it to another instance

### 3. Distribute Traffic with ELB (Elastic Load Balancer)
- Create an **Application Load Balancer (ALB)** in the console
- Configure **Listeners** (HTTP/HTTPS)
- Create **Target Groups** and register EC2 instances
- Configure **Health Checks** to monitor instance availability
- Test ALB DNS name and verify traffic is distributed

### 4. Configure Auto Scaling Groups
- Create a **Launch Template** (AMI, instance type, SG)
- Define an **Auto Scaling Group** (Min=1, Max=3, Desired=2)
- Attach the ASG to an ALB
- Configure a **Scaling Policy** (CPU > 70% scale out, CPU < 30% scale in)
- Simulate load and observe scaling events

### 5. Apply EC2 Security Best Practices
- Use **Security Groups** to restrict traffic
- Restrict **SSH** to specific IPs or use **Session Manager**
- Enable **EBS encryption** with AWS KMS
- Use **IAM Roles** instead of credentials on instances
- Monitor logs/metrics with **CloudWatch**
- Track user activity with **CloudTrail**
- Enable **GuardDuty** for threat detection

