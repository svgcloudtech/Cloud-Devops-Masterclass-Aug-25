# Week 4 – Student Practice Guide
This guide is designed for students to practice AWS concepts hands-on with both **Console** and **CLI** instructions.  
Topics include EC2, EBS, ELB, Auto Scaling, Security Best Practices, CloudWatch, and CloudTrail.

---

# Day 1 – EC2, EBS, ELB, Auto Scaling, Security

## Launch and Secure an EC2 Instance

### Console Steps:
1. Go to **AWS Management Console → EC2 → Launch Instance**
2. Choose an **AMI** (Amazon Linux 2 Free Tier is good)
3. Select **t2.micro** instance type
4. Configure instance details (default VPC, subnet)
5. Add storage (default EBS 8GB)
6. Add tags (Key: `Name`, Value: `Student-EC2`)
7. Configure **Security Group** → allow `SSH (22)` from *My IP*
8. Review and Launch → create a new **Key Pair** and download `.pem` file

### CLI Steps:
```bash
aws ec2 run-instances   --image-id ami-xxxxxxxx   --count 1   --instance-type t2.micro   --key-name student-key   --security-group-ids sg-xxxxxxx   --subnet-id subnet-xxxxxxx
```

**Expected Outcome:**  
- EC2 instance running in AWS console  
- Able to SSH: `ssh -i student-key.pem ec2-user@<public-ip>`

---

## Manage EBS Volumes and Snapshots

### Console Steps:
1. Go to **EC2 → Volumes → Create Volume**
2. Choose `gp3`, size = `10GB`, AZ = same as your instance
3. Attach volume to your instance (`/dev/sdf`)
4. Connect via SSH and run:
   ```bash
   sudo mkfs -t ext4 /dev/xvdf
   sudo mkdir /data
   sudo mount /dev/xvdf /data
   ```
5. Create a **Snapshot** from this volume

### CLI Steps:
```bash
aws ec2 create-volume --size 10 --volume-type gp3 --availability-zone us-east-1a
aws ec2 attach-volume --volume-id vol-123456 --instance-id i-123456 --device /dev/sdf
aws ec2 create-snapshot --volume-id vol-123456 --description "Student Backup"
```

**Expected Outcome:**  
- EBS volume mounted at `/data`  
- Snapshot visible in AWS console

---

## Distribute Traffic with ELB (Load Balancer)

### Console Steps:
1. Go to **EC2 → Load Balancers → Create Load Balancer**
2. Choose **Application Load Balancer (ALB)**
3. Configure name `student-alb`, scheme = Internet-facing
4. Add listeners → HTTP:80
5. Create **Target Group**, register EC2 instance
6. Add health checks (path `/`)
7. Copy ALB **DNS name** and test in browser

### CLI Steps:
```bash
aws elbv2 create-load-balancer --name student-alb --subnets subnet-1 subnet-2
aws elbv2 create-target-group --name student-tg --protocol HTTP --port 80 --vpc-id vpc-12345
aws elbv2 register-targets --target-group-arn arn:aws:... --targets Id=i-123456
```

**Expected Outcome:**  
- Traffic routed via ALB DNS  
- Load balanced between instances

---

## Configure Auto Scaling Group

### Console Steps:
1. Create **Launch Template** with AMI + instance type
2. Go to **EC2 → Auto Scaling Groups → Create**
3. Name = `student-asg`, select template
4. Min = 1, Max = 3, Desired = 2
5. Attach to ALB target group
6. Add scaling policy:
   - Scale Out: CPU > 70% for 5 min
   - Scale In: CPU < 30% for 5 min

### CLI Steps:
```bash
aws autoscaling create-auto-scaling-group   --auto-scaling-group-name student-asg   --launch-template LaunchTemplateName=student-template,Version=1   --min-size 1 --max-size 3 --desired-capacity 2   --vpc-zone-identifier "subnet-1,subnet-2"
```

**Expected Outcome:**  
- Auto Scaling group launches 2 EC2s  
- Group automatically scales with load
