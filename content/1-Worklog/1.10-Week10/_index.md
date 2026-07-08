---
title: "Week 10 - CloudTrail, Threshold Review and Cloud Architect Event"
date: 2026-06-19
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---
{{% notice info %}}
**Week 10 Worklog** - 19/06/2026 - 25/06/2026
{{% /notice %}}

### Weekly Overview
Week 10 combined audit-log study, community learning, and AI1 threshold review. I reviewed CloudTrail to understand AWS API activity records, joined or recorded notes from the Cloud Architect event, then inspected candidate thresholds and false positives from the Zeek-based experiments. Explainability was considered as an offline study topic rather than a finished integration.

### Weekly Objectives
* Study CloudTrail and the value of audit logs.
* Record useful lessons from the Cloud Architect event.
* Review candidate thresholds and score distributions for AI1.
* Inspect false positives and review SHAP as a possible offline explainability method.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 19/06/2026 | 4h | Studied CloudTrail events and audit-log use cases. | Understood how API actions can be reviewed after they happen. | Audit logs are different from network telemetry. | Keep CloudTrail in the audit/evidence category. | Prepare event notes. |
| Day 2 | 20/06/2026 | 4h | Participated in or summarized the Cloud Architect event. | Collected architecture and operational points useful for the report. | Event notes needed to stay concise. | Keep only points related to internship learning. | Review AI1 thresholds. |
| Day 3 | 22/06/2026 | 5h | Compared candidate threshold values against score distributions. | Saw how different cutoffs changed anomaly counts. | A precise number could be mistaken for a finalized threshold. | Use candidate threshold wording. | Inspect false positives. |
| Day 4 | 23/06/2026 | 4h | Reviewed false positive examples from Zeek-based scores. | Found records that required context before being called abnormal. | Score alone did not explain why a record was suspicious. | Add false-positive notes to evaluation. | Read about explainability options. |
| Day 5 | 24/06/2026 | 3h | Reviewed SHAP as a possible offline explainability approach. | Understood the idea without claiming SHAP plots were produced. | Explainability could be overclaimed easily. | Describe it as explored/reviewed only. | Move to documentation preparation. |

### Technical Implementation
CloudTrail added an audit perspective to the AWS learning path. Unlike network logs, it records API activity, which is useful for accountability and investigation.

For AI1, I compared candidate thresholds with score distributions and false-positive examples. I kept the threshold discussion experimental and treated SHAP as a possible offline explanation method, not an implemented dashboard feature.

### Challenges & Solutions
* **Challenge:** Candidate thresholds could easily be presented as final decisions.
* **Solution:** I paired each threshold note with uncertainty, false-positive review, and the need for broader validation.

### Internship Reflection
This week showed me that threshold tuning is partly technical and partly communicative. If the report presents a candidate as final, readers may trust the system more than they should. CloudTrail also reminded me that security work often depends on evidence after the fact. Explainability and auditability became more important in how I described AI1.
