---
title: "Week 4 - SQS Messaging and Baseline Isolation Forest"
date: 2026-05-08
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---
{{% notice info %}}
📋 **Week 4 Worklog** — 08/05/2026 – 14/05/2026
{{% /notice %}}

### Weekly Overview
This week, I stepped into the AWS messaging services and core ML modeling. I learned how SQS handles messages in real-time pipelines and trained the initial AI1_ISOLATION_FOREST_V1 model on our normal baseline dataset.

### Weekly Objectives
* Understand Amazon SQS and its role in real-time message queuing.
* Train the baseline AI1 Isolation Forest model on normal traffic.
* Understand the `contamination` parameter and the concept of anomaly scores.
* Prepare the initial `NORMAL`/`ANOMALY` + `anomaly_score` output structure.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 08/05/2026 | 4h | Studied Amazon SQS concepts and its producer/consumer separation pattern. | Grasped how SQS buffers real-time log ingestion. | No major issue. | Use SQS concepts to plan how the AI1 inference script will poll data. | Attend FCAJ Community Day. |
| Day 2 | 09/05/2026 | 4h | Participated in FCAJ Community Day and took notes on AI, AWS learning direction, and prompt engineering. | Collected useful insights for the internship report and project reflection. | Needed to connect event lessons with my AI1 role. | Summarize only key takeaways in Worklog and keep the detailed report in Event Participated. | Apply relevant AI workflow lessons to the AI1 reflection. |
| Day 3 | 11/05/2026 | 5h | Trained the `AI1_ISOLATION_FOREST_V1` model using the 30-feature baseline dataset. | Model successfully learned the representation of normal network behavior. | Unsupervised nature meant no immediate validation metric. | Focus on tuning the contamination parameter to observe score distributions. | Analyze anomaly scores. |
| Day 4 | 12/05/2026 | 5h | Evaluated the output `anomaly_score` distributions on the training set. | Noticed how the model isolates data points. | Needed to map raw model scores to a clear `NORMAL`/`ANOMALY` output. | Adopt the `higher_is_more_anomalous` score direction for consistency. | Prepare threshold tuning strategy. |
| Day 5 | 13/05/2026 | 4h | Formatted the AI1 output structure to return `label` and `confidence`. | AI1 output contract is now clearly defined for the Fusion Layer. | No major issue. | Proceed to generate attack traffic to actually test this baseline. | Prepare for attack data generation. |

### Technical Implementation
I explored Amazon SQS to understand how it separates data producers (log shippers) from consumers (AI Engine), preparing for future real-time integrations. I then trained the **Isolation Forest** model on the normal baseline. I chose Isolation Forest because AI1's goal is strictly anomaly detection (finding deviations from normal) rather than being a supervised attack classifier. The model outputs an `anomaly_score` where `higher_is_more_anomalous`. I configured the model output to provide a `NORMAL` or `ANOMALY` label alongside a `confidence` metric, perfectly aligning with the inputs expected by the decision-level Fusion Layer.

### Challenges & Solutions
* **Challenge:** Without labeled attack data, validating the Isolation Forest's initial performance was entirely subjective, relying solely on adjusting the `contamination` parameter.
* **Solution:** I decided to tentatively set the contamination parameter based on statistical outlier expectations in the baseline, but acknowledged that attack data is strictly required, not for training, but to objectively evaluate and freeze the decision threshold in the coming weeks.

### Internship Reflection
Training the baseline model brought the project's logic together. The FCAJ Community Day event also reinforced my understanding of AI-driven workflows. I learned that an unsupervised model shouldn't be forced into the role of an attack classifier; its strength lies purely in knowing what "normal" looks like, making the next step—testing it against real attacks—crucial.
