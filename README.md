# Automated Incident Response System using AWS

## Project Overview

This project demonstrates an automated incident response system using AWS services. The system continuously monitors EC2 instance CPU utilization using Amazon CloudWatch. When CPU usage exceeds a defined threshold, a CloudWatch alarm is triggered which invokes an AWS Lambda function. The Lambda function automatically performs remediation by restarting the EC2 instance. All remediation actions are logged in CloudWatch Logs.

This project shows how cloud monitoring and serverless automation can be used to build a **self-healing infrastructure system**.

---

## Architecture Flow

<img width="1536" height="1024" alt="Automated incident response flowchart" src="https://github.com/user-attachments/assets/5c3e80bb-36fd-48f6-981a-d274f9d4daae" />


---

## AWS Services Used

* **Amazon EC2** – Server being monitored
* **Amazon CloudWatch** – Monitoring metrics and triggering alarms
* **AWS Lambda** – Automated remediation logic
* **CloudWatch Logs** – Logging remediation actions
* **Auto Scaling (Optional)** – Scaling infrastructure if required

---

## Project Setup Steps

### 1. Launch EC2 Instance

* Launch an EC2 instance (Amazon Linux)
* Enable SSH access
* Install stress tool to generate CPU load

Example:<img width="1920" height="1200" alt="Screenshot 2026-03-12 144433" src="https://github.com/user-attachments/assets/af3356ee-c12f-416d-b605-54ac347bbe2c" />

<img width="1920" height="1200" alt="Screenshot 2026-03-12 115032" src="https://github.com/user-attachments/assets/233e6932-8d6d-43d3-92fb-b466263fbe3b" />


```
sudo yum install stress -y
```

---

### 2. Create CloudWatch Alarm

* Go to **CloudWatch → Alarms → Create Alarm**
* Select metric: **EC2 → CPUUtilization**
* Configure threshold:
* <img width="1920" height="1200" alt="Screenshot 2026-03-12 152649" src="https://github.com/user-attachments/assets/9f93bfe7-89ed-4c02-8e05-a1c42f3e1da7" />

<img width="1920" height="1200" alt="Screenshot 2026-03-12 152707" src="https://github.com/user-attachments/assets/947be52f-e407-41f2-85ab-6fbba760873e" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 152723" src="https://github.com/user-attachments/assets/770b433d-a83d-4d95-998a-ccbac45c7396" />

```
CPUUtilization > 70%
Evaluation period: 2 minutes
```

---

### 3. Create Lambda Function

Create a Lambda function with Python runtime.

Example Lambda Code:

```python
import boto3

def lambda_handler(event, context):

    ec2 = boto3.client('ec2')

    instance_id = "YOUR_INSTANCE_ID"

    ec2.reboot_instances(
        InstanceIds=[instance_id]
    )

    print("EC2 instance restarted:", instance_id)
```

---

### 4. Configure IAM Role

Attach the required permission to Lambda:

```
AmazonEC2FullAccess
```

This allows Lambda to reboot the EC2 instance.

---

### 5. Connect CloudWatch Alarm to Lambda

* In alarm actions select **Invoke Lambda Function**
* Choose your Lambda function.

---

### 6. Testing the System

Generate CPU load on EC2:

```
stress --cpu 4 --timeout 300
```

When CPU crosses the threshold:

* CloudWatch Alarm enters **ALARM state**
* Lambda function is triggered
* EC2 instance is rebooted automatically

---

## Logs and Monitoring

All actions performed by Lambda are recorded in:

```
CloudWatch → Log Groups → /aws/lambda/ec2-remediations
```

This helps track automated remediation actions.

---

## Expected Outcome

* EC2 CPU utilization is continuously monitored.
* High CPU usage triggers an alarm.
* Lambda automatically restarts the EC2 instance.
* System recovers automatically without manual intervention.

---

## Conclusion

This project demonstrates how AWS monitoring and automation tools can be combined to create an **automated incident response system**. The solution improves system reliability, reduces downtime, and enables faster recovery from infrastructure issues.

---

## Author

Shreyash Meshram
DevOps / Cloud Project
