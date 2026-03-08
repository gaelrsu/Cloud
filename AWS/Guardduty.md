# AWS GuardDuty Lab

## Objective

The goal of this lab is to understand how **AWS GuardDuty** detects suspicious activity and how a cloud security analyst can investigate findings.

This lab covers:

- Enabling GuardDuty
- Generating sample findings
- Understanding GuardDuty alerts
- Investigating alerts using CloudTrail
- Understanding GuardDuty limitations

---

# 1. Verify GuardDuty is Enabled

Navigate to:

Security → GuardDuty

Verify the following:

- GuardDuty status: **Enabled**
- Region: **us-east-1** (or the region used in the lab)

In **Settings**, leave the default protections enabled:

- S3 Protection
- EKS Protection
- Malware Protection (if available)

For this lab, the **default configuration is sufficient**.

---

# 2. Generate Sample Findings

AWS provides the ability to generate **simulated GuardDuty findings** for testing and training.

Navigate to:

Settings  
→ **Generate sample findings**

Click the button to generate findings.

GuardDuty will automatically create several simulated alerts.

---

# 3. Explore the Findings

Navigate to:

GuardDuty  
→ **Findings**

You should see alerts such as:

- `Recon:EC2/Portscan`
- `UnauthorizedAccess:EC2/TorIPCaller`
- `Trojan:EC2/BlackholeTraffic`
- `CryptoCurrency:EC2/BitcoinTool`

Select one finding to inspect its details.

---

# 4. Understanding a Finding

Example finding:
