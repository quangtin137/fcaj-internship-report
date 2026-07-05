---
title: "Week 9 - Dataset Mismatch Review and Model Generalization"
date: 2026-06-12
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice info %}}
📋 **Week 9 Worklog** — 12/06/2026 – 18/06/2026
{{% /notice %}}

### Weekly Overview
This week, I stepped back from the pipeline implementation to conduct a critical dataset mismatch review. I explored incorporating supplementary public datasets into our primary Zeek lab data to improve the generalization of the AI1 Isolation Forest model.

### Weekly Objectives
* Review the concept of train-serving dataset mismatch.
* Evaluate public/Kaggle datasets to see if they can supplement the `conn_dataset`.
* Standardize the public dataset schemas to match the Zeek 30-feature schema.
* Assess whether incorporating external data improves AI1's generalization capabilities.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 12/06/2026 | 5h | Reviewed literature on PSI (Population Stability Index) and train-serving mismatch in network anomaly models. | Understood that a model trained strictly on a single lab's topology might fail in production. | We rely entirely on the lab's `conn_dataset`. | Seek supplementary public datasets to diversify the normal baseline and attack vectors. | Search for compatible Kaggle data. |
| Day 2 | 13/06/2026 | 5h | Found public network intrusion datasets and compared their feature distributions to our Zeek schema. | Most public datasets use different flow extractors (not Zeek). | Significant schema mismatch; many public flow features couldn't map to our 30-feature schema. | Discard datasets that cannot be reliably mapped to Zeek's `conn.log` format. | Write a conversion script. |
| Day 3 | 15/06/2026 | 6h | Wrote a script to map compatible public dataset fields to our Zeek schema, filling `-` where necessary. | Successfully appended a subset of public data to our training pool. | Naively merging data caused the Isolation Forest to flag the new normal data as anomalies. | The public dataset's "normal" baseline had completely different rate/ratio features (`bytes_per_pkt`). | Consider domain classification. |
| Day 4 | 16/06/2026 | 5h | Evaluated the AI1 model using the merged dataset. | Observed that generalization is exceptionally difficult across different network domains. | Mixing datasets without care degraded the False Positive Rate on our primary lab data. | Revert to using the lab `conn_dataset` as the primary dataset. Public data is strictly supplementary for testing extreme edge cases. | Document the mismatch findings. |

### Technical Implementation
I investigated the potential of using public datasets (like those from Kaggle) to improve AI1. I recognized that AI1's 30-feature schema is strictly tied to Zeek's `conn.log` extraction logic. Many public datasets provide flow-level features, but their definitions of `duration` or `packet_rate` often differ subtly from Zeek's. After writing a conversion script to standardize a public dataset to our schema, I discovered a severe dataset mismatch. The Isolation Forest, which had perfectly learned the "normal" baseline of our lab's topology, immediately flagged the public dataset's "normal" traffic as highly anomalous. This hands-on experiment validated our initial project strategy: a **Zeek-first dataset strategy** where lab data is the core, rather than blindly relying on generic public datasets. 

### Challenges & Solutions
* **Challenge:** When attempting to merge public data, the feature distributions (especially rate/ratio features like `orig_byte_rate` and `orig_resp_byte_ratio`) were so fundamentally different that the model's False Positive Rate skyrocketed.
* **Solution:** I documented this phenomenon as a classic domain adaptation problem. I concluded that for AI1, we must rely primarily on our internal lab `conn_dataset` for training the normal baseline. Public data should only be used as a supplementary attack testing set, provided it can be safely converted to the Zeek schema without distortion.

### Internship Reflection
This week was a profound lesson in real-world data science. It is easy to assume that "more data is better," but in network security, data is inextricably linked to the topology that generated it. The dataset mismatch experiment solidified my understanding that feature engineering (`feature extractor` consistency) is far more important than just blindly feeding large Kaggle datasets into an algorithm.
