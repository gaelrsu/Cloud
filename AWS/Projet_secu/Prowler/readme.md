# 🛡️ Automated Cloud Security Posture & Compliance Monitoring (AWS & Prowler)

[![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Prowler](https://img.shields.io/badge/Prowler-Open_Source-blue?style=for-the-badge)](https://github.com/prowler-cloud/prowler)
[![License](https://img.shields.io/badge/License-MIT-green.style=for-the-badge)](LICENSE)

An automated, serverless solution designed to continuously audit AWS environments, assess security posture against major compliance frameworks (CIS AWS Foundations, NIS 2, ISO 27001), and push security findings directly into **AWS Security Hub**.

---

## 🏗️ Architecture Overview

The infrastructure is 100% automated using **Terraform** and operates on a serverless, pay-as-you-go architecture to minimize operational costs.

```text
               ┌──────────────────────┐
               │  AWS EventBridge     │
               │  (Weekly Cron Trigger)│
               └──────────┬───────────┘
                          │
                          ▼
               ┌──────────────────────┐
               │   AWS CodeBuild      │ ◄── (Prowler Docker Container)
               │ (Execution Environment)│
               └──────────┬───────────┘
                          │
                          ├───────────────────────────────────┐
                          ▼                                   ▼
               ┌──────────────────────┐            ┌───────────────────┐
               │   AWS Security Hub   │            │   Amazon S3       │
               │  (Centralized Dashboard)│          │ (Raw JSON/CSV Logs)│
               └──────────┬───────────┘            └───────────────────┐
                          │
                          ▼
               ┌──────────────────────┐
               │ Amazon EventBridge / │
               │     SNS Notifications│ ──► [ Slack / Teams / Email ]
               └──────────────────────┘

```

### Workflow
- Trigger: An AWS EventBridge Rule initiates the pipeline on a scheduled basis (e.g., weekly or post-deployment).

- Execution: AWS CodeBuild pulls the official Prowler Docker container and executes the compliance scans against the AWS account using a dedicated least-privilege IAM Role.

- Ingestion: Findings are formatted into AWS Security Finding Format (ASFF) and published directly into AWS Security Hub.

- Archival & Alerting: Full raw reports (CSV/JSON/HTML) are archived in an encrypted S3 bucket, and critical alerts generate immediate SNS/Slack notifications.


### Features
- Continuous Compliance Auditing: Automated assessment against CIS Benchmarks, NIS 2, DORA, GDPR, and ISO 27001.

- Centralized Visibility: Seamless integration with AWS Security Hub for a single-pane-of-glass security dashboard.

- Serverless & Cost-Effective: Runs exclusively on-demand using AWS CodeBuild and EventBridge (~$0/month for standard accounts).

- Infrastructure as Code (IaC): Fully deployable within minutes using Terraform.

- Zero Persistent Credentials: Uses IAM Service Roles and temporary security credentials (STS).


### Getting Started
Prerequisites
- An active AWS Account with permissions to deploy IAM, Security Hub, EventBridge, and CodeBuild.

- Terraform >= 1.5.0 installed.

- AWS CLI configured locally.

### Installation
1. Clone the repository:
```
git clone [https://github.com/](https://github.com/)[your-username]/aws-prowler-securityhub-automation.git
cd aws-prowler-securityhub-automation
```
2. Enable AWS Security Hub Integration:

- Go to AWS Security Hub console > Integrations.

- Search for Prowler and click Accept findings.

3. Deploy with Terraform:
```
cd terraform/
terraform init
terraform plan
terraform apply
```
### Configuration & Customization
You can customize the compliance frameworks and scan schedules via terraform.tfvars:
```
Terraform
aws_region          = "eu-west-3"
compliance_framework = ["cis_1.5_aws", "nis2_aws"]
schedule_expression  = "cron(0 8 ? * MON *)" # Every Monday at 08:00 UTC
notification_email  = "security-alerts@yourcompany.com"
```
### Security Hub Dashboard & Results
Once the scan completes, security findings appear in AWS Security Hub categorized by severity (CRITICAL, HIGH, MEDIUM, LOW).
| Compliance Check | Status | Severity | Remediation Action |
| :--- | :--- | :--- | :--- |
| **S3.1** S3 Block Public Access | `FAILED` | **HIGH** | Enable S3 Account Level Block Public Access |
| **IAM.1** Root User MFA Enabled | `PASSED` | **CRITICAL** | N/A |
| **EC2.2** Unrestricted Security Group Ingress | `FAILED` | **HIGH** | Restrict port 22/389 ingress to VPN IPs |

### Security & Least Privilege
The IAM Role created for Prowler follows the principle of least privilege:

- Attached AWS Managed Policy: SecurityAudit and ViewOnlyAccess.

- Custom Inline Policy: Explicit permissions for securityhub:BatchImportFindings and S3 report delivery.





