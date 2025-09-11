# Week 4 – Practice Guide

# EC2 + ALB + Security Groups + Auto Scaling + CloudWatch (Practice Guide)

> Hands‑on lab you can finish in ~2–3 hours. Using console Console. Works in any AWS region.
>
> **Architecture:** Internet → **ALB** → **Target Group** → **ASG (EC2 instances)** → (User Data boots simple web app). **CloudWatch** drives scaling.

```
[Client Browser]
        │
        ▼
 ┌───────────────┐        registers          ┌─────────────┐
 │  ALB (HTTP)   │ ────────────────────────► │ Target Grp  │ ──► Health checks
 └───────────────┘      sends traffic        └─────────────┘
        │                                        │
        ▼                                        ▼
                                   ┌───────────────────────────┐
                                   │ Auto Scaling Group (ASG)  │
                                   │  EC2 instances (LT/AMI)   │
                                   └───────────────────────────┘
                     ▲                                      │
                     └─────────── CloudWatch metrics ───────┘
                         (Target tracking policy: CPU 50%)
```

---

## 0) Prereqs & Clean‑room Setup (10 min)

- **IAM:** Use a lab/admin user with permissions for EC2, ELB, Auto Scaling, CloudWatch, IAM PassRole.
- **Region:** Choose one region (e.g., `ap-south-1`). Use it **everywhere** in this lab.
- **VPC:** Use the **default VPC** (simplest) with at least **2 public subnets** in different AZs.
- **Key pair (optional):** Create an RSA key pair if you plan to SSH (`EC2 → Key pairs → Create key pair`).

---

## 1) Security Groups (SG)
Create **two** SGs so we separate concerns:

### SG-ALB (for the Load Balancer)
- **Inbound:**  
  - HTTP `80` from `0.0.0.0/0` (and `::/0` for IPv6 if enabled).
- **Outbound:** Allow all (default is fine).

### SG-EC2 (for the instances)
- **Inbound:**  
  - HTTP `80` from **SG-ALB** (reference the SG, not `0.0.0.0/0`).
  - (Optional) SSH `22` from **your IP only**.
- **Outbound:** Allow all (default).

> Why? Public hits ALB; only ALB can reach instances on port 80. Least-privilege FTW.

---

## 2) Launch Template (LT) with User Data

Create a **Launch Template** the ASG will use.

1. **AMI:** Amazon Linux 2023 (or latest Amazon Linux 2).  
2. **Instance type:** `t3.micro` (Free‑tier‑friendly; use what your account allows).  
3. **Network:** Don’t fix subnets here; ASG will choose.  
4. **Security group:** **SG-EC2**.  
5. **User data:** paste the script below (**choose ONE**: AL2023 or AL2). It installs a tiny web server that prints the instance ID—great for verifying load balancing.

### User Data — Amazon Linux 2023
```bash
#!/bin/bash
set -euxo pipefail

# Update & install
dnf -y update
dnf -y install httpd

# Simple page with instance metadata
TOKEN=$(curl -sX PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 300" || true)
IID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id || echo "unknown")
AZ=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/placement/availability-zone || echo "unknown")

cat >/var/www/html/index.html <<EOF
<!doctype html>
<html>
  <head><title>ALB Demo</title></head>
  <body style="font-family: sans-serif;">
    <h1>ALB Demo OK</h1>
    <p>Instance ID: <b>${IID}</b></p>
    <p>AZ: <b>${AZ}</b></p>
    <p>Time: <b>$(date)</b></p>
  </body>
</html>
EOF

systemctl enable httpd
systemctl start httpd
```

### User Data — Amazon Linux 2 (alternative)
```bash
#!/bin/bash
set -euxo pipefail
yum -y update
yum -y install httpd
TOKEN=$(curl -sX PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 300" || true)
IID=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/instance-id || echo "unknown")
AZ=$(curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/placement/availability-zone || echo "unknown")
echo "ALB Demo OK - Instance ${IID} in ${AZ} - $(date)" >/var/www/html/index.html
systemctl enable httpd
systemctl start httpd
```

> Tip: Save your LT as **v1** (name it `alb-asg-demo-lt`).

---

## 3) Target Group (TG) (5 min)

- **Type:** Instances
- **Protocol/Port:** HTTP `80`
- **VPC:** your default VPC
- **Health checks:** path `/`, protocol HTTP; healthy threshold `3`, unhealthy `2`, timeout `5s`, interval `10s` (defaults are fine).
- **Register targets:** **Don’t** register now; ASG will attach instances automatically.

---

