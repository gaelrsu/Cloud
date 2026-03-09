# Send alert on discord

## 1. Create a Discord Webhook
text
Go to your Discord server settings → Integrations → Webhooks → New Webhook
Name: AWS-Alerts (or whatever you prefer)
Select the channel where you want alerts
Click "Copy Webhook URL" and save it for later

## 2. Create a New SNS Topic for Discord
text
Go to Simple Notification Service (SNS) → Create topic
Name: discord-alerts (or security-alerts-discord)
Type: Standard
Create topic

## 3. Create the Lambda Function (Bridge to Discord)
### 3.1 Create the function
text
Go to AWS Lambda → Create function
Name: cloudwatch-to-discord
Runtime: Python 3.12
Permissions: Create new role with basic permissions
Create function
### 3.2 Add the code
In the code editor past the code below
### 3.3 Configure environment variable
text
In Lambda → Configuration → Environment variables → Edit
Add variable:
  Key: DISCORD_WEBHOOK_URL
  Value: [Paste your Discord webhook URL here]
Save
### 3.4 Adjust timeout
text
In Configuration → General configuration → Edit
Timeout: 10 seconds
Save
## 4. Subscribe the Lambda to the SNS Topic
text
Go back to SNS → Topics → discord-alerts → Subscriptions
Click Create subscription
Protocol: AWS Lambda
Endpoint: Select your cloudwatch-to-discord function
Create subscription
The status should become "Confirmed" after a few moments.

## 5. Connect Your Alarm to the Discord SNS Topic
text
Go to CloudWatch → Alarms → [Your Alarm] → Edit
In "Configure actions" section:
  - Alarm state trigger: In alarm
  - Send notification to: Select your "discord-alerts" SNS topic
Update alarm
## 6. Test the Integration
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


| Issue | Solution |
|-------|----------|
| No message on Discord | Check the webhook URL in Lambda environment variables (`DISCORD_WEBHOOK_URL`) |
| Alarm stays in OK state | Make sure the statistic is set to **Sum** (not Average/Minimum) in your alarm configuration |
| Manual test works, but not the real alarm | Verify the alarm is using the CORRECT SNS topic (`discord-alerts`) in its actions |
| Lambda times out | Increase timeout to at least **10 seconds** in Lambda configuration |
| SNS subscription shows "Pending" | Check that your Lambda role has the `sns:Subscribe` permission |
| No logs in CloudWatch | Verify Lambda has `AWSLambdaBasicExecutionRole` policy attached |
| Discord returns 401/403/404 | The webhook URL is invalid - create a new one in Discord |
| Graph shows spikes but no alarm | Change "Datapoints to alarm" from `5 out of 5` to **`1 out of 5`** |
| Metric shows 1 instead of 5 failures | Change statistic from Average/Minimum to **Sum** |
| Email works but Discord doesn't | The Lambda function or SNS subscription is the problem - check CloudWatch logs |



# Lambda code :
```
import json
import urllib3
import os

http = urllib3.PoolManager()

def lambda_handler(event, context):
    # 1. Récupérer le message de l'alarme
    sns_message = event['Records'][0]['Sns']['Message']
    alarm = json.loads(sns_message)
    
    # 2. Construire un message
    texte = f"**{alarm['AlarmName']}** est en état **{alarm['NewStateValue']}**\n{alarm['NewStateReason']}"
    
    # 3. Envoyer à Discord
    webhook_url = os.environ['DISCORD_WEBHOOK_URL']
    data = {"content": texte}
    
    http.request(
        'POST',
        webhook_url,
        body=json.dumps(data),
        headers={'Content-Type': 'application/json'}
    )
    
    return {"status": "ok"}
```
