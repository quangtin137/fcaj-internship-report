---
title: "Week 1 - Internship Onboarding and AI1 Scope Analysis"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice info %}}
📋 **Week 1 Worklog** — 17/04/2026 – 23/04/2026
{{% /notice %}}

### Weekly Overview
This week marked the beginning of my FCJ Cloud Internship. I focused on understanding the team's v3.0 Dataset-Centric / Zeek-First / Decision-Level Fusion project outline and identifying my specific responsibilities as the AI1 (Network Anomaly Detection) developer.

### Weekly Objectives
* Complete internship onboarding and understand the AWS account structure (IAM, Budgets).
* Study the Hybrid IDS architecture (Zeek, Suricata, AI models, Fusion Layer).
* Define the exact scope of the AI1 model: detecting network-level anomalies using Zeek `conn.log` telemetry.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 17/04/2026 | 3h | Completed internship onboarding and reviewed the team project proposal. | Understood the overall goal of the Hybrid Cloud Security Platform. | Needed clarification on the boundaries between AI models. | Met with the team to divide AI scopes. | Define AI1's specific input and output contract. |
| Day 2 | 18/04/2026 | 4h | Defined AI1 scope based on the Zeek-first dataset strategy. | Confirmed AI1 focuses only on network anomaly detection, not attack classification. | No major issue. | Use Zeek `conn.log` as the primary telemetry for AI1. | Familiarize with AWS services supporting the pipeline. |
| Day 3 | 20/04/2026 | 5h | Explored the architecture's data flow involving SQS, AI Engine, and the Fusion Layer. | Grasped how AI1 output will act as evidence for the decision-level fusion. | Suricata's role seemed overlapping with AI1 at first glance. | Clarified that Suricata provides rule-based evidence, while AI1 provides ML-based anomaly scores. | Study Zeek `conn.log` schema. |
| Day 4 | 21/04/2026 | 4h | Reviewed AWS IAM concepts and AWS Budgets for the project account. | Ensured I can safely practice AWS labs without exceeding cost limits. | No major issue. | Proceed with setting up a secure practice environment. | Prepare for Zeek lab setup. |

### Technical Implementation
I reviewed the project's **decision-level fusion** architecture. I learned that my **AI1** model will consume flow-level features extracted from Zeek's `conn.log` / `conn_dataset`. The AI1 output will not classify specific attack types (which is AI2A's job) but will output a `NORMAL` or `ANOMALY` label along with an `anomaly_score` or `confidence`. This output will then be forwarded to the Fusion Layer, which aggregates results from AI1, AI2A, AI2B, and Suricata evidence to calculate a final Risk Score.

### Challenges & Solutions
* **Challenge:** Initially, I was confused about whether AI1 needed to process HTTP/DNS logs or Suricata alerts directly.
* **Solution:** I coordinated with the team and clarified that AI1 strictly processes `conn.log` flow-level telemetry. Other logs like `http.log` will be handled by AI2B, and Suricata alerts bypass the AI models to go straight to the Fusion Layer. This clearly bounded my scope.

### Internship Reflection
Starting the internship by clearly drawing the boundaries of my AI1 role was essential. I now understand that my responsibility is to build a robust, unsupervised anomaly detection model using Zeek telemetry, while trusting the rest of the architecture (SQS, Fusion Layer) to handle message routing and final alerting decisions.
