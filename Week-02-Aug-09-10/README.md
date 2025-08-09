# DevOps Training – Day 2
**Date:** 10-Aug-2025 

---

## 1. Git Basics
**What is Git?**  
Git is a distributed version control system that tracks changes in your code. It allows multiple people to work on a project without overwriting each other’s work.

**Why use Git?**
- Tracks changes over time  
- Enables collaboration  
- Allows rollback to previous versions  

**Key Commands:**
```bash
git init                # Initialize a new Git repository
git add <filename>      # Stage a file for commit
git commit -m "message" # Commit changes
git log                 # View commit history
```

**Hands-on Task:**
- Create a folder and initialize Git.
- Add a file and commit it.
- Check your commit history.

---

## 2. GitHub Repositories, Push/Pull, Branching
**What is GitHub?**  
GitHub is a cloud-based hosting service for Git repositories, enabling collaboration, backups, and code sharing.

**Git vs GitHub:**
- **Git** → Local version control on your computer.
- **GitHub** → Online hosting for Git repositories.

**Key Commands:**
```bash
# Push code to GitHub
git push origin main

# Pull code from GitHub
git pull origin main

# Branching
git branch feature-1     # Create a new branch
git checkout feature-1   # Switch to that branch
```

**Hands-on Task:**
- Push local changes to GitHub.
- Create a new branch and push changes.

---

## 3. Jira Boards, Epics, Stories, Tasks
**What is Jira?**  
Jira is a tool for managing Agile projects. It helps teams plan, track, and release software efficiently.

**Key Concepts:**
- **Epic** → Large body of work
- **Story** → A feature to be developed
- **Task** → Small unit of work
- **Board** → Visual tracker of project progress

**Steps to Practice:**
1. Log in to Jira.
2. Create a project board.
3. Add an Epic, a Story, and a Task.
4. Move tasks across workflow stages: To Do → In Progress → Done.

**Hands-on Task:**
- Create your own Jira board and populate it with sample Epics, Stories, and Tasks.

---

## 4. Visual Studio Code Setup, Extensions, Git Integration
**What is VS Code?**  
A lightweight, powerful code editor from Microsoft that supports multiple programming languages and extensions.

**Steps to Setup:**
1. Download & install VS Code from https://code.visualstudio.com.
2. Install useful extensions:
   - GitLens (Git integration)
   - Prettier (Code formatting)
3. Configure VS Code to work with GitHub.

**Hands-on Task:**
- Install VS Code.
- Install extensions.
- Connect VS Code to your GitHub account.

---

## 5. AWS Free Tier Account Creation
**What is AWS Free Tier?**  
A limited, free usage tier offered by AWS for 12 months to explore and test services without cost.

**Steps to Create:**
1. Sign up at https://aws.amazon.com/free.
2. Add payment method (for verification only).
3. Enable billing alerts to avoid charges.
4. Explore AWS Management Console.

**Hands-on Task:**
- Create your AWS Free Tier account.
- Enable cost alerts in Billing Dashboard.

---

## 6. Launch EC2 Instance, VPC, Subnets, Security Groups
**What is EC2?**  
Amazon Elastic Compute Cloud (EC2) is a scalable virtual server in the AWS cloud.

**Steps to Launch:**
1. Open EC2 Dashboard in AWS.
2. Click **Launch Instance**.
3. Choose **Amazon Linux 2 AMI**.
4. Select instance type **t2.micro**.
5. Configure VPC & subnet.
6. Add inbound rule for **SSH (port 22)**.
7. Launch and download the **.pem** key file.

**Hands-on Task:**
- Launch your own EC2 instance in AWS.

---

## 7. SSH Login to EC2, Install Packages, Pull & Deploy Code from GitHub
**What is SSH?**  
Secure Shell (SSH) is a protocol to securely connect to remote servers.

**Commands:**
```bash
ssh -i mykey.pem ec2-user@<public-ip>
sudo yum update -y
sudo yum install git -y
git clone <repository-url>
```

**Hands-on Task:**
- Connect to your EC2 instance using SSH.
- Install Git.
- Clone your GitHub repository.
- Run the application.
