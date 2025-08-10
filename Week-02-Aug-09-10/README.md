# DevOps Training – Day 2
**Date:** 10-Aug-2025

---

## Overview

This session covers essential DevOps skills including Git basics, GitHub workflows, Jira, VS Code setup, AWS EC2 launch, SSH access, and deploying a simple HTML site on EC2 using Python’s built-in HTTP server.

---

## Topics Covered

| Topic |
|-------|
| Git basics and configuration |
| GitHub push/pull, branching, and merge |
| Jira project and issue management |
| VS Code setup and Git integration |
| AWS account setup and EC2 launch |
| SSH into EC2 and deploy HTML site via Python HTTP server |
| Pull latest code and redeploy |

---

## 1. Git Basics

### 1.1 Configure Git (one-time setup)
```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list  # Verify your configuration
```
> Tip: Use `git status` frequently to check changes and branch status.  
> Note: Create a `.gitignore` file to exclude files/folders from version control.

### 1.2 Create a local repo & first commit
```bash
mkdir my-first-repo
cd my-first-repo
git init
echo "Hello Git" > hello.txt
git add hello.txt
git commit -m "first commit: add hello.txt"
git log --oneline
git status
```

---

## 2. GitHub — Push/Pull & Branching

### 2.1 Create GitHub repo
- Login to GitHub  
- Click **New** → Name repo (e.g., `day2-html-demo`)  
- Do not initialize with README if pushing an existing repo

### 2.2 Link local repo & push
```bash
git remote add origin https://github.com/<your-username>/day2-html-demo.git
git branch -M main
git push -u origin main
```

### 2.3 Create a branch & push changes
```bash
git checkout -b feature-html
# Make changes/edit files
git add index.html
git commit -m "add index.html for demo"
git push -u origin feature-html
```

### 2.4 Merge branch

**Option A: GitHub UI**  
Create Pull Request → Review → Merge  

**Option B: Local CLI**
```bash
git checkout main
git merge feature-html
git push origin main
```
> Note: If merge conflicts occur, resolve conflicts in the files then commit.

### 2.5 Useful commands & concepts
- `git fetch` - Fetches latest changes without merging  
- `git pull` - Fetches and merges changes from remote  
- SSH Authentication (optional): Generate SSH keys with `ssh-keygen` and add your public key to GitHub for password-less access.

---

## 3. Jira Quick Practice
- Create a Project  
- Create an Epic (e.g., "Website Demo")  
- Create Stories & Tasks linked to the Epic  
- Move tasks through the workflow: To Do → In Progress → Done  

> Tip: Reference Jira issue IDs in commit messages to link code changes with Jira tickets.

---

## 4. VS Code Setup
- Install extensions:  
  - GitLens (Git superpowers)  
  - Prettier (Code formatting)  
  - Live Server (Static web server)  
- Open your project folder in VS Code  
- Use Source Control tab to stage, commit, and push changes  
- Enable Settings Sync to save your extensions/settings across devices

---

## 5. AWS EC2 Setup Guide

### 5.1 AWS Account & Permissions
1. Go to [AWS Free Tier](https://aws.amazon.com/free) → Create AWS account  
2. Enter details, payment info, verify via phone OTP  
3. Log in to AWS Console  
4. Create an IAM user for daily use (do not use root account)  
5. Attach `AmazonEC2FullAccess` policy (or least privilege required)  
6. Enable MFA for security  
7. Enable Billing alerts for cost monitoring  

### 5.2 Launch an EC2 Instance
1. Go to EC2 Dashboard → Launch Instance  
2. Name your instance (e.g., `demo-instance`)  
3. Select Amazon Linux 2 AMI  
4. Choose `t2.micro` (Free Tier eligible)  
5. Configure Key Pair:  
   - Use existing or Create new key pair (e.g., `demo-key`)  
   - Download `.pem` file and save securely  
   - Set file permission: `chmod 400 demo-key.pem`  
6. Configure Security Group inbound rules:  
   - SSH (port 22) — Source: your IP (restrict for security)  
   - HTTP (port 80) — Optional if using Python server on port 8000  
   - Custom TCP (port 8000) — Source: Anywhere (0.0.0.0/0) for Python server access  
7. Launch instance and note Public IPv4 address  

---

## 6. Connect to EC2 Instance
```bash
chmod 400 demo-key.pem  # Ensure private key permissions are secure
ssh -i demo-key.pem ec2-user@<EC2-Public-IP>
```
> Troubleshooting:  
> - If you get Permission denied (publickey), check `.pem` file permissions and username.  
> - Use `ssh -vvv` for detailed connection debugging.

---

## 7. Deploy HTML to EC2 using Python HTTP Server

### 7.1 Push your code to GitHub  
If not already done:
```bash
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

### 7.2 SSH into your EC2 instance
```bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>
```

### 7.3 Update system packages
```bash
sudo yum update -y
```

### 7.4 Install Git and Python 3
```bash
sudo yum install -y git python3
```

### 7.5 Clone your GitHub repository
Replace `<your-username>` and `<repo-name>`:
```bash
cd /tmp
git clone https://github.com/<your-username>/<repo-name>.git
```

### 7.6 Navigate into the repo and start Python HTTP server
```bash
cd /tmp/<repo-name>
python3 -m http.server 8000
```
> This serves files from the current directory on port 8000.

---

### 7.7 Access your application  
In your browser:
```
http://<EC2-Public-IP>:8000/
```

**Notes:**
- Press `Ctrl+C` to stop the server.  
- To run in the background:
```bash
nohup python3 -m http.server 8000 &
```
- Make sure **port 8000** is open in your EC2 Security Group.

---

## 8. Pull Latest Code & Restart Server
When you update your GitHub repo:
```bash
cd /tmp/<repo-name>
git pull origin main
```
Restart the server if needed:
```bash
pkill -f "python3 -m http.server"
nohup python3 -m http.server 8000 &
```

---

## 9. Hands-on Exercises
- Create and deploy your own `index.html` file to EC2  
- Create a feature branch → make changes → open Pull Request → merge → redeploy  
- Try running the Python HTTP server on a different port (e.g., 8080)  

---

## 10. Quick Command Reference

### Git commands
```bash
git init
git add .
git commit -m "message"
git branch -M main
git checkout -b feature
git push -u origin feature
git pull origin main
```

---

## Additional Tips & Best Practices
- Always secure your `.pem` private keys (`chmod 400`). Never share.  
- Use IAM users with least privileges, not root.  
- Enable MFA on AWS accounts for better security.  
- Regularly check EC2 security groups to restrict access.  
- Use Git branching and PR workflows for collaboration and code quality.  
- Use VS Code Git integration for a smooth development experience.  
- Document commit messages clearly and link to Jira tickets when possible.
