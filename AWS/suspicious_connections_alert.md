# suspicious_connections_alert

## Activate AWS CloudTrail to track activity
### Activate CloudTrail
```
Go to the AWS console and search for CloudTrail.
Click on Create Trail.
Trail name: security-trail.
Log storage :
Select Create a new S3 bucket (if you don't already have one).
Give it a unique name (my-cloudtrail-logs).
```
### Log configuration
```
Check Enable for all regions (important for multi-region security).
Active Log file validation (to check that logs have not been modified).
Active Server-side encryption to secure logs.
Create trail.
```
## Create an alert for suspicious connections
### Create an SNS subject to receive alerts
```
Go to Simple Notification Service (SNS).
Click on Create topic.
Name: security-alerts.
Type: Standard.
Create topic.
```
### Add your email address to receive alerts
```
In SNS, go to Subscriptions and click on Create subscription.
Topic ARN : Select security-alerts.
Protocol: Email.
Endpoint : Enter your email address.
Create the subscription, then validate the email received.
```
### CloudWatch Alarm → SNS Topic → Lambda → Discord Webhook
```
1. Create a Discord Webhook
text
Go to your Discord server settings → Integrations → Webhooks → New Webhook
Name: AWS-Alerts (or whatever you prefer)
Select the channel where you want alerts
Click "Copy Webhook URL" and save it for later

2. Create a New SNS Topic for Discord
text
Go to Simple Notification Service (SNS) → Create topic
Name: discord-alerts (or security-alerts-discord)
Type: Standard
Create topic

3. Create the Lambda Function (Bridge to Discord)
3.1 Create the function
text
Go to AWS Lambda → Create function
Name: cloudwatch-to-discord
Runtime: Python 3.12
Permissions: Create new role with basic permissions
Create function
3.2 Add the code
In the code editor past the code from the file "LAMBDA SNS discord"
3.3 Configure environment variable
text
In Lambda → Configuration → Environment variables → Edit
Add variable:
  Key: DISCORD_WEBHOOK_URL
  Value: [Paste your Discord webhook URL here]
Save
3.4 Adjust timeout
text
In Configuration → General configuration → Edit
Timeout: 10 seconds
Save
4. Subscribe the Lambda to the SNS Topic
text
Go back to SNS → Topics → discord-alerts → Subscriptions
Click Create subscription
Protocol: AWS Lambda
Endpoint: Select your cloudwatch-to-discord function
Create subscription
The status should become "Confirmed" after a few moments.

5. Connect Your Alarm to the Discord SNS Topic
text
Go to CloudWatch → Alarms → [Your Alarm] → Edit
In "Configure actions" section:
  - Alarm state trigger: In alarm
  - Send notification to: Select your "discord-alerts" SNS topic
Update alarm
6. Test the Integration
Manual test (without waiting for a real alarm)
text
Go to SNS → Topics → discord-alerts → Publish message
Use this test message:

{
  "AlarmName": "Test Discord",
  "NewStateValue": "ALARM", 
  "NewStateReason": "This is a manual test"
}

Publish message → Check Discord!

Real test
text
Generate 5 failed login attempts
Wait 5-10 minutes
Check Discord → You should receive the alert

```
### Create a CloudWatch alarm for suspicious connections
```
Go to CloudWatch -> Alarms.
Click on Create Alarm.
Select CloudTrail Metrics and choose Signin Failures.
Set an alert threshold (e.g. more than 5 failures in 5 minutes).
In Actions, select SNS topic and choose security-alerts.
Create the alarm.
```

## Checking CloudTrail logs in CloudWatch
```
First you need to make sure that CloudTrail is sending its logs to CloudWatch Logs :
Go to the AWS console and open CloudTrail
Go to the Trails tab, check that your Trail is active
In the CloudWatch Logs column, check that the logs are being sent (if not, you'll need to activate this option).
```
## Access CloudTrail logs in CloudWatch Logs
```
Go to the AWS console and search for CloudWatch
In the left-hand menu, click on Logs > Log groups
Search for a Log Group named /aws/cloudtrail/... (or similar)
Click on it, then go to “Logs Insights”.
```
## Run a query to view connections and errors
In the CloudWatch Logs Insights query editor, copy and paste this query:
```
fields @timestamp, eventName, userIdentity.arn, sourceIPAddress, responseElements.ConsoleLogin
| filter eventName="ConsoleLogin"
| sort @timestamp desc
| limit 20
```
Explanation :
Filter on eventName=“ConsoleLogin”.
Display user, source IP and result
Display last 20 events
## Create an alarm to detect connection failures
```
If you want to be alerted when there are multiple connection errors:
Go back to CloudWatch > Alarms
Click on Create Alarm
Select the log source (CloudTrail log group)
Configure a rule:
Statistic: Sum
Condition: > X errors in Y minutes
Action: Notify via SNS (e-mail, SMS, Lambda, etc.)
```
## 
```

```
