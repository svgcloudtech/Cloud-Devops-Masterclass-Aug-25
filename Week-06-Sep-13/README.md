# 🔐 AWS Virtual Private Cloud (VPC) – Updated Guide

## 📘 Topics Covered
- [VPC Overview](#vpc-overview)  
- [CIDR & Limits](#cidr--limits)  
- [Subnets](#subnets)  
- [Internet Gateway](#internet-gateway)  
- [Egress-only Internet Gateway](#egress-only-internet-gateway)  
- [NAT Gateway vs NAT Instance](#nat-gateway-vs-nat-instance)  
- [Route Tables](#route-tables)  
- [Bastion Host](#bastion-host)  
- [Network ACL (NACL)](#network-acl)  
- [Security Groups](#security-groups)  
- [NACL vs Security Groups](#nacl-vs-security-groups)  
- [VPC Flow Logs](#vpc-flow-logs)  
- [VPC Peering](#vpc-peering)  
- [VPC Endpoints](#vpc-endpoints)  
- [Site-to-Site VPN](#site-to-site-vpn)  
- [AWS Direct Connect](#aws-direct-connect)  
- [Transit Gateway](#transit-gateway)  
- [Summary](#summary)

---

## VPC Overview
- **VPC (Virtual Private Cloud)**: Isolated private network in AWS.  
- **Scope**: Regional resource (spans all AZs).  
- **Control**: Choose CIDR ranges (IPv4 & IPv6), subnets, route tables, gateways.  

---

## CIDR & Limits
- Max 5 VPCs per region (soft limit).  
- Max 5 CIDR blocks per VPC.  
- CIDR size: **/28 (16 IPs) to /16 (65,536 IPs)**.  
- Allowed ranges: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16.  
- CIDR must **not overlap** with corporate or other networks.  

---

## Subnets
- Each subnet is tied to **one AZ**.  
- AWS reserves **5 IPs per subnet** (first 4 + last 1).  
- Example: 10.0.0.0/24 → usable IPs = 251 (out of 256).  
- Subnet types: **Public** (via IGW) & **Private** (via NAT GW/Instance).  

---

## Internet Gateway
- Provides IPv4 & IPv6 internet access.  
- Must be attached to the VPC.  
- Only 1 IGW per VPC.  

---

## Egress-only Internet Gateway
- **IPv6 only**.  
- Allows outbound traffic to the internet but blocks inbound.  

---

## NAT Gateway vs NAT Instance
- **NAT Gateway**: AWS-managed, scalable, AZ-specific, recommended.  
- **NAT Instance**: Legacy, EC2-based, requires disabling Source/Destination check.  

---

## Route Tables
- Define routes for subnets.  
- Must explicitly add routes for IGW, VPC peering, and VPC endpoints.  

---

## Bastion Host
- Public EC2 instance used to SSH into private subnet instances.  

---

## Network ACL
- **Subnet-level firewall**.  
- **Stateless**: return traffic must be explicitly allowed.  
- Supports **Allow & Deny** rules.  
- Don’t forget to configure **Ephemeral Ports (1024–65535)**.  

---

## Security Groups
- **Instance-level firewall**.  
- **Stateful**: return traffic automatically allowed.  
- Only **Allow** rules supported.  

---

## NACL vs Security Groups
- **NACL**: Subnet level, stateless, allow/deny.  
- **SG**: Instance level, stateful, allow only.  

---

## VPC Flow Logs
- Capture metadata at **VPC, Subnet, or ENI** level.  
- Record accepted/rejected traffic.  
- Destination: **S3, CloudWatch Logs, Kinesis**.  
- Use **Athena or CloudWatch Logs Insights** for queries.  

---

## VPC Peering
- Private connection between 2 VPCs using AWS backbone.  
- Must not overlap CIDRs.  
- **Non-transitive**: A-B & B-C ≠ A-C.  
- Must update route tables for communication.  
- Supports **cross-account & cross-region**.  

---

## VPC Endpoints
- Provide **private connectivity** to AWS services without internet.  
- Types:  
  - **Gateway Endpoint**: S3, DynamoDB.  
  - **Interface Endpoint (PrivateLink)**: Most AWS services.  
- Redundant, scalable, removes need for IGW/NAT.  

---

## Site-to-Site VPN
- Connects **on-premises to AWS** over encrypted tunnel via public internet.  
- Requires **Customer Gateway (CGW)** and **Virtual Private Gateway (VGW)**.  
- Often used as a **backup for Direct Connect**.  

---

## AWS Direct Connect
- Dedicated private connection from data center to AWS.  
- More secure, stable, low-latency.  
- Expensive and longer setup time.  
- Supports **high resiliency** with multiple redundant connections.  

---

## Transit Gateway
- Hub-and-spoke model for connecting thousands of VPCs + on-prem.  
- **Transitive routing supported**.  
- Regional resource, can peer across regions.  
- Works with **Direct Connect Gateway & VPN**.  
- Supports **Resource Access Manager (RAM)** for cross-account sharing.  
- Supports **IP multicast** (unique to TGW).  

---

## Summary
- **VPC Structure**: CIDR, subnets (AZ-specific).  
- **Connectivity**: IGW, NAT GW, Egress-only IGW.  
- **Security**: NACL (stateless), SG (stateful).  
- **Access**: Bastion Host, Route Tables.  
- **Observability**: Flow Logs.  
- **Interconnectivity**: Peering, Endpoints, VPN, Direct Connect, Transit Gateway.  