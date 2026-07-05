---
title: "Week 2 - Zeek Lab Setup and conn.log Schema Study"
date: 2026-04-24
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice info %}}
📋 **Week 2 Worklog** — 24/04/2026 – 30/04/2026
{{% /notice %}}

### Weekly Overview
This week, I focused on the infrastructure necessary for generating our primary ML telemetry. I studied the security monitoring lab setup and deeply analyzed the Zeek `conn.log` schema to understand the raw network features I would be working with.

### Weekly Objectives
* Understand the security monitoring lab environment on AWS (VPC, EC2).
* Study the Zeek `conn.log` schema and analyze the `conn_dataset`.
* Design the initial feature schema for the AI1 network-level model.
* Recognize the difference between raw logs, processed features, and model input.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 24/04/2026 | 5h | Reviewed the lab's VPC and EC2 setups to understand where Zeek captures traffic. | Understood the network topology generating the data. | Needed to isolate Zeek logs from standard system logs. | Focus only on Zeek `conn.log` outputs in the S3 bucket. | Study `conn.log` fields. |
| Day 2 | 25/04/2026 | 4h | Read Zeek documentation regarding `conn.log` fields (duration, orig_bytes, resp_bytes, conn_state). | Identified foundational flow-level features. | Some fields like `conn_state` are categorical. | Plan to encode categorical connection states in the feature extractor. | Analyze raw `conn_dataset`. |
| Day 3 | 27/04/2026 | 6h | Explored a sample `conn_dataset` to see realistic value distributions. | Noticed missing values represented as hyphens ('-'). | Missing values would break ML inference. | Adopt a `fail_if_missing` policy for required fields, but implement robust parsing for hyphens. | Draft the AI1 feature schema. |
| Day 4 | 28/04/2026 | 4h | Designed the initial AI1 feature schema separating basic flow features from protocol flags. | Created a structured plan for the feature extractor. | Risk of schema inconsistency between training and serving. | Ensure the feature extractor code is strictly shared between the train and inference pipelines. | Prepare for normal traffic collection. |

### Technical Implementation
I studied the security lab environment, familiarizing myself with how VPC and EC2 configurations support the log collection pipeline. My primary technical work involved dissecting the Zeek `conn.log` structure. I began observing basic flow features like `duration`, `orig_bytes`, `resp_bytes`, `orig_pkts`, and `resp_pkts`. I also noted protocol flags (`proto`, `service`) and connection states (`conn_state`). I recognized early on that a strong feature extractor must be built to parse these raw logs consistently, as maintaining schema consistency is the only way to prevent train-serving mismatch when the model goes into production.

### Challenges & Solutions
* **Challenge:** I realized that raw `conn.log` files are not immediately usable by machine learning models due to missing values (often denoted by `-`) and categorical strings.
* **Solution:** I decided that our feature extractor must handle these explicitly. I established a strict schema mindset: raw logs must be deterministically transformed into a fixed-length numeric vector before hitting the model, preparing a foundation for the `feature_manifest.json` I will create later.

### Internship Reflection
Diving into the raw Zeek logs made the project feel much more real. I realized that a machine learning model is completely dependent on the quality and structure of its data pipeline. Learning how the AWS VPC infrastructure actively routes traffic to our Zeek sensors gave me a greater appreciation for the relationship between cloud networking and AI security monitoring.
