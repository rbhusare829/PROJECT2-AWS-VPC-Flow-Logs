# 🚀 AWS DevOps Project 2

## Configure VPC Flow Logs and Store Logs in Amazon S3 Using IAM Role

---

## 📌 Project Overview

This project demonstrates how to enable **VPC Flow Logs** in AWS and store the captured network traffic logs inside an **Amazon S3 bucket** using an **IAM Role**.

VPC Flow Logs help monitor and troubleshoot network traffic within a Virtual Private Cloud (VPC). These logs are useful for **security analysis, auditing, and debugging network connectivity issues**.

---

## 🏗 Architecture

```
EC2 Instance
      │
      │ Network Traffic
      ▼
VPC Flow Logs
      │
      ▼
IAM Role
      │
      ▼
Amazon S3 Bucket
```

---

## 🛠 AWS Services Used

* Amazon VPC
* VPC Flow Logs
* Amazon S3
* AWS IAM
* Amazon EC2 (to generate traffic)

---

## ⚙ Step-by-Step Implementation

### 1️⃣ Create or Identify a VPC

1. Open **AWS Console**
2. Navigate to **VPC**
3. Click **Your VPCs**
4. Create a new VPC or select an existing VPC.

Example configuration:

```
Name: devops-vpc
CIDR Block: 10.0.0.0/16
```

---

### 2️⃣ Create an S3 Bucket

1. Go to **Amazon S3**
2. Click **Create Bucket**

Example configuration:

```
Bucket Name: rohit-vpc-flow-logs
Region: ap-south-1
```

This bucket will store the VPC Flow Logs.

---

### 3️⃣ Create IAM Role for Flow Logs

Go to **IAM → Roles → Create Role**

Trusted entity:

```
AWS Service
```

Custom Trust Policy:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Principal": {
    "Service": "vpc-flow-logs.amazonaws.com"
   },
   "Action": "sts:AssumeRole"
  }
 ]
}
```

Permissions Policy:

```json
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Action": "s3:PutObject",
   "Resource": "arn:aws:s3:::rohit-vpc-flow-logs/*"
  }
 ]
}
```

Role name:

```
VPCFlowLogsRole
```

---

### 4️⃣ Enable VPC Flow Logs

1. Go to **VPC**
2. Select **Your VPC**
3. Click **Flow Logs**
4. Click **Create Flow Log**

Configuration:

```
Filter: ALL
Destination: Send to an Amazon S3 bucket
S3 Bucket ARN: arn:aws:s3:::rohit-vpc-flow-logs
IAM Role: VPCFlowLogsRole
Log Format: AWS Default Format
```

Click **Create Flow Log**.

---

### 5️⃣ Generate Traffic

Launch an EC2 instance or connect to an existing instance and run:

```
ping google.com
```

or

```
curl https://amazon.com
```

This will generate network traffic for the flow logs.

---

### 6️⃣ Verify Logs in S3

Go to:

```
S3
→ rohit-vpc-flow-logs
→ AWSLogs
→ Account-ID
→ VPC-ID
```

You will see a file:

```
log.gz
```

Download and extract this file to analyze the network traffic logs.

---

## 📄 Sample Log Entry

Example:

```
2 123456789 eni-123abc 10.0.1.5 172.217.10.14 443 52344 6 10 840 1678900000 1678900060 ACCEPT OK
```

Explanation:

| Field    | Description             |
| -------- | ----------------------- |
| eni      | Network Interface ID    |
| srcaddr  | Source IP Address       |
| dstaddr  | Destination IP Address  |
| srcport  | Source Port             |
| dstport  | Destination Port        |
| protocol | Protocol used (TCP/UDP) |
| action   | ACCEPT or REJECT        |

---

## 📸 Project Screenshots

Add the following screenshots in your repository:

* VPC Flow Logs configuration
* IAM Role configuration
* S3 bucket containing logs
* Sample log file (`log.gz`)

---

## 🎯 Key Learnings

* Monitoring network traffic using VPC Flow Logs
* Using IAM Roles for secure access
* Storing logs in Amazon S3
* Understanding network log structure
* Troubleshooting VPC networking issues

---

## 👨‍💻 Author

**Rohit Bhusare**

Aspiring **DevOps Engineer | Cloud Enthusiast**

GitHub: https://github.com/

---

⭐ If you like this project, feel free to star the repository!
