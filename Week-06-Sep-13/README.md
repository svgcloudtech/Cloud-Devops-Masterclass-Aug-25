# 🔐 AWS Virtual Private Cloud (VPC) – Updated Guide

## 📘 Topics Covered
- [VPC Overview](#vpc-overview)  
- [Subnets](#subnets)  
- [Internet Gateway](#internet-gateway)  
- [NAT Gateway](#nat-gateway)  
- [Network ACL (NACL)](#network-acl)  
- [Security Groups](#security-groups)  
- [VPC Flow Logs](#vpc-flow-logs)  
- [VPC Peering](#vpc-peering)  
- [VPC Endpoints](#vpc-endpoints)  
- [Site-to-Site VPN](#site-to-site-vpn)  
- [AWS Direct Connect](#aws-direct-connect)  
- [Transit Gateway](#transit-gateway)  
- [Summary](#summary)

---

## VPC Overview
- **VPC (Virtual Private Cloud)**: Your own isolated private network inside AWS.  
- **Scope**: Regional (spans all AZs in the region).  
- **Control**: You decide IP ranges (CIDR), create subnets, route tables, and gateways.  

📌 **Note:** A subnet always belongs to a **single Availability Zone (AZ)**.

---

## Subnets
- **Public Subnet**: Internet-facing (requires Internet Gateway).  
- **Private Subnet**: No direct internet access; can use NAT Gateway for outbound access.  
- **Route Tables** define how subnets communicate with each other and with the outside world.

---

## Internet Gateway
- Attached at **VPC level**.  
- Required for internet access for public subnets.  
- Outbound & inbound routes go through it.

---

## NAT Gateway
- Deployed in a **public subnet**.  
- Allows private subnets to **initiate outbound** internet traffic (e.g., software updates).  
- Managed by AWS, highly available within an AZ.

---

## Network ACL (NACL)
- **Subnet-level firewall**.  
- Rules: **Allow & Deny** supported.  
- **Stateless**: return traffic must be explicitly allowed.  
- Ordered list of rules, applied to all instances in subnet.

---

## Security Groups
- **Instance-level firewall**.  
- Rules: **Only Allow** supported.  
- **Stateful**: return traffic automatically allowed.  
- Can reference IPs or other security groups.

---

## NACL vs Security Groups
- **NACL**: First layer of defense, subnet-wide, stateless.  
- **Security Group**: Second layer, instance-specific, stateful.

---

## VPC Flow Logs
- Capture traffic metadata from:  
  - VPC  
  - Subnets  
  - Elastic Network Interfaces (ENIs)  
- Export to **S3** or **CloudWatch Logs** for analysis.

---

## VPC Peering
- Connect two VPCs privately using AWS backbone.  
- **Non-transitive**: if A ↔ B and B ↔ C, then A cannot talk to C unless peering is created separately.  
- CIDR ranges must **not overlap**.

---

## VPC Endpoints
- Access AWS services **privately** without internet.  
- **Gateway Endpoints**: S3, DynamoDB.  
- **Interface Endpoints (PrivateLink)**: Other AWS services.

---

## Site-to-Site VPN
- Encrypted tunnel over **public internet** between on-prem and AWS.  
- Requires **Customer Gateway (CGW)** and **Virtual Private Gateway (VGW)**.

---

## AWS Direct Connect
- **Private dedicated connection** from on-prem to AWS.  
- Pros: secure, stable, faster.  
- Cons: costly, long setup time.

---

## Transit Gateway
- Central hub to connect **thousands of VPCs** and on-premises networks.  
- Hub-and-spoke (star) topology.  
- Works with Direct Connect and VPN.

---

## Summary
- **VPC Structure**: VPC → Subnets (AZ-specific).  
- **Connectivity**: IGW (public), NAT GW (private-to-public).  
- **Security**: NACL (stateless, subnet-level), SG (stateful, instance-level).  
- **Observability**: Flow Logs for troubleshooting.  
- **Interconnectivity**: Peering, Endpoints, VPN, Direct Connect, Transit Gateway.
