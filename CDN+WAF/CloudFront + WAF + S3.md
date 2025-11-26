# CloudFront + WAF + S3 / ALB Architecture

## 📌 Overview
This architecture improves **performance**, **security**, and **availability** for modern web applications using:
- **Amazon CloudFront** → CDN for global fast delivery  
- **AWS WAF** → Web Application Firewall (Layer-7 security)  
- **Amazon S3 / ALB** → Application origin  
- **Shield, Route53, CloudWatch** → Additional reliability & protection  

---

# 1. Architecture Diagram (Conceptual)

       ┌──────────────────────────┐
       │        Route 53          │
       └────────────┬─────────────┘
                    │
                    ▼
       ┌──────────────────────────┐
       │   CloudFront (CDN)       │
       │  - Edge Locations         │
       │  - Caching                │
       └───────┬──────────────────┘
               │  Integrated At Edge
               ▼
       ┌──────────────────────────┐
       │        AWS WAF           │
       │  - SQLi, XSS Protection  │
       │  - Bot Control           │
       │  - Rate Limiting         │
       └─────────┬────────────────┘
                 │
    ┌────────────┼────────────┬──────────────┐
    ▼            ▼            ▼              ▼

┌────────┐   ┌────────┐ ┌────────┐ ┌───────────┐
│ S3│ALB │   API GW │ │ Custom │
│Static  │    EC2/EKS  │ Backend│ │ Origin    │
└────────┘   └────────┘ └────────┘ └───────────┘

---

# 2. CloudFront (CDN)

## What CloudFront Does
- Caches content at **global edge locations**  
- Delivers content with **low latency**  
- Reduces load on S3 or ALB  
- Adds an extra security layer  
- Uses **HTTPS** and free SSL via ACM  

## Key Features
- Edge caching  
- Signed URLs / cookies  
- Geo-blocking or geolocation routing  
- Origin failover (primary + secondary)  
- Integration with AWS WAF  

---

# 3. AWS WAF (Security Layer)

## What AWS WAF Protects Against
- SQL Injection  
- XSS  
- Path traversal  
- Malicious bots  
- Layer-7 DDoS  
- IP blocking  
- Rate limiting  

## Where WAF Can Be Attached
- CloudFront  
- ALB  
- API Gateway  
- AppSync  
- Verified Access  

## Common WAF Rules
- AWS Managed Rule Sets (SQLi, XSS, bot control)  
- Geo-block rules  
- IP allow/block lists  
- Rate-limit rules (e.g., 1000 requests / 5 minutes)  
- Custom regex rules  

---

# 4. CloudFront + S3 (Static Website Architecture)

## When to Use S3 as Origin
- Static websites  
- React/Angular/Vue build outputs  
- Images, CSS, JS, videos  
- Documentation sites  

## Architecture Flow


## Setup Steps
1. Create S3 bucket  
2. Upload static site  
3. Block Public Access → Enabled  
4. Create Origin Access Control (OAC)  
5. Configure CloudFront with S3 as origin  
6. Attach WAF WebACL  
7. Add Route53 → point domain to CloudFront  
8. Enable HTTPS (ACM certificate)  

## Benefits
- No servers  
- Highly scalable  
- Cost-efficient  
- Global performance boost  

---

# 5. CloudFront + ALB (Dynamic Web/App Architecture)

## When to Use ALB as Origin
- Backend APIs  
- Authentication-based apps  
- Dynamic content  
- Microservices  
- EC2, ECS, or EKS workloads  

## Architecture Flow


## Setup Steps
1. Create ALB  
2. Attach EC2/EKS/ECS  
3. Use ALB DNS as CloudFront origin  
4. Enable Origin Request Policies  
5. Attach WAF WebACL  
6. Update Route53 alias → CloudFront  
7. Enable HTTPS  

## Benefits
- Caches API responses  
- Protects backend from attacks  
- Hides backend IPs  
- Improves performance globally  

---

# 6. Security Best Practices

## CloudFront
- Enforce HTTPS only  
- TLS 1.2+  
- Use OAC to restrict S3  

## WAF
- Enable AWS Managed Rule Sets  
- Create custom rules for specific URL paths  
- Use rate-based rules  
- Allow only whitelisted IPs for admin routes  
- Block suspicious user agents  

## Origin (S3/ALB)
- Keep S3 bucket private  
- Restrict ALB security group to **CloudFront only**  
- Disable direct access  

---

# 7. Real-Time Project Example

## Use Case: E-commerce Application
Frontend: React (S3 + CloudFront)  
Backend: Node.js/Java APIs (ALB + EC2/EKS)  
Security: AWS WAF  

### Request Flow


### Benefits
- Faster page load  
- Secured from SQLi, XSS, bots  
- Lower backend load  
- Highly available and globally distributed  

---

# 8. Interview-Ready Summary

## CloudFront
- CDN to boost speed and reduce latency  
- Caches at edge locations  
- Supports HTTPS, signed URLs, geolocation  

## WAF
- Protects against SQLi, XSS, bots, HTTP floods  
- Best attached to CloudFront for maximum coverage  

## S3 as Origin
- For static apps  
- Use OAC + CloudFront + WAF  
- Highly scalable and cheap  

## ALB as Origin
- For dynamic apps  
- Works with EC2, ECS, or EKS  
- CloudFront adds caching + WAF adds protection  

---

# End of Document
