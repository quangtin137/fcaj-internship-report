---
title: "Week 4 - SQS Messaging and Initial Isolation Forest Experiments"
date: 2026-05-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice info %}}
**Week 4 Worklog** - 08/05/2026 - 14/05/2026
{{% /notice %}}

### Weekly Overview
Week 4 combined AWS messaging practice with the first small Isolation Forest trials. I studied how SQS queues decouple producers and consumers, practiced basic boto3 send/receive operations, and used a selected public dataset subset to observe anomaly score behavior. These experiments were useful for learning, but not treated as a stable AI1 baseline.

### Weekly Objectives
* Understand SQS queue behavior and basic boto3 message operations.
* Record useful points from FCAJ Community Day if applicable.
* Run an initial Isolation Forest experiment on selected public dataset features.
* Inspect contamination settings and anomaly score distribution without overclaiming results.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 08/05/2026 | 4h | Studied SQS queue concepts and message lifecycle. | Understood why queues are useful between ingestion and processing components. | Visibility timeout and deletion behavior needed extra attention. | Keep SQS notes focused on decoupling, not final AI1 integration. | Try boto3 send/receive. |
| Day 2 | 09/05/2026 | 4h | Practiced boto3 send_message and receive_message patterns. | Confirmed the basic shape of queue-based communication. | Local credentials and region settings needed checking. | Document setup assumptions for later labs. | Prepare model experiment input. |
| Day 3 | 11/05/2026 | 4h | Prepared a public dataset subset for the first Isolation Forest run. | Created a small numeric feature matrix for testing. | Feature selection changed the input distribution noticeably. | Use the subset only for behavior observation. | Run Isolation Forest with different contamination values. |
| Day 4 | 12/05/2026 | 5h | Ran initial Isolation Forest experiments and inspected anomaly scores. | Saw how contamination affects the number of predicted anomalies. | The score distribution was not easy to interpret as attack probability. | Describe scores as anomaly indicators, not attack classification confidence. | Write exploratory findings. |
| Day 5 | 13/05/2026 | 3h | Summarized why the model trial was exploratory. | Clarified that public dataset experiments were for learning model behavior. | It was tempting to call the first run a baseline. | Avoid naming it as a frozen or stable AI1 version. | Continue limitation analysis next week. |

### Technical Implementation
SQS introduced a different kind of AWS thinking: the producer and consumer do not have to run at the same time. boto3 practice helped me see how message payloads would later need clear structure and validation.

The Isolation Forest trial was intentionally small. I selected numeric features, ran the model, and checked how score distribution changed with contamination. The result was a learning artifact, not a final model contract.

### Challenges & Solutions
* **Challenge:** Isolation Forest was sensitive to feature selection and contamination.
* **Solution:** I compared score distributions instead of presenting the first run as a reliable baseline.

### Internship Reflection
This week was the first time AI1 felt like a model task, but it also showed why early model results can be misleading. Isolation Forest is useful for learning abnormal behavior, yet it depends heavily on the input representation. SQS also reminded me that model output eventually has to move through software components. A model experiment alone is not enough to define a system.
