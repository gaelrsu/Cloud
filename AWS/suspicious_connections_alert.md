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
