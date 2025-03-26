
# Project Description: Instance Scheduler on AWS

## 1. Project Name:
**Instance Scheduler on AWS – Automate EC2 Start/Stop Jobs**

## 2. Team Members:
Rodiakina Yana, Viktoryia Leshava

## Problem Statement

Many organizations run EC2 instances that are not required 24/7, such as development, testing, or training environments. Keeping these instances running continuously leads to unnecessary costs. Manually starting and stopping instances is inefficient and prone to human error. Therefore, a reliable and automated solution is needed to manage EC2 instance schedules based on specific time periods.

## Solution Overview

To solve the problem of unnecessary EC2 costs, we implemented the AWS Instance Scheduler. This solution allows users to automatically start and stop EC2 instances based on custom-defined schedules.

The deployment is done through AWS CloudFormation, which sets up all required resources, including DynamoDB, Amazon EventBridge, Lambda functions, IAM roles, and Amazon SNS for notifications.

Users define schedules and time periods in DynamoDB tables. EC2 instances are tagged with schedule names, which the Lambda functions read and use to control instance state. This ensures resources run only when needed, helping reduce costs and improve efficiency.

## Architecture Diagram

The architecture of the Instance Scheduler solution consists of the following components:

1. **User (Admin)** – initiates the deployment by launching the CloudFormation stack.  
2. **AWS CloudFormation** – provisions all necessary resources for the scheduler solution.  
3. **Amazon DynamoDB** – stores configuration data such as schedules and time periods.  
4. **Amazon EventBridge** – triggers AWS Lambda functions based on a defined schedule.  
5. **AWS Lambda (Scheduler Functions)** – reads schedules from DynamoDB and starts/stops EC2 instances accordingly.  
6. **Amazon SNS** – sends notifications about scheduler actions.  
7. **Amazon EC2 Instance** – the instance is controlled based on the schedule.  
8. **Tag-based linking** – EC2 instances are linked to schedules using specific tags defined in the configuration.

This architecture enables automatic management of instance uptime, helping to optimize cost and reduce manual effort.

## AWS Services Used

1. **AWS CloudFormation**  
Used to deploy the entire Instance Scheduler solution as a stack. It provisions all the necessary resources automatically.

2. **Amazon DynamoDB**  
Stores schedule and period configuration data. Lambda functions read from this table to determine when to start/stop instances.

3. **Amazon EventBridge**  
Triggers the Lambda functions at scheduled times based on configuration.

4. **AWS Lambda**  
Executes the logic to start and stop EC2 instances based on the schedule defined in DynamoDB.

5. **Amazon EC2**  
Virtual machines that are started and stopped according to the schedule.

6. **Amazon SNS**  
Sends notifications about scheduler actions such as instance start or stop.

7. **AWS IAM (Identity and Access Management)**  
Provides the necessary roles and permissions for Lambda and CloudFormation to interact with other services securely.

## How It Works

1. **Deployment via CloudFormation**  
The user deploys the Instance Scheduler solution using an AWS CloudFormation template. This automatically provisions all required resources.

2. **Configuration in DynamoDB**  
Configuration tables are created in DynamoDB, including schedules (e.g., “chicago-office-hours”) and periods (e.g., “office-hours”). These define when EC2 instances should be running or stopped.

3. **EC2 Instances and Tagging**  
The user launches EC2 instances and applies a tag (e.g., Schedule = chicago-office-hours). This tag links the instance to the schedule stored in DynamoDB.

4. **Scheduled Lambda Execution via EventBridge**  
Amazon EventBridge triggers AWS Lambda functions on a regular schedule (e.g., every 5 minutes). The functions read DynamoDB and apply start/stop actions to EC2 instances based on matching tags.

5. **Notifications via SNS**  
SNS sends optional notifications about start/stop events if configured.

## Key Benefits

1. **Automation**  
EC2 instances are automatically started and stopped according to a defined schedule, reducing the need for manual intervention.

2. **Cost Savings**  
Helps significantly reduce costs by stopping unused resources outside working hours.

3. **Scalability**  
The solution easily scales to support multiple instances and schedules using DynamoDB and tag-based linking.

4. **Flexibility**  
Schedules can be customized for different teams, time zones, and office hours by modifying the DynamoDB table.

5. **Integration with AWS Services**  
The solution uses only managed AWS services (CloudFormation, Lambda, EventBridge, DynamoDB, EC2, SNS), making it reliable and extensible.

6. **Notifications**  
Amazon SNS can be used to send alerts for changes or errors, improving process visibility.
