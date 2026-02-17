# AWS IAM Secure Multi-Role Architecture (HashmiTech)

## 📌 Project Overview

This project demonstrates the implementation of a secure AWS multi-role IAM architecture using best practices:

- Least Privilege Access
- MFA Enforcement
- Group-Based Permission Design
- Tag-Based EC2 Control
- S3 Access Restriction
- CloudTrail Audit Monitoring

The goal is to simulate a real-world secure AWS account suitable for production-grade governance.

---

## 🏗 Architecture Diagram

![Architecture](architecture.png)

---

# 🔐 Phase 1 – Root Account Hardening

## 1️⃣ Root MFA Enabled

To secure the AWS account, MFA was enabled on the root user.

![Root MFA Enabled](screenshots/01-root-mfa-enabled.png)

---

## 2️⃣ Strong Password Policy

Configured IAM password policy with:

- Minimum 12 characters
- Uppercase & lowercase required
- Numbers required
- Symbols required

![Password Policy](screenshots/02-password-policy.png)

---

# 👤 Phase 2 – Administrative Separation

## 3️⃣ Created IAM Admin User

Created `admin1` with `AdministratorAccess` policy to avoid using root account for operations.

![Admin User Created](screenshots/03-admin-user-created.png)

---

# 👥 Phase 3 – IAM Role Design

## 4️⃣ Created IAM Groups

- Developers
- Interns
- Auditors

![Groups Created](screenshots/04-groups-created.png)

---

## 5️⃣ Created Custom Policies

- Hashmi-EnforceMFA
- Hashmi-Developers
- Hashmi-Interns
- Hashmi-Auditors

![Policies Created](screenshots/05-policies-created.png)

---

## 6️⃣ Attached Policies to Groups

### Auditors Group

![Auditors Policy Attached](screenshots/06-policies-attached-to-AuditorsGroup.png)

### Developers Group

![Developers Policy Attached](screenshots/07-policies-attached-to-DevelopersGroup.png)

### Interns Group

![Interns Policy Attached](screenshots/08-policies-attached-to-InternGroup.png)

---

## 7️⃣ Created Users and Assigned to Groups

Users created:

- dev1, dev2 → Developers
- intern1 → Interns
- auditor1 → Auditors

![Users Created](screenshots/09-users-created.PNG)

---

# 🗂 Phase 4 – S3 Access Control

## 8️⃣ Created S3 Bucket

Bucket: `hashmit-bucket-dev`

![S3 Bucket Created](screenshots/10-s3-bucket-created.png)

---

## 9️⃣ Created Secure Intern Folder

Folder: `intern-uploads/`

![Intern Folder](screenshots/11-intern-folder-created.png)

---

## 🔒 S3 Permission Testing

### Developer IAM Access Denied

Developers cannot access IAM dashboard.

![Dev IAM Denied](screenshots/12-dev-iam-denied.png)

---

### Intern Upload Success

Intern can upload files inside allowed folder.

![Intern Upload](screenshots/13-intern-upload-successful.png)

---

### Intern Delete Denied

Intern cannot delete objects (Explicit Deny in policy).

![Intern Delete Denied](screenshots/14-intern-delete-denied.png)

---

# 🖥 Phase 5 – EC2 Tag-Based Control

## 10️⃣ Tagged EC2 Instance

Key: `Environment`  
Value: `Dev`

![EC2 Tag](screenshots/15-ec2-tag-dev.png)

---

## 11️⃣ Developer Start/Stop Allowed (Only Dev Tagged)

![Dev EC2 Control](screenshots/16-dev-ec2-startstop-allowed.png)

---

# 📊 Phase 6 – CloudTrail Monitoring

## 12️⃣ CloudTrail Enabled (Admin)

Trail: `hashmi-trail`  
Management Events: ON

![CloudTrail Enabled](screenshots/17-cloudtrail-enabled.png)

---

## 13️⃣ Auditor Event Lookup

Auditor can view CloudTrail logs but cannot modify resources.

![Auditor CloudTrail](screenshots/18-auditor-cloudtrail-lookup.png)

---

# 🧠 Architecture Decisions

## Why Group-Based Instead of Direct User Policies?

Group-based access ensures scalable permission management. Users inherit permissions automatically, reducing administrative complexity.

---

## Why Explicit Deny?

Explicit Deny overrides all Allow rules and prevents accidental privilege escalation (e.g., interns deleting data).

---

## Why Tag-Based EC2 Access?

Developers can only operate on EC2 instances tagged `Environment=Dev`, protecting production workloads.

---

## Why MFA Enforcement Using Condition?

If MFA is not present, access is denied except for MFA setup. This ensures strong authentication protection.

---

# 🔐 Administrative Model

Root account used only for:

- Initial MFA setup
- Password policy configuration
- Creating IAM admin user

All operational tasks were performed using IAM admin account.

---

# 📚 Key Learning Outcomes

- IAM Policy Structure & Evaluation Logic
- Condition-Based Access Control
- Resource-Level Permission Enforcement
- AWS Console Permission Dependencies
- CloudTrail Audit Monitoring
- Enterprise-Level IAM Design Principles

---

# 🚀 Future Improvements

- Implement Permission Boundaries
- Convert IAM structure to Terraform (IaC)
- Implement AWS Config compliance checks
- Add Service Control Policies (SCP) for multi-account setup
- Implement CI/CD for policy validation

---

# 🧹 Cleanup Steps

1. Delete CloudTrail trail and log bucket
2. Terminate EC2 instance
3. Delete S3 bucket contents
4. Remove IAM users and groups

---

## 👨‍💻 Author

Hamzah Zubair Hashmi  
Aspiring AWS Solutions Architect  
Manchester, UK
