# AWS Route 53 + CloudFront Static Website Project

## Domain: atulkamble.in

## Region: us-east-1

This project demonstrates how to host a highly available static website using:

* Amazon S3
* Amazon CloudFront
* Amazon Route 53
* Custom Domain (atulkamble.in)
* SSL Certificate (HTTPS)

---

# Architecture

```text
User
  │
  ▼
https://atulkamble.in
  │
  ▼
Route 53
  │
  ▼
CloudFront Distribution
  │
  ▼
S3 Bucket
(atulkamble.in)
  │
  ▼
index.html
```

---

# Project Objectives

✅ Purchase/Register Domain

✅ Configure Route 53 Hosted Zone

✅ Create S3 Static Website

✅ Upload Website Files

✅ Create SSL Certificate

✅ Configure CloudFront

✅ Configure Route53 Alias Record

✅ Enable HTTPS

---

# Prerequisites

You already have:

| Resource    | Value         |
| ----------- | ------------- |
| Domain      | atulkamble.in |
| Hosted Zone | Created       |
| Region      | us-east-1     |
| AWS CLI     | Installed     |
| IAM User    | Configured    |

---

# Step 1: Verify Route 53 Hosted Zone

AWS Console:

```text
Route53
 → Hosted Zones
 → atulkamble.in
```

You should see:

```text
NS Record
SOA Record
```

Current Hosted Zone:

```text
Hosted Zone ID:
Z05295699NFYIN7UL7G2
```

---

# Step 2: Create S3 Bucket

Bucket name MUST match domain.

Bucket:

```text
atulkamble.in
```

### AWS CLI

```bash
aws s3api create-bucket \
--bucket atulkamble.in \
--region us-east-1
```

Verify:

```bash
aws s3 ls
```

Expected:

```text
atulkamble.in
```

---

# Step 3: Create Website Files

Create project directory.

```bash
mkdir route53-cloudfront-project
cd route53-cloudfront-project
```

Create HTML page.

```bash
nano index.html
```

Example:

```html
<!DOCTYPE html>
<html>
<head>
<title>Atul Kamble</title>
</head>
<body>
<h1>Welcome to AtulKamble.in</h1>
<h2>Hosted on AWS S3 + CloudFront</h2>
</body>
</html>
```

---

# Step 4: Upload Files to S3

```bash
aws s3 cp index.html s3://atulkamble.in/
```

Verify:

```bash
aws s3 ls s3://atulkamble.in/
```

---

# Step 5: Enable Static Website Hosting

Console:

```text
S3
 → atulkamble.in
 → Properties
 → Static Website Hosting
```

Enable:

```text
Enable
Index document:
index.html
```

Website Endpoint Example:

```text
http://atulkamble.in.s3-website-us-east-1.amazonaws.com
```

---

# Step 6: Bucket Policy

Bucket Permissions

```json
{
  "Version":"2012-10-17",
  "Statement":[
    {
      "Sid":"PublicRead",
      "Effect":"Allow",
      "Principal":"*",
      "Action":"s3:GetObject",
      "Resource":"arn:aws:s3:::atulkamble.in/*"
    }
  ]
}
```

Apply:

```bash
aws s3api put-bucket-policy \
--bucket atulkamble.in \
--policy file://policy.json
```

---

# Step 7: Create SSL Certificate

Go to:

```text
AWS Certificate Manager
```

Important:

```text
Region = us-east-1
```

Request certificate:

```text
atulkamble.in
*.atulkamble.in
```

Validation:

```text
DNS Validation
```

Create record automatically in Route53.

Wait:

```text
Status = Issued
```

---

# Step 8: Create CloudFront Distribution

Console:

```text
CloudFront
→ Create Distribution
```

Origin:

```text
S3 Bucket:
atulkamble.in
```

Viewer Protocol:

```text
Redirect HTTP to HTTPS
```

Alternate Domain Names:

```text
atulkamble.in
www.atulkamble.in
```

Certificate:

```text
Select ACM Certificate
```

Default Root Object:

```text
index.html
```

Create Distribution.

---

# Step 9: Copy CloudFront Domain

Example:

```text
d123abc456xyz.cloudfront.net
```

Keep this value.

---

# Step 10: Configure Route53 Alias Record

Hosted Zone:

```text
atulkamble.in
```

Create Record:

```text
Record Name:
(blank)

Type:
A

Alias:
Yes

Alias Target:
CloudFront Distribution
```

Create WWW Record:

```text
www

Type:
A

Alias:
Yes

Target:
CloudFront
```

---

# Step 11: Verify DNS

```bash
nslookup atulkamble.in
```

or

```bash
dig atulkamble.in
```

Expected:

```text
CloudFront DNS
```

---

# Step 12: Test Website

Open:

```text
https://atulkamble.in
```

Expected:

```text
Welcome to AtulKamble.in
Hosted on AWS S3 + CloudFront
```

---

# AWS CLI Verification Commands

### Bucket

```bash
aws s3 ls
```

### Files

```bash
aws s3 ls s3://atulkamble.in/
```

### Bucket Website

```bash
aws s3api get-bucket-website \
--bucket atulkamble.in
```

### Route53 Hosted Zone

```bash
aws route53 list-hosted-zones
```

### Route53 Records

```bash
aws route53 list-resource-record-sets \
--hosted-zone-id Z05295699NFYIN7UL7G2
```

### ACM Certificates

```bash
aws acm list-certificates \
--region us-east-1
```

### CloudFront Distributions

```bash
aws cloudfront list-distributions
```

---

# CloudFront Cache Invalidation

After updating website files:

```bash
aws cloudfront create-invalidation \
--distribution-id DISTRIBUTION_ID \
--paths "/*"
```

Example:

```bash
aws cloudfront create-invalidation \
--distribution-id E123ABC456XYZ \
--paths "/*"
```

---

# Project Deliverables

| Service                 | Purpose                 |
| ----------------------- | ----------------------- |
| Amazon S3               | Store Website Files     |
| Amazon CloudFront       | Global Content Delivery |
| Amazon Route 53         | DNS Management          |
| AWS Certificate Manager | SSL Certificates        |
| Domain                  | atulkamble.in           |
| Protocol                | HTTPS                   |
| Region                  | us-east-1               |

---

# Interview Questions

### Why bucket name should match domain?

For direct website hosting and easier Route53 integration.

### Why ACM certificate must be in us-east-1?

CloudFront only uses ACM certificates from us-east-1.

### Why use CloudFront?

* HTTPS support
* Global caching
* Lower latency
* DDoS protection
* Better performance

### Why Route53 Alias instead of CNAME?

* Works on root domain
* No DNS query charges
* AWS native integration

---

# Real-World Use Cases

### Personal Portfolio

```text
atulkamble.in
```

### Company Website

```text
cloudnautic.in
```

### Documentation Portal

```text
docs.company.com
```

### Training Platform

```text
learn.atulkamble.in
```

### Resume Hosting

```text
resume.atulkamble.in
```

### Static React Application

```text
React App
 → Build
 → S3
 → CloudFront
 → Route53
```

This is one of the most common AWS Architect and DevOps interview projects because it combines DNS, CDN, Object Storage, SSL, and website hosting into a complete production-ready solution.
