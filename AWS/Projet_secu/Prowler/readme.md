# 🛡️ Automated Cloud Security Posture & Compliance Monitoring (AWS & Prowler)

[![AWS](https://img.shields.io/badge/AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Terraform](https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white)](https://www.terraform.io/)
[![Prowler](https://img.shields.io/badge/Prowler-Open_Source-blue?style=for-the-badge)](https://github.com/prowler-cloud/prowler)


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

## STEP 1: Activate AWS Security Hub
First, AWS Security Hub must be enabled in your region (e.g., eu-west-3 or us-east-1).

- Log into the AWS Management Console.

- Search for Security Hub in the top search bar.

- Click Go to Security Hub (or Enable Security Hub).

- Uncheck security standards like CIS AWS Foundations or PCI-DSS if you only want Prowler to manage findings for now (or leave them default if you want AWS native checks as well).

- Click Enable Security Hub.


## STEP 2: Enable Prowler Integration in Security Hub
Security Hub must be configured to accept findings pushed by Prowler.

- In the Security Hub console, go to the left navigation menu and click Integrations.

- Search for Prowler.

- Locate the Prowler card and click Accept findings (or Enable).


## STEP 3: Create IAM Role & Policy for Prowler
Prowler needs read-only permissions to audit your AWS infrastructure, plus permissions to import findings into Security Hub.

- Go to the IAM Console > Policies > Create Policy.

- Switch to the JSON tab and paste the following policy:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowProwlerSecurityHubImport",
            "Effect": "Allow",
            "Action": [
                "securityhub:BatchImportFindings",
                "securityhub:GetFindings",
                "securityhub:BatchUpdateFindings"
            ],
            "Resource": "*"
        }
    ]
}
```
- Name this policy: ProwlerSecurityHubImportPolicy.

- Now go to IAM > Roles > Create Role.

- Select Custom trust policy (or AWS Service > ECS Task if preparing for Fargate):
  For manual CLI testing (Step 4): You can attach this policy to your IAM User/Role currently in use.
  For Fargate (Step 6): Select Elastic Container Service > Elastic Container Service Task.
  Attach the following managed policies to the role:

- SecurityAudit (AWS Managed Policy - grants read-only access for Prowler auditing)

- ViewOnlyAccess (AWS Managed Policy - optional, adds extra read visibility)

- ProwlerSecurityHubImportPolicy (The custom policy you created above)

Name the role: ProwlerExecutionRole.

## STEP 4: Execute Prowler locally/CLI (Manual Test)
Let's do a fast manual execution using Docker on your local terminal or AWS CloudShell to verify the integration before setting up Fargate.

Run the Docker image using your AWS credentials (or assumed role):
```
docker run --rm -ti \
  -e AWS_ACCESS_KEY_ID="<YOUR_ACCESS_KEY>" \
  -e AWS_SECRET_ACCESS_KEY="<YOUR_SECRET_KEY>" \
  -e AWS_DEFAULT_REGION="eu-west-3" \
  toniblyx/prowler:latest aws --security-hub
```

## STEP 5: Verify Findings in Security Hub
- Return to the AWS Security Hub Console.

- In the left panel, click Findings.

- In the filter bar, filter by Company name = Prowler or Product name = Prowler.

- You should see Prowler's audit results appearing in the dashboard sorted by severity (CRITICAL, HIGH, MEDIUM, LOW).


## STEP 6: Automate Prowler Execution using ECS Fargate & EventBridge



## STEP 7: Final Verification & Clean-up / Transition to Remediation





























