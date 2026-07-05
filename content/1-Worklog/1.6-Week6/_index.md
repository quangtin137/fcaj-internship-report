---
title: "Week 6 - AI1 Evaluation and Artifact Freezing"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice info %}}
📋 **Week 6 Worklog** — 22/05/2026 – 28/05/2026
{{% /notice %}}

### Weekly Overview
This week was a critical milestone. I evaluated the AI1 model using standard metrics, froze the optimal decision threshold, and finalized the technical artifacts required for downstream deployment.

### Weekly Objectives
* Evaluate AI1 using metrics like False Positive Rate and Detection Rate.
* Define and freeze the decision threshold for the `NORMAL`/`ANOMALY` output.
* Standardize AI1 artifacts (`model_card.md`, `feature_manifest.json`, `thresholds_frozen.json`).
* Dump the model and preprocessor for the inference environment.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 22/05/2026 | 5h | Evaluated the AI1 model using the labeled attack dataset to calculate False Positive Rate and Detection Rate. | Identified the trade-off curve between catching attacks and causing alert fatigue. | Needed to select a definitive operating point. | Select a threshold that heavily penalizes false positives, as Fusion Layer relies on clean evidence. | Freeze the decision threshold. |
| Day 2 | 23/05/2026 | 4h | Participated in AWS Vietnam Community Day and noted sessions about AI context, CloudFront, Amazon Quick, LLM non-determinism, and multi-agent systems. | Expanded my understanding of AI and AWS use cases beyond the AI1 model. | Needed to avoid mixing event notes with core AI1 implementation details. | Keep detailed event content in Event Participated and mention only learning impact in Worklog. | Review which lessons can support future AI/security architecture reflection. |
| Day 3 | 25/05/2026 | 4h | Froze the threshold at `0.398066` and documented the decision rule. | Established `if confidence >= 0.398066 => ANOMALY`. | Need to ensure downstream apps use the exact same threshold. | Store the threshold in `thresholds_frozen.json` to be loaded dynamically. | Generate feature manifest. |
| Day 4 | 26/05/2026 | 4h | Created the `feature_manifest.json` outlining the exact 30-feature order. | Guaranteed schema consistency between training and inference. | Inference code might crash if a feature is missing. | Document the `fail_if_missing` policy explicitly in the model card. | Dump joblib artifacts. |
| Day 5 | 27/05/2026 | 5h | Exported `model.joblib`, `preprocessor.joblib`, and generated `smoke_samples.jsonl`. | Completed the `AI1_ISOLATION_FOREST_V1` artifact package. | Considered ONNX but opted to stick with `joblib` for now based on current runtime compatibility. | Prepare for integration testing with the joblib artifacts. | Prepare for SQS integration. |

### Technical Implementation
I evaluated the `AI1_ISOLATION_FOREST_V1` model, focusing heavily on minimizing the False Positive Rate. I analyzed the score distribution and officially froze the `selected_threshold = 0.398066`. The decision rule is strictly defined: `confidence >= 0.398066` outputs `ANOMALY`, while anything lower is `NORMAL`, adhering to the `higher_is_more_anomalous` score direction. I finalized the deployment package by creating `feature_manifest.json` (locking in the 30 features), `thresholds_frozen.json`, and exporting `model.joblib` and `preprocessor.joblib`. I also wrote the `model_card.md` to document the model's objective, scope (`ZEEK_CONN_FLOW_ANOMALY_FEATURES`), and the `fail_if_missing` data policy. Finally, I created `smoke_samples.jsonl` to allow downstream engineers to test their integrations easily.

### Challenges & Solutions
* **Challenge:** Ensuring the inference environment executes exactly the same preprocessing and ordering as the training environment is a notoriously difficult MLOps problem.
* **Solution:** By freezing the `feature_manifest.json` and exporting the `preprocessor.joblib` alongside the model, I decoupled the logic from my training notebook. The inference script will strictly read the manifest to enforce column ordering and apply the preprocessor, virtually eliminating train-serving schema mismatch.

### Internship Reflection
Attending the AWS Vietnam Community Day expanded my perspective on cloud AI architectures, while my daily tasks required extreme detail-orientation. Freezing the artifacts and documenting the model card felt like a transition from "experimentation" to "engineering." AI1 is no longer just a script on my laptop; it is a versioned, strictly contracted microservice component ready for pipeline integration.