## 4) Application Load Balancer (ALB) 

- **Scheme:** Internet-facing
- **IP type:** IPv4 (or dualstack if you need IPv6)
- **VPC & Subnets:** pick **at least two public subnets** in different AZs
- **Security group:** **SG-ALB**
- **Listeners:** HTTP `:80`
  - Default action → Forward to **your Target Group**

Create ALB and note its **DNS name** (e.g., `my-alb-123.ap-south-1.elb.amazonaws.com`).

---

## 5) Auto Scaling Group (ASG) 

- **ASG name:** `alb-asg-demo`
- **Launch template:** pick `alb-asg-demo-lt (v1)`
- **VPC:** default
- **Subnets:** select **2+ public subnets** (same as ALB; it’s okay for demo)
- **Load balancing:** Attach to **existing Target Group**
- **Health checks:** Enable **ELB health checks** (recommended), grace period `60–120s`
- **Capacity:** Desired = `2`, Min = `2`, Max = `4` (you can change later)

### Scaling Policy (Target Tracking) — recommended
- **Metric type:** Average CPU utilization
- **Target value:** `50%`
- **Cooldowns:** default

> Why target tracking? It auto-adjusts capacity to keep average CPU near 50% without you managing explicit alarms.

Create the ASG and wait until **2 instances** are **InService/Healthy** in the Target Group.

---

## 6) Test the Load Balancing 

Open the **ALB DNS name** in your browser several times or run:

```bash
for i in {1..10}; do curl -s http://<your-alb-dns> | grep -E 'Instance|ALB Demo|AZ'; sleep 1; done
```

You should see **different instance IDs/AZs** across requests.

---

## 7) Trigger Scaling (two options)

### A) Quick & Safe demo: Scale with a manual change
- Edit the ASG and set **Desired capacity** to `3` (Max ≥ 3).
- Observe one more instance launching and joining the TG as healthy.
- Roll back to Desired = `2` after the demo (it will terminate one instance).

### B) Realistic demo: Trigger CPU load (optional)
SSH to **one** instance (if you opened port 22 from your IP) and run a small CPU load:

```bash
# Amazon Linux 2023
sudo dnf -y install stress-ng || true
stress-ng --cpu 2 --timeout 180s

# Amazon Linux 2
sudo yum -y install stress || true
stress --cpu 2 --timeout 180
```

Watch **EC2 → Metrics → CPUUtilization** or **CloudWatch → Metrics → EC2 → Per-Instance Metrics**.  
If average CPU of the ASG goes above 50% for several minutes, the ASG should **scale out** (to 3 or 4). When load drops, it should scale in back to 2.

> If your account blocks package repos, you can simulate load with: `dd if=/dev/zero of=/dev/null &` repeated a few times; kill with `pkill dd`.

---

## 8) CloudWatch Alarms for Visibility

Even with target tracking (which manages alarms internally), create your **own** alarm to *look* at the effect:

- **Metric:** `ALB → TargetResponseTime` or `ASG → GroupInServiceInstances`
- **Alarm:** e.g., `ALB 5xx errors > 5 for 5m` or `InServiceInstances < 2 for 5m`
- **Action:** just notify (email/SNS) for the lab.  
This helps students see CloudWatch → Alarm state changes during tests.

---

---

## 10) Clean‑Up (IMPORTANT)

Delete in this order to avoid “in use” errors:

1. **ASG** (set Desired=0, then delete ASG)
2. **Launch Template** (or keep for reuse)
3. **ALB listeners** and then **ALB**
4. **Target Group**
5. **Security Groups** (SG-EC2 then SG-ALB)
6. Any **CloudWatch alarms** created for the lab
7. **Key pair** (optional)

---

## FAQ / Common Pitfalls

- **Unhealthy targets?** Check SG-EC2 allows **port 80 from SG-ALB** and User Data actually started `httpd`.
- **ALB 5xx?** Usually target not passing health checks. Open `http://<instance_private_ip>` from a bastion to confirm app runs.
- **No scaling?** Use **target tracking (CPU 50%)** and generate enough load for several minutes. Check ASG **Max** value.
- **Stuck “OutOfService”?** Health check path `/` must return `200`. Verify `/var/www/html/index.html` exists.

---

### What you should be able to do by the end

- Explain ALB vs SG roles
- Build SGs with least privilege
- Create Launch Template + User Data
- Wire **ALB ⇄ Target Group ⇄ ASG** end‑to‑end
- Validate health checks and traffic distribution
- Configure **target tracking** scaling and (optionally) see it in action
- Clean resources safely
