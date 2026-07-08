---
title: "Week 1 - Internship Onboarding and AI1 Scope Analysis"
date: 2026-04-17
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---
{{% notice info %}}
**Week 1 Worklog** - 17/04/2026 - 23/04/2026
{{% /notice %}}

### Weekly Overview
The first week was mainly about entering the internship rhythm: understanding the AWS account environment, reviewing IAM and Budgets, and reading the project proposal carefully enough to locate my AI1 responsibility. At this stage, Zeek was only one possible telemetry direction in the architecture, so I focused on defining the AI1 boundary instead of assuming the final dataset too early.

### Weekly Objectives
* Complete onboarding and review safe AWS account usage with IAM and Budgets.
* Read the Hybrid IDS project proposal and identify where AI1 fits.
* Separate AI1's anomaly detection scope from AI2A, AI2B, and Suricata.
* List open questions about data sources and model direction for the next weeks.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 17/04/2026 | 3h | Completed onboarding, checked AWS account access, and reviewed IAM/Budgets notes. | Understood the basic account boundaries and cost-control expectations. | I was still unfamiliar with the team's AWS naming and access conventions. | Keep a short account-safety checklist before doing labs. | Review the project proposal in more detail. |
| Day 2 | 18/04/2026 | 4h | Read the Hybrid IDS proposal and marked the parts related to AI1. | Identified AI1 as the network anomaly detection component. | The proposal mentioned several evidence sources, so the AI roles were not immediately obvious. | Treat AI1 as an unsupervised anomaly module while the data source is still being studied. | Clarify AI1 boundaries with the team. |
| Day 3 | 20/04/2026 | 4h | Discussed AI1, AI2A, AI2B, and Suricata responsibilities with the team. | Reduced overlap between ML-based anomaly detection and rule-based alerting. | Suricata alerts initially looked close to AI1 outputs. | AI1 should not duplicate Suricata; it should provide a separate anomaly signal. | Prepare initial notes about possible telemetry sources. |
| Day 4 | 21/04/2026 | 4h | Summarized AI1 scope, AWS onboarding notes, and unanswered dataset questions. | Created a clearer starting point for Week 2 exploration. | The final dataset direction was not ready to confirm. | Use careful wording in the report and keep Zeek as a potential direction only. | Start AWS networking practice and dataset survey. |

### Technical Implementation
On the AWS side, I reviewed IAM users/roles, permission boundaries at a beginner level, and Budgets as a guardrail for hands-on practice. This was useful because most later labs would require me to understand what I was allowed to create, change, or delete.

For AI1, I focused on the project proposal rather than implementation. The main output of the week was a scope note: AI1 should contribute network anomaly evidence, while AI2A, AI2B, and Suricata cover different evidence types. I avoided writing that Zeek conn.log had already become the main dataset.

### Challenges & Solutions
* **Challenge:** AI1's boundary was easy to blur with Suricata and the other AI modules.
* **Solution:** I clarified the role split with the team and recorded AI1 as an anomaly-detection component whose data source still needed validation.

### Internship Reflection
This week taught me that defining the role correctly is part of engineering work, not just administration. If AI1 is described too broadly, the later model work can become hard to explain. I also realized that early project documents are starting points, not final implementation evidence. That helped me write the worklog more carefully from the beginning.
