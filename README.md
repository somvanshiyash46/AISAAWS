🛡️ AISAWS — AI-Powered Self-Healing Cloud Security System

🚀 An intelligent cloud security automation system that detects threats, makes AI-based decisions, automatically remediates AWS misconfigurations, verifies fixes, and alerts SOC teams.

🌩️ Project Overview

AISAWS continuously monitors AWS activity and performs:
Detect → Analyze → Decide → Remediate → Verify → Alert

🧠 Key Capabilities
 | Feature            | Description                         |
| ------------------ | ----------------------------------- |
| 🔍 Detection       | Real-time AWS event monitoring      |
| 🤖 AI Scoring      | Threat risk score (0–1)             |
| 🧠 Decision Engine | SOC-style decision making           |
| 🔁 Automation      | Step Functions orchestration        |
| 🛠 Remediation     | Fixes security issues automatically |
| ✅ Verification     | Confirms issue resolved             |
| 📧 Alerts          | Sends SOC email notifications       |

🏗️ System Architecture
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

🔁 Event Processing Flow
sequenceDiagram
participant AWS
participant Lambda
participant AI
participant Decision
participant Workflow

AWS->>Lambda: Security Event
Lambda->>AI: Extract features
AI->>Decision: Risk Score
Decision->>Workflow: Action Plan
Workflow->>AWS: Fix Issue
Workflow->>SOC: Send Alert

⚙️ AWS Resources Used
| Service        | Purpose                 |
| -------------- | ----------------------- |
| CloudTrail     | Logs AWS activity       |
| EventBridge    | Routes events           |
| Lambda         | Processing + AI + Fixes |
| DynamoDB       | Event database          |
| S3             | Raw log storage         |
| Step Functions | Orchestration           |
| SNS            | Email alerts            |

🧩 PHASE 1 — Setup

🔹 Create S3 Bucket
aws s3 mb s3://aisaaws-logs

🔹 Create DynamoDB Table
aws dynamodb create-table \
--table-name aisaaws-events \
--attribute-definitions AttributeName=eventID,AttributeType=S \
--key-schema AttributeName=eventID,KeyType=HASH \
--billing-mode PAY_PER_REQUEST

🧩 PHASE 2 — Ingest Lambda
import json, boto3, uuid
from datetime import datetime

s3 = boto3.client('s3')
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table("aisaaws-events")

def lambda_handler(event, context):
    event_id = str(uuid.uuid4())
    event_name = event["detail"]["eventName"]
    source_ip = event["detail"]["sourceIPAddress"]

    table.put_item(Item={
        "eventID": event_id,
        "event_type": event_name,
        "source_ip": source_ip,
        "timestamp": datetime.utcnow().isoformat()
    })

    s3.put_object(Bucket="aisaaws-logs",
                  Key=f"raw-events/{event_id}.json",
                  Body=json.dumps(event))

🧠 PHASE 3 — AI Scoring Lambda
def lambda_handler(event, context):
    risk_score = 0.8  # Simulated AI output
    risk_level = "HIGH" if risk_score > 0.7 else "LOW"

🧠 PHASE 4 — Decision Engine
def decide(score):
    if score < 0.4: return "LOW","LOG_ONLY"
    elif score < 0.7: return "MEDIUM","AUTO_REMEDIATE"
    else: return "HIGH","HUMAN_APPROVAL"

🔁 PHASE 5 — Step Functions Workflow
flowchart TD
A[PreSnapshot] --> B{Decision}
B -->|AUTO| C[Fix]
B -->|HUMAN| D[Wait]
C --> E[Verify]
E --> F[Alert]
F --> G[Log]

🛠️ PHASE 6 — Remediation Lambda
import boto3
s3 = boto3.client("s3")

def lambda_handler(event, context):
    s3.put_bucket_acl(Bucket=event["resource_name"], ACL="private")
    return {"status": "SUCCESS"}

📧 Alert Lambda
sns.publish(
    TopicArn="SNS_TOPIC_ARN",
    Subject="AISAWS Security Alert",
    Message="Security issue fixed"
)

🚀 How to Test

1️⃣ Make S3 bucket public
2️⃣ Wait 30s
3️⃣ Watch Step Functions execution
4️⃣ Bucket becomes private
5️⃣ Email alert received

🎯 Final Output
| Stage        | Result         |
| ------------ | -------------- |
| Detection    | Event captured |
| AI           | Risk scored    |
| Decision     | Action chosen  |
| Automation   | Fix applied    |
| Verification | Success        |
| Alert        | Email sent     |

