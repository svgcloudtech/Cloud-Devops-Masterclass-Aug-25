# DevOps Training – Day 2
**Date:** 10-Aug-2025  

---

- **Git** installation steps (Git Bash recommended for Windows) → [Download Git](https://git-scm.com/downloads)  
- **GitHub account** → [Create here](https://github.com)  
  1. Go to [GitHub](https://github.com) and click **Sign Up**
  2. Enter your email, password, and username
  3. Verify your email address
  4. Choose **Free Plan**
  5. Complete setup and log in

- **VS Code** installed → [Download VS Code](https://code.visualstudio.com)  
  1. Download and install for your OS
  2. Install extensions: GitLens, Prettier, Live Server
  3. Open folder for project work

---

| Topic |
|-------|
| Git basics |
| GitHub push/pull, branching |
| Jira |
| VS Code setup |
| AWS account + EC2 launch |
| SSH + Deploy HTML to EC2 |

---

## 1. Git Basics

### 1.1 Configure Git
```bash
git --version
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

### 1.2 Create local repo & first commit
```bash
mkdir my-first-repo
cd my-first-repo
git init
echo "Hello Git" > hello.txt
git add hello.txt
git commit -m "first commit: add hello.txt"
git log --oneline
```

---

## 2. GitHub — Push/Pull & Branching

### 2.1 Create a repository on GitHub
1. Login to GitHub  
2. Click **New** → Name repo (e.g., `day2-html-demo`)  
3. Do not initialize with README (if pushing existing repo)  

### 2.2 Link local repo & push
```bash
git remote add origin https://github.com/<your-username>/day2-html-demo.git
git branch -M main
git push -u origin main
```

### 2.3 Create a branch & push
```bash
git checkout -b feature-html
# edit files
git add index.html
git commit -m "add index.html for demo"
git push -u origin feature-html
```

### 2.4 Merge branch
**Option A (GitHub UI):** Create Pull Request → Merge  
**Option B (Local):**
```bash
git checkout main
git merge feature-html
git push origin main
```

---

## 3. Jira Quick Practice
- Create a Project  
- Create an Epic (e.g., "Website Demo")  
- Create Stories & Tasks  
- Move tasks: **To Do → In Progress → Done**

---

## 4. VS Code Setup
- Install extensions: GitLens, Prettier, Live Server  
- Open project folder in VS Code  
- Use **Source Control** tab to stage, commit, push changes

---

# AWS EC2 Setup Guide

This guide covers AWS account setup and EC2 launch in one continuous flow.

---

## 1. AWS Account & Permissions
1. Go to [AWS Free Tier](https://aws.amazon.com/free) and click **Create an AWS Account**.
2. Enter account details and payment info (mandatory for activation).
3. Verify your account via phone OTP.
4. Log in to AWS Console.
5. Create an **IAM user** (avoid using root account).
6. Attach **AmazonEC2FullAccess** policy to the IAM user.
7. Enable **MFA** for the account.
8. Enable **billing alerts**.

---

## 2. Launch an EC2 Instance (Including Key Pair Creation)
1. AWS Console → **EC2** → **Launch Instance**.
2. **Name:** Choose a descriptive name (e.g., `demo-instance`).
3. **AMI:** Select **Amazon Linux 2**.
4. **Instance Type:** Choose **t2.micro** (Free Tier eligible).
5. **Key Pair:**
   - If you already have a key pair, select it from the list.
   - If not, click **Create new key pair**:
     - Enter a name (e.g., `demo-key`).
     - Select `.pem` format.
     - Download and save the file securely (you cannot download it again).
6. **Network Settings (Security Group):**
   - **Inbound Rule 1:** SSH (22) → My IP.
   - **Inbound Rule 2:** HTTP (80) → 0.0.0.0/0.
7. Click **Launch Instance**.
8. Once running, note the **Public IPv4 address**.

---

## 3. Connect to EC2
Use the `.pem` key to SSH into your instance:
```bash
ssh -i your-key.pem ec2-user@<EC2-Public-IP>


---

## 7. SSH & Deploy HTML to EC2

### 7.0 Create `index.html` locally
```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <title>Day 2 Training</title>
</head>
<body style="font-family: Arial; text-align:center; margin-top:40px;">
  <h1>Welcome to Day 2 Training!</h1>
  <p>This page is deployed from GitHub to EC2.</p>
</body>
</html>
```

### 7.1 Push to GitHub
```bash
git init
git add index.html
git commit -m "Add index.html"
git branch -M main
git remote add origin https://github.com/<your-username>/day2-html-demo.git
git push -u origin main
```

### 7.2 SSH into EC2
```bash
chmod 400 my-key.pem
ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP>
```

### 7.3 Install Apache & Git
```bash
sudo yum update -y
sudo yum install -y httpd git
sudo systemctl start httpd
sudo systemctl enable httpd
```

### 7.4 Deploy from GitHub
**Option A: Clone directly to `/var/www/html`**
```bash
cd /var/www/html
sudo rm -rf *
sudo git clone https://github.com/<your-username>/day2-html-demo.git .
sudo chown -R apache:apache /var/www/html
sudo chmod -R 755 /var/www/html
sudo systemctl restart httpd
```

**Option B: Clone in home directory and copy**
```bash
cd ~
git clone https://github.com/<your-username>/day2-html-demo.git
sudo cp ~/day2-html-demo/index.html /var/www/html/index.html
sudo chown apache:apache /var/www/html/index.html
sudo systemctl restart httpd
```

**Ensure Port 80 is open in Security Group inbound rules**

### 7.5 Verify
```bash
sudo systemctl status httpd
ls -l /var/www/html/index.html
curl -I http://localhost
```

### 7.6 Access in browser
```
http://<EC2_PUBLIC_IP>/
```

---

## 8. Pull Latest Code from GitHub and Redeploy

If you make changes in your GitHub repo and want to update the EC2 site:

```bash
ssh -i my-key.pem ec2-user@<EC2_PUBLIC_IP>

# Go to the web root
cd /var/www/html

# Pull the latest changes
sudo git pull origin main

# Restart Apache
sudo systemctl restart httpd
```

---

## 9. Hands-on Exercises
1. Create and deploy `index.html` to EC2  
2. Make a change in a feature branch → PR → merge → redeploy  
3. Try `scp` deployment as alternative  

---

## 10. Quick Command Reference

### Git
```bash
git init
git add .
git commit -m "msg"
git branch -M main
git checkout -b feature
git push -u origin feature
git pull origin main
```

### Apache on EC2
```bash
sudo yum install httpd git -y
sudo systemctl start httpd
sudo systemctl enable httpd
sudo git clone https://github.com/<user>/<repo>.git /var/www/html
```
