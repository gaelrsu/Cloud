# WAF

## Intro

AWS WAF, or Amazon Web Services Web Application Firewall, is a firewall service that helps protect web applications from common web exploits and attacks.
It allows you to control which traffic to allow or block to your web applications by defining customizable web security rules.
These rules are designed to filter out malicious traffic, such as SQL injection and Cross-Site Scripting (XSS) attacks,
before it reaches your web applications running on AWS resources, such as Amazon EC2 instances or Amazon API Gateway.

Used to : 
Protection Against Common Web Exploits
Mitigating DDoS Attacks
Compliance and Data Protection
Flexible and Customizable Rules
Integration with Other AWS Services Ex : AWS WAF with Amazon CloudFront
Real-time Monitoring and Insights
Cost-effective Security Solution

## Web ACLs
Designed to protect your web applications from various online threats. 
By creating a Web ACL, you can define specific rules and conditions about IP addresses, geolocation, or specific patterns within the incoming requests to allow or block incoming traffic to your web application.
```
On WAF & Shield, go to Web ACLs -> select 'Create a new Web ACL'
Specify a Name for the web ACL to identify it.
```
Rule sets are predefined collections of rules provided by AWS WAF, simplifying the process of setting up security rules for your web applications. 
These rule sets are created and maintained by AWS security experts and cover common vulnerabilities and threats, such as SQL injection and XSS attacks.
Choose a Managed Rule Set
```
In the AWS WAF console, go to the Web ACLs section.
Create a new Web ACL or edit an existing one.
In the Web ACL editor, add a new rule group. You can choose either a regular rule group or a managed rule group.
Select "Managed rule group" and choose from the available rule sets provided by AWS. These rule sets cover various types of threats such as SQL injection, XSS attacks, and more.
# For example, if you're concerned about SQL injection attacks, choose the “SQL Injection” managed rule set.
```
Conditions : Conditions are the building blocks of rules that define the characteristics of web requests you want to match. These can include IP addresses, strings in the request, request headers, query strings, and more. For example, you can create a condition to match requests coming from a specific IP address range.
Rules : Rules are composed of one or more conditions. You define rules based on these conditions to specify what action AWS WAF should take when a web request matches the defined conditions. For instance, you could create a rule that blocks all requests containing a specific SQL injection pattern in the query string.

## Creating an AWS WAF ACL
```
Access AWS WAF Console : Go to the AWS WAF console by visiting https://console.aws.amazon.com/wafv2/ .
Create a Web ACL : In the AWS WAF console, go to Web ACLs in the navigation pane, and select "Create web ACL."
Specify ACL Details : Enter a unique and descriptive name for your Web ACL. This name will help you identify and manage the ACL easily.
Configure Rules : Define rules for your Web ACL based on your security requirements. You can create rules to block specific IP addresses, filter requests based on geolocation, or detect patterns indicative of attacks.
Add Conditions : Set conditions for the rules, specifying when they should be triggered. Conditions can be based on various factors such as IP addresses, query strings, or request headers.
```

## Custom Rules

```
Go to AWS WAF & Shield.
Choose the web ACL where you want to add the custom rule.

Select "Rules" from the navigation pane.
Click on "Create rule."
Define your conditions. For instance, you can base rules on specific query parameters or request headers associated with your application.
Specify actions for requests that match the conditions. Common actions include allowing, blocking, or counting requests.
```
Rate-based rules are essential for mitigating brute-force attacks or limiting requests from specific IP addresses. Here’s how to set up rate-based rules:
```
Within the AWS WAF console, go to the web ACL you want to configure.
Select "Rate-based rules" from the left panel.
Create a new rate-based rule.
Define the rate limit (requests per minute) and the conditions that trigger the rule.
Choose the action to be taken when the rate limit is exceeded, such as blocking the requests.
```

## Regex and IP Set Matching
block SQL injection attempts, you can use a regular expression to match SQL keywords.
```
While creating a custom rule, select " Regex pattern set " as the type of condition.
Define your regular expression pattern. For SQL injection, it could be something like: `.*(SELECT|INSERT|UPDATE|DELETE).*`.
Choose the action to be taken when the pattern is matched (usually block or count).
## Working with IP Sets to Block or Allow Specific IP Addresses
```
```
In the AWS WAF console, go to IP Sets.
Create a new IP set.
Define the IP addresses or ranges you want to include in the set.
Use this IP set in your web ACL rules to either allow or block traffic from these addresses.
```
