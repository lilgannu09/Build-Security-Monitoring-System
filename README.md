# Build a Security Monitoring System

**Author:** Ganesh Mahankali  
**Email:** ganeshmahankali9@gmail.com

---

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_reghtjy)

---

## Introducing Today's Project

I'm doing this project to learn how to use AWS Cloud Trail, SNS, and CloudWatch. Attempting to use these tools to learn how AWS Security tools work, which sends us emails as alerts.

### Tools and concepts

Services I used were CloudTrail, CloudWatch, and SNS. I also used Secrets Manager IAM (roles) and S3 Buckets. Key concepts I learnt include secret storing, CloudWatch vs CloudTrail, what notifications are, different types of endpoints, and how to create a CloudWatch alarm.

### Project reflection

This project took me approximately 5 hours, including demo and yarn time. The most time-consuming aspect of this was exposure to the various AWS tools. The most challenging part was troubleshooting why the email was not delivering in our first test, as in the future, for any sort of monitoring system in real-world applications, it's hard if there are no "error logs" when an error occurs. It was most rewarding to compare CloudTrail to CloudWatch and most importantly, enable SNS for every time an event occurred in Secrets Manager. 

---

## Create a Secret

Secrets Manager is AWS's security service to store Database credentials, API Keys, account IDs, or anything that could potentially cause trouble if leaked. 

To set up for my project, I created a secret called "TopSecretInfo which contains details of the Monitoring System which contained "I will get an Internship" as the secret, and to alert if it is accessed. 

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_o5p6q7r8)

---

## Set Up CloudTrail

CloudTrail is a monitoring service that tracks activity in our AWS account. These logs are useful to detect suspicious activity and are actually following the rules for something. 

CloudTrail events include types like data, management, and network activity. We will set up our trail to track Management Events, as accessing or opening a secret falls into that category.

### Read vs Write Activity

Read API activity involves accessing, reading, or opening a resource. Write API activity involves altering, updating, or deleting a resource.ce. For this project, we need the Write Activity because we are accessing a secret.

---

## Verifying CloudTrail

I retrieved the secret in two ways: First, by going to the Secrets Manager tab and revealing the secret that was typed in the events tab. Second, using AWS CloudShell Terminal.

To analyze CloudTrail events to see the event where we get our secrets value, we visited the Event History in CloudTrail. There is a GetSecretValue event that always gets tracked, whether we accessed it over the console or not. This shows that CloudTrail can track when and where we open our Secret in Secrets Manager key. 

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_s8t9u0v1)

---

## CloudWatch Metrics

CloudWatch Logs is a monitoring service that gathers data from all AWS applications, such as CloudTrail, to help us create alarms for. It's important for monitoring as you get to create insights and get alerts based on events happening in your AWS account.

CloudTrail's Event History is useful for reading management events that happened within the last 90 days, while CloudWatch Logs are better for combining and accessing logs for longer than 90-day periods through advanced filtering.

CloudWatch metrics are a way we count and track events that are in a log group. The metric value represents how many times we count an event when it passes our filters. In our case, we increase the metric value by one each time our secret is accessed. The default value is to track when an event does not occur.

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_a9b0c1d2)

---

## CloudWatch Alarm

CloudWatch alarm is an alert system in CloudWatch that is designed to indicate when certain conditions occur in our log group. We set our CloudWatch alarm threshold to be about how many times our GetSecretValue event happens within 5 minutes, so the alarm will trigger when the average number of times is above 1. 

We created an SNS Topic, which is like the broadcast channel for emails, phone numbers, and various related functions. Applications can subscribe in order to receive SNS to see if it has a new update to share. In our case, our SNS is set up to send us an email when our secret gets accessed.

AWS requires email confirmation because it sends the alerts directly to the email associated with receiving the SNS alarms. This helps prevent penetrators or any anomalies from accessing the secrets in the Secrets Manager without authorized access.

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_fsdghstt)

---

## Troubleshooting Notification Errors

To test my monitoring system, I opened and accessed my secret in SecretsManager. The results were that we did not get any email at all.

When troubleshooting the notification issues, I checked every part of my monitoring system. Whether CloudTrail picked up events that are happening when I access my secret. Whether CloudTrail is sending logs to CloudWatch, or if the filter was potentially rejecting the correct events, and even if the alarm gets triggered at all. 

CloudWatch was configured using the wrong threshold. Instead of calculating the average number of times a secret is accessed in a time period, it should have been the sum. 

---

## Done

To validate that our monitoring system can detect and alert when my secret is accessed, I checked my secret's value one more time. I received an email within 2 minutes of the event. Our alarm in CloudWatch is in an "alarm" state as well. 

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_ageraergearge)

---

## Comparing CloudWatch with CloudTrail Notifications

After enabling CloudTrail SNS, our inbox was filled with new emails from SNS, and the logs themselves don't show what happened in terms of management events that occurred. We only see that new logs have been stored in our bucket. 

After enabling CloudTrail SNS notifications, my inbox started flooding with emails. In terms of the usefulness of these emails, I thought, why doesn't it show where the alert specifically happens?

![Image](http://learn.nextwork.org/optimistic_navy_bold_sage/uploads/aws-security-monitoring_d7e8f9g0)

---

---
