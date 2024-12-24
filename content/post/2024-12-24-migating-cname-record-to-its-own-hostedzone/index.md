---
title: "Migrating a CNAME Record to Its Own Hosted Zone in AWS Route 53 with Zero Downtime"
date: 2024-12-24T18:23:13+05:00
featured_image: "featured.webp"
tags: ["AWS Route 53", "CNAME Record Migration", "Zero Downtime", "DNS Management", "Terraform", "AWS CLI", "Hosted Zone", "DNS Delegation", "Infrastructure as Code"]
---

## Overview
Migrating DNS records, especially CNAME records, between hosted zones in AWS Route 53 can be tricky and often introduces the risk of downtime. This blog post discusses the challenges of migrating a CNAME record from a parent hosted zone to its own hosted zone and provides a step-by-step guide to achieve zero downtime using a combination of AWS CLI and Terraform.

**Example Use Case:**
We need to migrate the `app.example.com` CNAME record from the parent hosted zone `example.com` to a new hosted zone `app.example.com`. This migration must be performed without any downtime to ensure uninterrupted service.

---

## Challenges
1. **CNAME Record Collision with NS Records:**
   - A CNAME record cannot coexist with NS records required to delegate a subdomain to another hosted zone. This collision makes it impossible to directly set up delegation without first modifying or removing the CNAME record.

2. **Infrastructure as Code (IAC) Latencies:**
   - Managing DNS records with Terraform or other IAC tools introduces latencies due to plan and apply phases. These latencies can result in short-lived DNS lookup failures during the migration process.

---

## Solution: Migrating with Zero Downtime
### Step 1: Delete the CNAME Record, Replace It with an A Record, and Remove It from Terraform State
- The existing CNAME record must be deleted first, as it will conflict with the creation of an A record for the same name.

**Example AWS CLI Command to Delete CNAME:**
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456ABCDEFG \
  --change-batch '{
    "Changes": [
      {
        "Action": "DELETE",
        "ResourceRecordSet": {
          "Name": "app.example.com",
          "Type": "CNAME",
          "TTL": 300,
          "ResourceRecords": [{"Value": "target.example.com"}]
        }
      }
    ]
  }'
```

**Example AWS CLI Command to Create A Record:**
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456ABCDEFG \
  --change-batch '{
    "Changes": [
      {
        "Action": "CREATE",
        "ResourceRecordSet": {
          "Name": "app.example.com",
          "Type": "A",
          "TTL": 300,
          "ResourceRecords": [{"Value": "203.0.113.10"}]
        }
      }
    ]
  }'
```
- Once the new A record is created, remove the old CNAME record from Terraform state to prevent future conflicts.

**Example Terraform State Removal Command:**
```bash
terraform state rm aws_route53_record.cname_record
```
- **Important Note:** Before removing the temporary A record in later steps, double-check DNS resolution to ensure queries are resolving correctly to the new hosted zone to avoid accidental disruptions.

- This ensures continuity of service while the migration takes place.
- Since the resources being replaced will eventually be deleted, there's no need to commit these changes to Terraform.

### Step 2: Create the New Hosted Zone in Terraform
- Define the new hosted zone in Terraform but **do not add NS records** in the parent hosted zone yet.

**Example Terraform Code:**
```hcl
resource "aws_route53_zone" "new_zone" {
  name = "app.example.com"
}

resource "aws_route53_record" "a_record" {
  zone_id = aws_route53_zone.new_zone.zone_id
  name    = "app.example.com"
  type    = "A"
  ttl     = 300
  records = ["203.0.113.10"]
}
```

### Step 3: Delegate the Domain
- After validating the new hosted zone and confirming that the A record exists, update the parent hosted zone to add NS records pointing to the new hosted zone.

**Example Terraform Code for NS Record in Parent Hosted Zone:**
```hcl
resource "aws_route53_record" "delegation" {
  zone_id = "Z123456ABCDEFG"  # Parent hosted zone ID
  name    = "app.example.com"
  type    = "NS"
  ttl     = 300
  records = aws_route53_zone.new_zone.name_servers
}
```

### Step 4: Validate the Migration
- Use `dig` or `nslookup` to confirm DNS queries are resolving through the new hosted zone.

**Example Dig Command:**
```bash
dig app.example.com @ns-123.awsdns-45.org
```

### Step 5: Cleanup Old A Record in Parent Hosted Zone
- Once migration is complete and queries are resolving correctly through the new hosted zone, delete the temporary A record in the parent hosted zone.

**Example AWS CLI Command:**
```bash
aws route53 change-resource-record-sets \
  --hosted-zone-id Z123456ABCDEFG \
  --change-batch '{
    "Changes": [
      {
        "Action": "DELETE",
        "ResourceRecordSet": {
          "Name": "app.example.com",
          "Type": "A",
          "TTL": 300,
          "ResourceRecords": [{"Value": "203.0.113.10"}]
        }
      }
    ]
  }'
```

---

## Why Does This Approach Avoid Downtime?
1. The existing CNAME is replaced with an A record, preserving DNS resolution.
2. DNS propagation for the NS records happens in parallel with the A record already present in the new hosted zone.
3. Once delegation is active, queries are routed to the new hosted zone where the A record is pre-configured, ensuring zero downtime.

---

## Final Thoughts
DNS migrations, especially in AWS Route 53, require careful planning to avoid service disruptions. By leveraging a hybrid approach—manual adjustments for time-sensitive changes and Terraform for persistent configurations—you can achieve zero-downtime migrations with minimal risk. 

This guide demonstrates a practical and scalable solution for managing DNS transitions without affecting availability, even in complex setups.
