---
title: "Week 3 - Normal Traffic Collection and Feature Extraction"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice info %}}
📋 **Week 3 Worklog** — 01/05/2026 – 07/05/2026
{{% /notice %}}

### Weekly Overview
This week was heavily focused on data engineering. I collected normal network traffic to build a solid baseline and wrote the feature extraction code necessary to convert raw Zeek logs into the 30-feature vector required for the AI1 model.

### Weekly Objectives
* Collect normal network traffic to establish the normal baseline.
* Build the feature extractor for Zeek `conn.log`.
* Handle missing values and convert data types.
* Generate derived features and prepare the exact input schema for the Isolation Forest.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 01/05/2026 | 5h | Coordinated with the team to capture varied normal baseline traffic in the lab. | Gathered a diverse `conn_dataset` without attack noise. | Baseline lacked service diversity initially. | Added HTTP, DNS, and normal SSH activities to broaden the baseline. | Begin coding the feature extractor. |
| Day 2 | 02/05/2026 | 6h | Wrote Python scripts to parse basic flow features and handle the `-` missing values. | Successfully cast text log fields to numeric formats. | Needed to derive rate/ratio features to capture temporal behavior. | Implemented logic for `bytes_per_pkt`, `packet_rate`, and `log1p_duration`. | Add protocol and connection state flags. |
| Day 3 | 04/05/2026 | 5h | Engineered connection-state flags (`is_success_state`, `is_failed_like_state`, `has_rst`) and protocol flags. | Categorical values successfully converted to binary features. | Needed a way to capture brute-force-like behavior without state tracking. | Added source aggregation features like `flow_count_per_src` and `unique_dst_ports`. | Add SSH rolling-window features. |
| Day 4 | 05/05/2026 | 5h | Developed SSH rolling-window features (e.g., `ssh_count_60s_same_src`, `ssh_non_success_60s`). | Completed the set of complex derived features. | Keeping track of 60s windows required stateful processing. | Used pandas rolling window operations during training data preparation. | Finalize the 30-feature schema. |
| Day 5 | 06/05/2026 | 4h | Finalized the 30-feature input schema for AI1. | A complete, fixed-width feature vector ready for modeling. | No major issue. | Proceed to document the schema logic. | Prepare for model training. |

### Technical Implementation
I built the feature extractor to transform the raw `conn.log` into our defined 30-feature vector. I handled missing values by replacing Zeek's hyphens with appropriate nulls or zeros, depending on the field. Beyond basic flow features (`duration`, `orig_bytes`, `resp_bytes`), I created derived rate/ratio features like `bytes_per_pkt`, `packet_rate`, and `orig_resp_byte_ratio` to capture behavioral context. I encoded categorical data into flags (`is_tcp`, `is_ssh`, `is_success_state`, `is_failed_like_state`). To provide context for attacks like Port Scans and Brute Force, I aggregated destination/source features (`flow_count_per_src`, `unique_dst_ports`) and implemented critical SSH rolling-window features (`ssh_count_60s_same_src`, `ssh_non_success_60s`). 

### Challenges & Solutions
* **Challenge:** Ensuring the normal baseline was broad enough was difficult. If the model only learned idle background traffic, any active (but benign) user activity would trigger false positives.
* **Solution:** I explicitly generated normal traffic that included heavy HTTP downloads, diverse DNS queries, and successful SSH administrative sessions. This diverse baseline prevents the model from learning a too-narrow definition of "normal".

### Internship Reflection
Writing the feature extraction logic was challenging but highly rewarding. It became evident that the intelligence of the AI1 model doesn't just come from the algorithm, but heavily relies on the quality of the derived features like `packet_rate` or the SSH rolling windows. A robust feature engineering step is truly the backbone of effective anomaly detection.
