---
title: "Week 10 - Threshold Tuning and SHAP Explainability"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice info %}}
📋 **Week 10 Worklog** — 19/06/2026 – 25/06/2026
{{% /notice %}}

### Weekly Overview
This week, I focused on fine-tuning the AI1 model's operational performance. I conducted an intensive threshold tuning session to balance the False Positive Rate with the Detection Rate and explored using SHAP to provide explainability for the anomaly scores.

### Weekly Objectives
* Re-evaluate the `selected_threshold` (currently frozen at 0.398066) against recent diverse attack tests.
* Perform threshold tuning to further reduce false positives in edge cases.
* Implement SHAP (SHapley Additive exPlanations) to explain which features drive the `ANOMALY` decisions.
* Consolidate AWS architectural knowledge related to the platform.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 19/06/2026 | 5h | Ran the AI1 evaluation script with a sweeping range of threshold values. | Plotted the trade-off curve between False Positive Rate and Detection Rate. | Needed to ensure the threshold remains robust under mild network spikes. | Keep the threshold strict at `0.398066` for now; AI1 must prioritize avoiding alert fatigue. | Research model explainability. |
| Day 2 | 20/06/2026 | 4h | Participated in Event Cloud Architect, an AWS knowledge competition. | Identified gaps in AWS architecture knowledge and gained motivation to review certification-style scenarios. | Some architecture questions required more real deployment experience than theory. | Keep the detailed competition report in Event Participated and reflect only on the learning gap in Worklog. | Review AWS architecture best practices and connect them with the internship system design. |
| Day 3 | 22/06/2026 | 5h | Integrated the SHAP library into the local AI1 evaluation script. | Generated initial SHAP values for a sample of True Positive Brute Force anomalies. | SHAP values can be computationally expensive to compute in real-time. | Use SHAP purely for offline evaluation and explainability, not in the streaming SQS pipeline. | Analyze feature importance. |
| Day 4 | 23/06/2026 | 4h | Generated SHAP summary plots to visually explain the Isolation Forest's decisions. | Confirmed that features like `ssh_count_60s_same_src` heavily influenced Brute Force detection. | No major issue. | Document these SHAP insights to increase the team's trust in AI1. | Analyze false positives with SHAP. |
| Day 5 | 24/06/2026 | 4h | Used SHAP to investigate why certain normal flows were flagged as `ANOMALY`. | Discovered the model is highly sensitive to extreme `duration` and `orig_byte_rate` outliers. | Some long-lived legitimate connections are being penalized. | Note this limitation in the technical documentation; Fusion Layer must handle this via correlation. | Prepare for end-to-end testing. |

### Technical Implementation
I conducted a deep dive into threshold tuning. Since the decision rule is `confidence >= 0.398066 => ANOMALY`, I swept through adjacent values to ensure 0.398066 was still optimal for balancing the False Positive Rate and Detection Rate on our updated dataset. To open the "black box" of the Isolation Forest, I applied SHAP (SHapley Additive exPlanations) in an offline analysis environment. The SHAP feature importance plots revealed that the model was correctly heavily weighting our carefully engineered SSH rolling-window features (e.g., `ssh_count_60s_same_src`, `ssh_non_success_60s`) during Brute Force attacks. However, it also showed that the `duration` and `packet_rate` features were the primary culprits behind our false positives.

### Challenges & Solutions
* **Challenge:** Explaining *why* AI1 flagged a specific connection as anomalous was difficult when team members asked for justification during false positive triage.
* **Solution:** By generating SHAP force plots for specific flow instances, I could scientifically demonstrate which Zeek features pushed the anomaly score above the threshold. I clarified that SHAP is too slow for the real-time SQS inference pipeline, but it is an invaluable offline tool for building trust in the model and guiding future feature engineering.

### Internship Reflection
The Event Cloud Architect competition reminded me of the broader AWS ecosystem, while my daily tasks required deep focus on ML explainability. Using SHAP was a turning point; it transformed AI1 from an opaque math equation into a transparent set of logical decisions based on network features. A model that can be explained is infinitely more valuable in a security context than a slightly more accurate "black box."
