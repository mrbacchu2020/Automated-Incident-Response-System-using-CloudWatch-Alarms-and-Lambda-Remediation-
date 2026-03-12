# Automated-Incident-Response-System-using-CloudWatch-Alarms-and-Lambda-Remediation
“Implemented an Automated Incident Response System using AWS CloudWatch Alarms and Lambda. When EC2 CPU usage exceeds a defined threshold, CloudWatch triggers a Lambda function that automatically performs remediation actions like restarting the instance, reducing downtime and improving system reliability.”

# Project Overview:
This project implements an automated incident response system using AWS services. Amazon CloudWatch monitors the CPU utilization of an EC2 instance, and when the usage exceeds a defined threshold, a CloudWatch alarm is triggered. The alarm invokes an AWS Lambda function that automatically performs remediation, such as restarting the EC2 instance. All actions are logged in CloudWatch Logs. This system helps reduce downtime and enables faster automated recovery from infrastructure issues.

# Components Used:
Amazon EC2,
Amazon CloudWatch,
AWS Lambda.

<img width="1536" height="1024" alt="Automated incident response flowchart" src="https://github.com/user-attachments/assets/2286de95-3a18-4fcc-9b5f-4c98a3dce845" />
