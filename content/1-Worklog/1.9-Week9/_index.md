---
title: "Week 9 - SageMaker Study and Zeek-Based Model Evaluation"
date: 2026-06-12
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---
{{% notice info %}}
**Week 9 Worklog** - 12/06/2026 - 18/06/2026
{{% /notice %}}

### Weekly Overview
Week 9 focused on model lifecycle thinking and cautious evaluation of the new Zeek-based direction. I studied SageMaker concepts at a high level, organized the latest experiment outputs, and reviewed early metrics such as Recall, F1, and false positive rate. The numbers were useful for discussion, but still treated as experimental because sample selection and thresholds were not mature.

### Weekly Objectives
* Review SageMaker and model lifecycle concepts.
* Organize Zeek-based experiment outputs for easier comparison.
* Evaluate early Recall, F1, and false positive rate at experimental level.
* Review sensitivity caused by sample selection and threshold assumptions.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 12/06/2026 | 4h | Studied SageMaker concepts such as training jobs, model artifacts, and experiment tracking. | Understood how managed ML services organize model lifecycle. | The project did not require full SageMaker implementation at this point. | Keep the study conceptual and connect it to documentation. | Organize Zeek experiment outputs. |
| Day 2 | 13/06/2026 | 4h | Grouped Zeek-based experiment results and score files. | Made it easier to compare runs. | Some file names did not clearly show feature versions. | Add version notes in filenames or report text. | Calculate early metrics. |
| Day 3 | 15/06/2026 | 5h | Reviewed Recall, F1, and false positive rate from test samples. | Got a first view of model behavior after switching to Zeek features. | Metrics changed when sample composition changed. | Present them as experimental indicators. | Inspect false positives. |
| Day 4 | 16/06/2026 | 4h | Looked at false positives and sample-selection sensitivity. | Identified cases where normal-looking records could receive high anomaly scores. | There was not enough evidence to set a final threshold. | Keep threshold tuning for review in Week 10. | Update report notes. |
| Day 5 | 17/06/2026 | 3h | Updated worklog/report notes with lessons from public dataset mismatch and Zeek evaluation. | Connected earlier dataset lessons to the new approach. | The narrative needed to avoid sounding like a complete model validation. | Use cautious evaluation language. | Move to CloudTrail and threshold review. |

### Technical Implementation
SageMaker was reviewed as a model lifecycle reference: training jobs, artifacts, versions, and experiment tracking. Even without implementing it fully, the concepts helped me organize AI1 outputs more responsibly.

The Zeek-based evaluation used early metrics only as indicators. Recall, F1, and false positive rate were reviewed together with sample-selection notes so that the report would not overstate model quality.

### Challenges & Solutions
* **Challenge:** Early metrics were sensitive to sample selection and threshold assumptions.
* **Solution:** I reported them as experimental evaluation results and kept the limitations beside the numbers.

### Internship Reflection
This week taught me to be careful with metrics. A number can look objective, but it still depends on how the sample was built and how the threshold was chosen. SageMaker concepts also helped me think beyond a single notebook run. I started seeing AI1 as something that needs versioned experiments and clear interpretation.
