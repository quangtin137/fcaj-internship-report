---
title: "Week 8 - Containerized AI Engine and Fusion Output Integration"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice info %}}
📋 **Week 8 Worklog** — 05/06/2026 – 11/06/2026
{{% /notice %}}

### Weekly Overview
This week, I stepped into deployment engineering. I helped containerize the AI1 runtime using Docker and learned how it deploys to Amazon ECS. Furthermore, I coordinated the integration of AI1's output into the project's central decision-level Fusion Layer.

### Weekly Objectives
* Package the AI1 artifacts and inference scripts into a Docker container.
* Understand the deployment architecture using Amazon ECS.
* Forward the AI1 output to the Fusion Layer for decision-level aggregation.
* Ensure AI1 properly functions as just one piece of evidence in the overall Hybrid IDS platform.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 05/06/2026 | 5h | Wrote a Dockerfile to package the Python runtime, `model.joblib`, `preprocessor.joblib`, and inference scripts. | AI1 runtime successfully containerized. | Need to ensure artifact compatibility inside the Linux container. | Prepared inference artifact compatibility checks during container startup. | Test container locally. |
| Day 2 | 06/06/2026 | 4h | Ran the Docker container locally, pushing `smoke_samples.jsonl` through it. | Container processed messages perfectly. | No major issue. | Proceed to learn how this container runs on ECS. | Study Amazon ECS concepts. |
| Day 3 | 08/06/2026 | 4h | Explored Amazon ECS (Elastic Container Service) to understand task definitions and scaling. | Grasped how the team manages container lifecycle on AWS. | How does the container authenticate to SQS on ECS? | Team uses ECS Task Roles; I verified my container doesn't hardcode credentials. | Integrate with Fusion Layer. |
| Day 4 | 09/06/2026 | 5h | Coordinated sending the `label` and `confidence` outputs from AI1 to the Fusion Layer. | AI1 successfully registered as a contributor to the final Risk Score. | Fusion Layer requires uniform JSON structures from AI1, AI2A, and AI2B. | Verified the AI1 output contract strictly adheres to the requested JSON format. | Observe Dashboard outputs. |
| Day 5 | 10/06/2026 | 4h | Observed the project Dashboard displaying final alerts based on aggregated evidence. | Confirmed AI1 anomalies correctly trigger investigations when correlated with Suricata logs. | No major issue. | AI1 is successfully integrated into the platform. | Begin dataset mismatch review. |

### Technical Implementation
I dockerized the AI1 inference engine, ensuring all dependencies and the `.joblib` artifacts were properly packaged. I prepared runtime artifact compatibility tests to ensure `ai1_inference_common.py` executes correctly inside the Linux container environment. I also studied how the team deploys these images via Amazon ECS, noting the use of ECS Task Roles for secure IAM permissions. The most critical integration was hooking AI1's output into the **decision-level fusion**. I checked that the score direction (`higher_is_more_anomalous`) was clearly documented so the Fusion Layer accurately interprets AI1's `confidence`. AI1 provides the network-anomaly evidence, while AI2A provides attack classification, AI2B provides application-layer insights, and Suricata provides signature-based alerts. 

### Challenges & Solutions
* **Challenge:** It was conceptually challenging to accept that an `ANOMALY` label from AI1 wouldn't necessarily result in a final alert on the Dashboard, making it hard to see direct impact during testing.
* **Solution:** I reviewed the decision-level fusion logic with the team. I understood that AI1 is just one piece of evidence. If AI1 flags an anomaly but AI2A and Suricata see nothing, the Fusion Layer might lower the Risk Score to prevent a false positive. This taught me the strength of a Hybrid Security Monitoring Platform over relying on a single model.

### Internship Reflection
Containerizing the application bridged the gap between my local code and the AWS cloud infrastructure. Integrating with the Fusion Layer was a humbling experience; it highlighted that in enterprise security systems, no single machine learning model is a silver bullet. AI1's role is to be a reliable, fast, and precise sensor feeding into a larger, intelligent decision matrix.
