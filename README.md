
# Build a Security Monitoring System




---

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project!

In this project, I will demonstrate how to set up a security monitoring system in AWS using CloudTrail, CloudWatch, and SNS. I'm doing this project to learn how to securely store secrets, track who's accessing them, and get notified instantly when something suspicious is happening.

### Tools and concepts

Services I used throughout this project included Secrets Manager, CloudTrail, CloudWatch, SNS, and S3. 

### Project reflection

This project took me approximately 2 hours to complete. 

---

## Create a Secret

AWS Secrets Manager helps you protect secrets like passwords, API keys, credentials, and sensitive information. Instead of storing them in your code or sharing them via email, you can utilize Secrets Manager.

To set up for my project, I created a secret called TopSecretInfo in Secrets Manager. This secret containes a string as part of the key value pair. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

CloudTrail is monitoring service that documents every action taken on the AWS account. This continuous reporting is extremely valuable for security, troubleshooting, and meeting compliance requirements. 

CloudTrail events include types like Management events, Data events, Insight events, and Network activity events. API activities consist of read, write, exclude AWS KMS events, and exclude Amazon RDS Data API events.  

### Read vs Write Activity

Read API activity happens when someone views but doesn't change anything. Write API activity occurs when changes happen. 

---

## Verifying CloudTrail

I retrieved the secret in two ways: First through the Secrets Manager console by selecting "retrieve secret value." Second through the AWS Cloudshell using this code "aws secretsmanager get-secret-value --secret-id "TopSecretInfo" --region us-east-1."

To analyze my CloudTrail events, I visited the Event History section in CloudTrail where I found the exact logs for when I accessed my secret that I stored. This tells me that CloudTrail is properly logging everything that's going on throughout my AWS account. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

CloudWatch Logs is a service that helps you bring together your logs from different AWS services. It's important for monitoring because once your logs are in CloudWatch you're able to create alerts based on specific patterns. 

CloudTrail's Event History is useful for quick investigations while CloudWatch Logs are better for setting up alerts and automated responses when specific events take place. 

The metric value is what gets recorded when our filter spots a match in the logs. A default value is what gets recorded when our filter doesn't find any matches during a given time period. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

A CloudWatch alarm is a mechanism that monitors metrics and triggers actions when a certain threshold is met. I set my CloudWatch alarm threshold to 1 so the alarm will trigger when my secret has been accessed greater than or equal to 1 in a 5 minute period. 

I created an SNS topic along the way. An SNS topic is a service that's used to broadcast notifications. My SNS topic is set up to send me an email based on the metrics I just set in CloudWatch.

AWS requires email confirmation beacuse it would be a bit intrusive if it didn't ask. This helps prevent possible spam. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

To test my monitoring system, I retrieved the secret value again. The results were that I did not receive an email notification.

When troubleshooting the notification issues I double checked that CloudTrail was actually recording the events, I checked to see if CloudTrail was actually sending the events to CloudWatch, I checked if the CloudWatch metric filter was working properly, I checked if my CloudWatch alarm was triggering properly, and I made sure Amazon SNS was working.

I initially didn't receive an email before because the CloudWatch alarm wasn't triggering properly. The key to fixing this was to change the statistic from "Average" to "Sum"

---

## Success!

To validate that my monitoring system works I retrieved my secret value again and that put the alarm in an alarm state. I also received an email notification that my secret has been accessed. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

In a project extension, I updated the CloudTrail configurations to enable SNS delivery notifications. This allows me to get notified every time a new log file is delivered to the S3 bucket. Whereas with CloudWatch, I was only notified when a certain pattern is triggered. 

After enabling CloudTrail SNS notifications, my inbox started to receive notifications that my secret was being accessed. In terms of the usefulness of these emails, CloudWatch alarms would be better suited due to it being more targeted. CloudTrail SNS notifications would be perfect if sent to a security monitoring system within a production environment. 

![Image](http://learn.nextwork.org/merry_azure_brave_bicho_papao/uploads/aws-security-monitoring_d7e8f9g0)

---

---
