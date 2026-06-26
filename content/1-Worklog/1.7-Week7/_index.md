---
title: "Week 7 Worklog"
date: 2026-06-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
📋 **Week 7 Worklog** — 31/05/2026 – 06/06/2026
{{% /notice %}}

### Week 7 Objectives:
* Learn AWS Lambda and build an event-driven S3-to-SQS trigger.
* Develop the AI Engine to read from SQS and run the ONNX model.
* Integrate components to write prediction results into RDS PostgreSQL.

### Tasks carried out this week:
| Day | Task | Start Date | Completion Date | Reference Material |
| --- | --- | --- | --- | --- |
| Monday | - Learn AWS Lambda and event-driven architecture | 01/06/2026 | 01/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| Tuesday | - Build Lambda function triggered by new S3 log files | 02/06/2026 | 02/06/2026 | https://cloudjourney.awsstudygroup.com/ |
| Wednesday | - Configure Lambda to automatically push Zeek data to SQS | 03/06/2026 | 03/06/2026 | |
| Thursday | - Develop AI Engine to read batch data from SQS | 04/06/2026 | 04/06/2026 | |
| Friday | - Integrate ONNX model and write predictions to RDS PostgreSQL | 05/06/2026 | 05/06/2026 | |

### Week 7 Achievements:
* Created a seamless serverless pipeline using Lambda to route logs from S3 to SQS.
* Developed a robust AI Engine capable of polling SQS messages efficiently.
* Successfully connected the complete data flow, storing AI prediction outputs directly into the RDS database.
