---
title: "Week 7 - SQS to AI1 Inference Pipeline Integration"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
📋 **Week 7 Worklog** — 29/05/2026 – 04/06/2026
{{% /notice %}}

### Weekly Overview
This week, I bridged the gap between data engineering and machine learning operations. I worked on integrating the frozen AI1 artifacts into the inference pipeline, receiving streaming network-flow messages from an Amazon SQS queue.

### Weekly Objectives
* Develop the inference adapter to consume messages from SQS via batch polling.
* Implement `ai1_inference_common.py` to handle the runtime logic using the frozen artifacts.
* Ensure the inference adapter properly formats the `label`, `confidence`, and `selected_threshold` output.
* Prepare the AI1 output format required by the downstream Fusion Layer.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 29/05/2026 | 5h | Reviewed the team's Feature Router and SQS batch polling setup. | Understood how AI1 will receive parsed Zeek logs from the queue. | Message payload schema from SQS didn't initially match the training format. | Coordinate with the team to ensure SQS messages adhere to the expected flow-level schema. | Implement artifact loading logic. |
| Day 2 | 30/05/2026 | 5h | Developed `ai1_inference_common.py` to load `model.joblib`, `preprocessor.joblib`, and manifests. | Artifacts successfully loaded into the inference runtime memory. | Need to enforce the `fail_if_missing` policy before inference. | Write a validation block checking for all 30 features against `feature_manifest.json`. | Write the core inference function. |
| Day 3 | 01/06/2026 | 4h | Implemented the inference function to transform data and calculate the anomaly score. | Model successfully calculated raw scores on streaming data. | Needed to apply the frozen decision rule. | Load `thresholds_frozen.json` and map scores to `NORMAL` / `ANOMALY`. | Format output payload. |
| Day 4 | 02/06/2026 | 5h | Formatted the output to strictly include `label`, `confidence`, and `selected_threshold`. | Prepared the exact output contract expected by the Fusion Layer. | Tested locally, but need to verify with SQS. | Connect the inference helper directly to the SQS polling script. | Conduct integration testing. |
| Day 5 | 03/06/2026 | 4h | Ran an integration test reading `smoke_samples.jsonl` through the inference pipeline. | The pipeline successfully consumed messages and produced correct labels. | No major issue. | Proceed to containerize this setup next week. | Review containerization requirements. |

### Technical Implementation
I focused on building `ai1_inference_common.py`, the core runtime logic for AI1. The script first loads `model.joblib`, `preprocessor.joblib`, `feature_manifest.json`, and `thresholds_frozen.json`. I implemented batch polling to read parsed Zeek logs from SQS. Crucially, I verified that the AI1 adapter receives all 30 required features in the correct order as dictated by the manifest. Adhering to the `fail_if_missing` policy, the script validates the payload before running the preprocessor and model. After calculating the raw score, it applies the `higher_is_more_anomalous` rule against the `selected_threshold` (0.398066) to generate the final output object containing `label`, `confidence`, and the threshold value itself.

### Challenges & Solutions
* **Challenge:** There was a schema mismatch during initial integration testing. The SQS messages were missing some derived features like the SSH rolling-windows because the upstream Feature Router wasn't computing them correctly for real-time streaming.
* **Solution:** Rather than letting the model predict on corrupted data, the `fail_if_missing` policy immediately rejected the payloads, throwing an explicit error. I coordinated with the team to fix the Feature Router, ensuring all 30 features were correctly computed and present before hitting the AI1 inference adapter.

### Internship Reflection
Writing the inference pipeline was highly satisfying. Seeing the `ai1_inference_common.py` script successfully load the artifacts, reject malformed payloads, and confidently output `NORMAL`/`ANOMALY` decisions proved the value of freezing the artifacts and writing strict input/output contracts. It demonstrated that robust MLOps is just as much about software engineering as it is about data science.
