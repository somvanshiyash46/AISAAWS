
# 🛡️ AISAWS — AI-Powered Self-Healing Cybersecurity System for AWS

## 📘 Project Description
**AISAWS** is an intelligent, autonomous cybersecurity framework built for AWS environments.
It uses **AI-driven anomaly detection** and **automated remediation workflows** to protect cloud resources from misconfigurations, attacks, and suspicious behavior.

By integrating **AWS CloudTrail, GuardDuty, EventBridge, Lambda, SageMaker, and Step Functions**, AISAWS creates a **self-healing AWS ecosystem** capable of:
- Detecting unusual activities in real-time  
- Scoring events with an AI model (Isolation Forest / Autoencoder)  
- Executing automated or semi-automated remediation  
- Alerting security teams through dashboards and notifications  

---

## ⚙️ System Architecture (Text Diagram)
```
+------------------+       +-------------------+       +----------------+
| CloudTrail/Logs  | --->  | EventBridge Rules | --->  | Lambda Ingest  |
+------------------+       +-------------------+       +----------------+
                                                                |
                                                                v
                                                    +-----------------------+
                                                    | DynamoDB / S3 Storage |
                                                    +-----------------------+
                                                                |
                                                                v
                                                    +-----------------------+
                                                    | SageMaker Model (AI)  |
                                                    +-----------------------+
                                                                |
                                                                v
                                                    +-----------------------+
                                                    | Step Functions        |
                                                    | (Remediation Logic)   |
                                                    +-----------------------+
                                                                |
                                                                v
                                                    +-----------------------+
                                                    | SNS / Slack Alerts    |
                                                    +-----------------------+
```

---

## 🧠 Core Features
- 🔍 **Real-Time Threat Detection**
- 🤖 **Machine Learning Anomaly Scoring**
- ⚡ **Auto-Remediation Playbooks**
- 🧩 **Human-in-the-loop Approvals**
- 📊 **Security Dashboards (OpenSearch / CloudWatch)**
- 🧱 **Fully Automated Infrastructure (SAM / CloudFormation)**

---

## 📅 Implementation Roadmap (12 Weeks)

| Phase | Duration | Objective |
|:------|:----------|:-----------|
| Week 1 | 5 days | Project setup & IAM/CloudTrail baseline |
| Week 2 | 5 days | EventBridge + Lambda ingestion pipeline |
| Week 3 | 5 days | Feature engineering for anomaly detection |
| Week 4 | 5 days | Local ML prototype (IsolationForest) |
| Week 5 | 5 days | Move model to AWS SageMaker |
| Week 6 | 5 days | Real-time inference integration |
| Week 7 | 5 days | Lambda + Step Function remediation logic |
| Week 8 | 5 days | Dashboard + SNS alert integration |
| Week 9–12 | 20 days | Testing, documentation, CI/CD, demo |

---

## 🧩 Tech Stack

| Category | Services/Tools |
|:----------|:---------------|
| **Compute** | AWS Lambda, EC2 |
| **AI/ML** | SageMaker, Python (scikit-learn, boto3, pandas) |
| **Data Storage** | S3, DynamoDB |
| **Security & Monitoring** | GuardDuty, CloudTrail, Config, SNS |
| **Automation** | Step Functions, SAM / CloudFormation |
| **Visualization** | OpenSearch, CloudWatch |
| **Version Control** | GitHub |

---

## 🚀 Setup Guide

### Prerequisites
- AWS Account  
- AWS CLI configured (`aws configure`)  
- IAM user with Lambda, S3, SageMaker, and CloudFormation permissions  

### Steps
```bash
# 1️⃣ Clone the repository
git clone https://github.com/<your-username>/AISAWS.git
cd AISAWS

# 2️⃣ Deploy AWS SAM template
sam build
sam deploy --guided

# 3️⃣ Train ML Model
cd model/
python train_model.py

# 4️⃣ Test event ingestion
aws events put-events --entries file://sample_event.json
```

---

## 📊 Example Use Case
1. Unauthorized IAM activity triggers EventBridge rule.  
2. Lambda sends event → SageMaker model scores as anomaly.  
3. Step Function launches remediation Lambda.  
4. SNS sends alert to Slack/email.  
5. OpenSearch dashboard updates incident in near real time.  

---

## 🔒 Security & Compliance
- IAM least-privilege roles enforced  
- End-to-end encryption (KMS, SSL/TLS)  
- Logs stored securely in S3 with versioning  
- Audit trail available via CloudTrail  

---

## 🧠 Future Enhancements
- Integrate **LLM-based Threat Reasoning (AWS Bedrock)**  
- Add **Multi-Account AWS Security Hub correlation**  
- Extend to hybrid cloud (Azure / GCP connectors)  
- Implement **predictive risk scoring**  

---

## 👨‍💻 Author
**Yash Somvanshi (Hari)**  
Cybersecurity Researcher & Cloud Security Enthusiast  
📧 Email: your_email@example.com  
🌐 LinkedIn: [Your LinkedIn Profile](https://linkedin.com/in/your-link)

---
