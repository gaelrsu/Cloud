# CloudWatch

1. Proactive Monitoring: CloudWatch enables you to monitor the performance of your AWS resources and applications in real-time, so you can identify and address issues before they impact your users.
2. Customizable Alarms: You can set up alarms to automatically notify you when certain thresholds are breached, allowing you to take immediate action to resolve issues.
3. Centralized Logging: CloudWatch Logs allow you to collect and store log data from your applications and AWS resources in one place, making it easier to troubleshoot and analyze issues.
4. Dashboards: With CloudWatch Dashboards, you can create custom visualizations of your metrics, providing a clear and intuitive way to monitor the health of your AWS environment.
5. Event-Driven Insights: CloudWatch Events and EventBridge allow you to respond to changes in your AWS environment or application state by triggering actions in real-time.
6. Cost Optimization: CloudWatch helps you understand resource utilization and identify opportunities for cost optimization by analyzing historical data and trends.

AWS provides a wide range of pre-defined metrics for its services, such as CPU utilization for EC2 instances or request count for an S3 bucket.
Can also create custom metrics to monitor specific aspects of your applications or resources.
can set up an alarm to notify you when the CPU utilization of an EC2 instance exceeds a specified threshold. 
Alarms can trigger actions like sending notifications via Amazon SNS, stopping or terminating instances, or invoking AWS Lambda functions.

## Collect
Metrics are collected at regular intervals, typically every minute, and sent to CloudWatch. AWS services and resources automatically send predefined metrics to CloudWatch.
To monitor specific aspects of your applications or resources, you can create custom metrics. This involves using the CloudWatch API or AWS SDKs to publish metric data. 
Custom metrics are essential for tracking application-specific performance indicators or business KPIs.
Ex of Custom Metric Creation :
```
1. Install AWS SDK: If you want to create custom metrics, first ensure that you have the AWS SDK or CLI installed, which allows you to interact with CloudWatch programmatically.
2. Publish Custom Metrics: Use the SDK or CLI to publish custom metric data to CloudWatch. You'll need to specify the metric's name, namespace, and dimensions (if applicable).
3. Set Up Metric Filters: To enable CloudWatch to process and analyze log data, set up metric filters within CloudWatch Logs. Metric filters extract data and convert it into custom metrics.
4. Create a Custom Dashboard: Once you have your custom metrics, create a custom dashboard to visualize and monitor these metrics. Dashboards allow you to create custom views of your data.
```

## CloudWatch Alarms
- Thresholds: Alarms are set based on metric thresholds.
define a threshold value, and when the metric crosses that value (e.g., CPU utilization exceeding 90% for five minutes), the alarm triggers.
- Actions: When an alarm is triggered, you can configure actions to take.
These actions can include sending notifications via Amazon SNS, stopping or terminating EC2 instances, invoking AWS Lambda functions, or running Auto Scaling policies.
Creating and Configuring Alarms :
```
1. Select a Metric: Choose the metric you want to monitor with an alarm. This could be a predefined AWS metric or a custom metric you've created.
2. Define the Threshold: Set the threshold value for the metric that will trigger the alarm.
3. Specify Actions: Define the actions to be taken when the alarm is triggered. You can set up notification actions to alert your team or automate responses.
4. Set Up Additional Alarm Configuration: Configure optional settings, such as the alarm's name, description, and evaluation period.
5. Create the Alarm: Once you've configured the alarm, create it. The alarm will start monitoring the metric immediately.
```

## CloudWatch Dashboards
Creating Dashboards :
```
1. Access the CloudWatch Console: Log in to the AWS Management Console and navigate to CloudWatch.
2. Create a New Dashboard: In the CloudWatch console, select "Dashboards" and then click "Create Dashboard."
3. Add Widgets: Customize your dashboard by adding widgets. Widgets can display metrics, alarms, or text/Markdown for documentation.
4. Configure Widgets: Each widget can be configured to display specific metrics or logs. You can adjust the time range and customize the widget's appearance.
5. Save the Dashboard: Give your dashboard a name and save it. You can also share dashboards with other AWS accounts or make them public.
```

## CloudWatch Logs
How it Works
Log Groups: Log data is organized into "log groups," which represent a logical grouping of related log streams. For example, you might have a log group for your application's access logs.
Log Streams: Log streams within a log group are individual sources of log data. Each log stream can represent a different source or instance, such as an EC2 instance or Lambda function.
Creating Groups and Log Streams :
```
1. Create a Log Group: In the CloudWatch console, navigate to "Log groups" and create a new log group. Give it a name that reflects the type of logs you'll be collecting.
2. Configure Log Streams: Within a log group, you can configure log streams. Log streams are typically automatically created as new log data is ingested. However, you can also create custom log streams if needed.
3. Publish Log Data: Use the CloudWatch Logs SDK or AWS CLI to publish log data to the appropriate log stream. Your applications and AWS resources can send log data to CloudWatch Logs.
```
### Log Retention Policies
CloudWatch Logs allows you to define retention policies for log data. This determines how long log data is retained. You can specify retention periods ranging from a few days to indefinitely.
Log data is a valuable source of information for troubleshooting, security analysis, and compliance. By centralizing logs in CloudWatch, you can easily access and analyze this data.

## Events and EventBridge in CloudWatch
CloudWatch Events and EventBridge are event-driven monitoring and automation tools that allow you to respond to changes and events within your AWS environment in real-time.
Events: Events are generated by AWS services, custom applications, or third-party sources. These events can signify changes in your AWS environment, such as instance launches, S3 bucket object creations, or CloudTrail log events.
Event Rules: CloudWatch Events uses "rules" to match specific events based on their attributes. You can define rules to capture events that are relevant to your use case.
Targets: When an event matches a rule, you can specify "targets" to define what action should be taken. Targets can include AWS Lambda.

## Query Structure
Selecting field
```
fields @timestamp, @message, @fieldName
```
Filtering Data
```
filter @logStream = 'your-log-stream-name'
filter @message like 'error'
```
Sorting Data
```
sort @timestamp desc
```
Limiting Results
```
| limit 30
```
Regular Expressions
for exemple to match log fields
```
fields @timestamp, @message
| filter @message like 'error'
| sort @timestamp desc
| limit 10
```
Mathematical and Comparative Operations
```
fields @timestamp, @responseTime
| stats avg(responseTime) as avgResponseTime
| filter @responseTime > avgResponseTime * 2
```
Exemple of Security Incident Detection
```
fields @timestamp, @message
| filter @message like 'Failed login attempt'
| sort @timestamp desc
| limit 10
```
```
fields @timestamp, @message
| filter @message like 
/.*(SELECT|UNION|INSERT|UPDATE|DELETE|FROM|WHERE|DROP|AND|OR).*/
```
Exemple of Detect failed login
```
fields @timestamp, eventName, userIdentity.arn, sourceIPAddress, responseElements.ConsoleLogin
| filter eventName="ConsoleLogin"
| filter responseElements.ConsoleLogin="Failure"
| sort @timestamp desc
| limit 20
```


