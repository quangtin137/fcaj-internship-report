---
title: "Week 6 - Security Monitoring Services and Data Strategy Revision"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---
{{% notice info %}}
**Week 6 Worklog** - 22/05/2026 - 28/05/2026
{{% /notice %}}

### Weekly Overview
Week 6 was the point where the public dataset limitations became a concrete data-strategy discussion. I studied VPC Flow Logs, GuardDuty, and Security Hub to understand AWS-native security monitoring, then summarized why CICIDS/CICIDS2018 did not fit AI1 well enough for the expected pipeline. The conclusion was to move toward a log source closer to real project telemetry.

### Weekly Objectives
* Study VPC Flow Logs and what network metadata they provide.
* Review GuardDuty and Security Hub roles in AWS security monitoring.
* Summarize CICIDS/CICIDS2018 schema, feature, and label limitations.
* Draft the expected AI1 artifact list and recommend the next data direction.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 22/05/2026 | 4h | Studied VPC Flow Logs fields and use cases. | Understood another AWS source of network-level evidence. | Flow Logs are not the same as Zeek and have different detail levels. | Keep service comparisons factual. | Review GuardDuty and Security Hub. |
| Day 2 | 23/05/2026 | 4h | Read GuardDuty and Security Hub overview material. | Learned how managed findings can support security monitoring. | Managed findings do not replace AI1's anomaly score. | Treat them as separate evidence sources. | Summarize dataset experiment issues. |
| Day 3 | 25/05/2026 | 5h | Collected notes from CICIDS/CICIDS2018 preprocessing and model trials. | Grouped issues into schema mismatch, feature mismatch, and label difficulty. | The same issue appeared in several forms across experiments. | Write the limitation summary by cause, not by notebook. | Discuss data direction. |
| Day 4 | 26/05/2026 | 4h | Discussed why a log source closer to the project pipeline was needed. | Reached a clearer rationale for moving toward Zeek conn.log. | The change needed to be presented as a technical adjustment, not a sudden restart. | Frame it as reducing train-serving mismatch. | Draft artifact expectations. |
| Day 5 | 27/05/2026 | 3h | Drafted an expected AI1 artifact and data-direction note. | Prepared the transition point for Week 7. | Artifacts were only planned, not finalized. | Use planned/expected wording in the report. | Start Zeek schema study next week. |

### Technical Implementation
VPC Flow Logs, GuardDuty, and Security Hub helped me separate different types of security evidence. AWS can provide network metadata and managed findings, but AI1 still needs a consistent feature source for model training and evaluation.

The dataset review ended with a practical concern: train-serving mismatch. A model trained on public dataset features may not behave well if deployment later serves a different log format. This was the reason for recommending Zeek conn.log as the next direction.

### Challenges & Solutions
* **Challenge:** The largest risk was training on one feature definition and serving on another.
* **Solution:** I summarized the mismatch clearly and wrote the Zeek direction as a recommendation rather than a completed migration.

### Internship Reflection
This week helped me understand why changing data strategy can be a responsible technical decision. The public dataset work was not wasted; it revealed what would go wrong if the team ignored serving-time data. AWS security services also showed that monitoring platforms combine many evidence types. AI1 needs to fit into that ecosystem instead of pretending one dataset solves everything.
