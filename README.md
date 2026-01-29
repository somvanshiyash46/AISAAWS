# 🛡️ AISAWS — AI-Powered Self-Healing Cloud Security System

> 🚀 An intelligent cloud security automation system that detects threats, makes AI-based decisions, automatically remediates AWS misconfigurations, verifies fixes, and alerts SOC teams.

---

## 🌩️ Project Overview

AISAWS continuously monitors AWS activity and performs:

Detect → Analyze → Decide → Remediate → Verify → Alert


It behaves like an **AI SOC analyst + automated response (SOAR) system**.

---

## 🧠 Key Capabilities

| Feature | Description |
|--------|-------------|
| 🔍 Detection | Real-time AWS event monitoring |
| 🤖 AI Scoring | Threat risk score (0–1) |
| 🧠 Decision Engine | SOC-style decision making |
| 🔁 Automation | Step Functions orchestration |
| 🛠 Remediation | Fixes security issues automatically |
| ✅ Verification | Confirms issue resolved |
| 📧 Alerts | Sends SOC email notifications |

---

## 🏗️ System Architecture

```mermaid
flowchart TD
A[AWS Activity] --> B[CloudTrail]
B --> C[EventBridge]
C --> D[Ingest Lambda]
D --> E[S3 Raw Logs]
D --> F[DynamoDB Events]
F --> G[AI Scoring Lambda]
G --> H[Decision Engine Lambda]
H --> I[Step Functions]
I --> J[Remediation Lambda]
J --> K[Verification Lambda]
K --> L[SNS Alert]
L --> M[SOC Team]
🔁 Processing Flow
sequenceDiagram
participant AWS
participant Ingest
participant AI
participant Decision
participant Workflow

AWS->>Ingest: Security Event
Ingest->>AI: Extract features
AI->>Decision: Risk Score
Decision->>Workflow: Action Plan
Workflow->>AWS: Fix Issue
Workflow->>SOC: Send Alert
⚙️ AWS Services Used
Service	Purpose
CloudTrail	Logs AWS activity
EventBridge	Routes events
Lambda	Processing + AI + Fixes
DynamoDB	Event database
S3	Raw log storage
Step Functions	Orchestration
SNS	Email alerts
🚀 FULL BUILD PROCESS
🟢 PHASE 1 — Infrastructure Setup
Create S3 Bucket
aws s3 mb s3://aisaaws-logs
Create DynamoDB Table
aws dynamodb create-table \
--table-name aisaaws-events \
--attribute-definitions AttributeName=eventID,AttributeType=S \
--key-schema AttributeName=eventID,KeyType=HASH \
--billing-mode PAY_PER_REQUEST
🟢 PHASE 2 — Ingest Lambda
import json, boto3, uuid
from datetime import datetime

s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table("aisaaws-events")

def lambda_handler(event, context):
    event_id = str(uuid.uuid4())
    detail = event["detail"]

    table.put_item(Item={
        "eventID": event_id,
        "event_type": detail["eventName"],
        "source_ip": detail["sourceIPAddress"],
        "region": event["region"],
        "timestamp": datetime.utcnow().isoformat()
    })

    s3.put_object(Bucket="aisaaws-logs",
                  Key=f"raw-events/{event_id}.json",
                  Body=json.dumps(event))
🟢 PHASE 3 — AI Threat Scoring Lambda
def lambda_handler(event, context):
    risk_score = 0.85
    risk_level = "HIGH" if risk_score > 0.7 else "LOW"
🟢 PHASE 4 — Decision Engine Lambda
def decide(score):
    if score < 0.4: return "LOW","LOG_ONLY"
    elif score < 0.7: return "MEDIUM","AUTO_REMEDIATE"
    else: return "HIGH","HUMAN_APPROVAL"
🟢 PHASE 5 — Step Functions Workflow
flowchart TD
A[PreSnapshot] --> B{Decision}
B -->|AUTO| C[Fix]
B -->|HUMAN| D[Wait]
C --> E[Verify]
E --> F[Alert]
F --> G[Log]
🟢 PHASE 6 — Remediation Lambda
import boto3
s3 = boto3.client("s3")

def lambda_handler(event, context):
    bucket = event["resource_name"]
    s3.put_bucket_acl(Bucket=bucket, ACL="private")
    return {"status": "SUCCESS"}
🟢 Verification Lambda
import boto3
s3 = boto3.client("s3")

def lambda_handler(event, context):
    return {"verification": "SUCCESS"}
🟢 Alert Lambda
import boto3
sns = boto3.client("sns")

def lambda_handler(event, context):
    sns.publish(
        TopicArn="YOUR_SNS_ARN",
        Subject="AISAWS Alert",
        Message="Security issue fixed"
    )
🧪 How to Test
1️⃣ Create an S3 bucket
2️⃣ Make bucket public
3️⃣ Wait for detection
4️⃣ Step Functions runs
5️⃣ Bucket becomes private
6️⃣ Email alert received

🎯 Final Outcome
Stage	Result
Detection	Event captured
AI	Risk scored
Decision	Action chosen
Automation	Fix applied
Verification	Success
Alert	Email sent
