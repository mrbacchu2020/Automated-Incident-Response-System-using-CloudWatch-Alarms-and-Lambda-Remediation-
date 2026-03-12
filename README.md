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
```
CPUUtilization > 70%
Evaluation period: 2 minutes
```

* <img width="1920" height="1200" alt="Screenshot 2026-03-12 152649" src="https://github.com/user-attachments/assets/9f93bfe7-89ed-4c02-8e05-a1c42f3e1da7" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 152707" src="https://github.com/user-attachments/assets/947be52f-e407-41f2-85ab-6fbba760873e" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 152723" src="https://github.com/user-attachments/assets/770b433d-a83d-4d95-998a-ccbac45c7396" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 121655" src="https://github.com/user-attachments/assets/27838835-fac7-4ba5-b051-70a932cd7ff0" />


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
<img width="1920" height="1200" alt="project4 17" src="https://github.com/user-attachments/assets/64170ac4-dca8-47fa-9cdd-3ea7ec9aadfd" />

<img width="1920" height="1200" alt="Screenshot 2026-03-12 120323" src="https://github.com/user-attachments/assets/38326a21-75b4-4321-a3bc-86cfdcb7e6d7" />



---<img width="1920" height="1200" alt="Screenshot 2026-03-12 120352" src="https://github.com/user-attachments/assets/daf99bf5-c466-4bef-9dfd-09ac5305c3be" />


### 4. Configure IAM Role

Attach the required permission to Lambda:

```
AmazonEC2FullAccess
```
This allows Lambda to reboot the EC2 instance.

<img width="1920" height="1200" alt="Screenshot 2026-03-12 115410" src="https://github.com/user-attachments/assets/29835b50-2171-4516-a7b7-7f30e0a3bca4" />


<img width="1884" height="1193" alt="Screenshot 2026-03-12 115523" src="https://github.com/user-attachments/assets/68d33994-dae2-4040-a8d6-74eb0d999335" />


---

### 5. Connect CloudWatch Alarm to Lambda

* In alarm actions select **Invoke Lambda Function**
* Choose your Lambda function.


<img width="1920" height="1200" alt="Screenshot 2026-03-12 142628" src="https://github.com/user-attachments/assets/fd9a9a6d-1b1a-4dd8-b05d-2e71e642e1b4" />




---

### 6. Testing the System

Generate CPU load on EC2:

```
stress --cpu 4 --timeout 300
```

<img width="1920" height="1200" alt="Screenshot 2026-03-12 143250" src="https://github.com/user-attachments/assets/4cdb3c63-a698-4790-aed8-04481c345e6e" />




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


<img width="1920" height="1200" alt="Screenshot 2026-03-12 161312" src="https://github.com/user-attachments/assets/81620d17-3e97-41c2-8c35-1525d50c7ce6" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 161337" src="https://github.com/user-attachments/assets/5e280dd7-43a4-46cf-935a-9278334f2958" />


<img width="1920" height="1200" alt="Screenshot 2026-03-12 144201" src="https://github.com/user-attachments/assets/264216d7-a33d-405d-b392-d463a36e19e9" />


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
