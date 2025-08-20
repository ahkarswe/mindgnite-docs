---
title: "Forward DNS Queries from AWS EC2 to On-Prem AD DNS Server"
tags: ["AWS", "Route53", "DNS", "Myanmar"]
last_updated: "2025-08-20"
---
# 🧭 SOP: Forward DNS Queries from AWS EC2 to On-Prem AD DNS Server  
**Version**: 1.0  
**Author**: Mindgnite Infrastructure Team  
**Last Updated**: August 2025  
**Purpose**: Enable EC2 instances in AWS VPC to resolve internal domain names (e.g. `corp.mindgnite.local`) hosted on an on-prem Active Directory DNS server.

---

## 📌 Overview  
This SOP sets up a Route 53 Resolver Outbound Endpoint to forward DNS queries from EC2 instances in a VPC to an on-prem DNS server.

**မြန်မာဘာသာ**: EC2 instance များမှ On-prem Active Directory DNS server သို့ DNS query များကို forward လုပ်နိုင်ရန် Route 53 Resolver Outbound Endpoint တစ်ခုကို configure လုပ်ခြင်း။

---

## 🧩 Architecture Diagram  
[ EC2 Instance ]
      |
      v
[ VPC DNS Resolver (169.254.169.253) ]
      |
      v
[ Route 53 Resolver Outbound Endpoint ]
      |
      v
[ On-Prem AD DNS Server (e.g. 10.0.0.5) ]


---

## 🛠️ Setup Steps

### Step 1: Create Outbound Resolver Endpoint  
**English**:
- Navigate to Route 53 → Resolver → Endpoints → Create Outbound Endpoint  
- Select your VPC and at least two subnets (for high availability)  
- Assign a security group that allows outbound UDP/TCP port 53  

**မြန်မာ**:
- Route 53 → Resolver → Endpoints → Outbound Endpoint အသစ်ဖန်တီးပါ  
- သင့် VPC နှင့် AZ နှစ်ခုအနည်းဆုံးပါဝင်သော subnet များကိုရွေးပါ  
- UDP/TCP port 53 ကို outbound ခွင့်ပြုသော security group တစ်ခုထည့်ပါ

---

### Step 2: Create DNS Forwarding Rule  
**English**:
- Go to Route 53 → Resolver → Rules → Create Rule  
- Rule Type: Forward  
- Domain Name: `corp.mindgnite.local`  
- Target IPs: On-prem DNS server IPs (e.g. `10.0.0.5`)  
- Associate rule with your VPC  

**မြန်မာ**:
- Route 53 → Resolver → Rules → Rule အသစ်ဖန်တီးပါ  
- Rule Type: Forward  
- Domain Name: `corp.mindgnite.local`  
- Target IP: On-prem DNS server IP (ဥပမာ `10.0.0.5`)  
- Rule ကို သင့် VPC နှင့်ချိတ်ဆက်ပါ

---

### Step 3: Configure EC2 DNS Behavior  
**English**:
- Ensure EC2 uses default VPC DNS (`169.254.169.253`)  
- When EC2 queries `corp.mindgnite.local`, it will be forwarded to on-prem DNS  

**မြန်မာ**:
- EC2 instance မှ VPC DNS (`169.254.169.253`) ကိုသုံးနေသည်ကိုသေချာပါစေ  
- EC2 မှ `corp.mindgnite.local` ကို query လုပ်သောအခါ On-prem DNS server သို့ forward လုပ်မည်

---

### Step 4: Verify Connectivity  
**English**:
- Ensure VPN or Direct Connect is active  
- Allow DNS traffic (UDP/TCP port 53) in firewalls, NACLs, and security groups  
- Test with `nslookup` or `dig` from EC2  

**မြန်မာ**:
- VPN သို့မဟုတ် Direct Connect ကိုအသုံးပြုထားသည်ကိုသေချာပါစေ  
- DNS traffic (UDP/TCP port 53) ကို firewall, NACL, security group များတွင်ခွင့်ပြုပါ  
- EC2 မှ `nslookup` သို့မဟုတ် `dig` ဖြင့် test လုပ်ပါ

---

## 🔐 Optional Enhancements  
- Enable Route 53 Resolver Query Logging to CloudWatch or S3  
- Add systemd health check service to monitor DNS resolution  
- Document fallback behavior if on-prem DNS is unreachable

---

## 📦 Contributor Checklist  
- [ ] Outbound endpoint created in correct subnets  
- [ ] Forwarding rule matches correct domain  
- [ ] EC2 DNS settings verified  
- [ ] VPN/Direct Connect connectivity tested  
- [ ] Query logs enabled (optional)  
- [ ] SOP reviewed in both English and မြန်မာ

---

## 📘 Glossary  
| Term | Definition | မြန်မာအဓိပ္ပါယ် |
|------|------------|------------------|
| VPC | Virtual Private Cloud | မျှဝေထားသောမူပိုင်ကွန်ယက် |
| Resolver | DNS query handler | DNS ဖြေဆိုစက် |
| Endpoint | Network access point | ဆက်သွယ်ရန်အချက် |
| Forwarding Rule | DNS query redirection | DNS query ပြောင်းပေးခြင်း |

---
