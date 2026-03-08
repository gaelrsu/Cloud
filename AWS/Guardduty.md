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
UnauthorizedAccess:EC2/TorIPCaller


Important fields to review:

## Severity

Indicates the risk level of the finding.

Possible values:

- Low
- Medium
- High

## Resource

Identifies the AWS resource involved.

Example:


EC2 instance


## Source IP

Example:


185.220.x.x


GuardDuty may identify this IP as a **Tor exit node**.

## Description

Provides a short explanation of the activity detected.

Example:


EC2 instance communicated with a Tor exit node


---

# 5. Investigation Process

Key elements to review:

## Timeline

When the event occurred.

## Resource Details

Information about the affected resource:

- Instance ID
- VPC
- Subnet
- Security Group

## API Activity

GuardDuty may provide related API activity linked to the event.

---

# 6. Correlate with CloudTrail

Further investigation should be done using **CloudTrail**.

Navigate to:

CloudTrail  
→ **Event History**

Search for related activity:

- EC2 instance ID
- Source IP
- IAM user involved

Verify:

- who launched the instance
- who modified the security group
- any suspicious API calls

---

# 7. Incident Response Example

If the alert is confirmed as malicious activity, typical response steps may include:

1. Isolate the EC2 instance
2. Inspect running processes on the instance
3. Review IAM credentials and access keys
4. Block the malicious IP address if necessary
5. Analyze VPC Flow Logs for network activity

---

# 8. GuardDuty Limitations

GuardDuty is effective at detecting:

- Botnet communication
- TOR network usage
- Port scans
- Malware activity
- Credential abuse

However, it may not detect certain legitimate but risky actions, such as:

- IAM admin user creation
- Policy privilege escalation
- Internal data exfiltration

Additional tools are required for deeper detection capabilities:

- **CloudTrail**
- **Security Hub**
- **Custom detection rules**
