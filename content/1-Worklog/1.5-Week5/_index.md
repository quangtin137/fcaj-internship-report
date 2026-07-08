---
title: "Week 5 - CloudWatch Practice and Public Dataset Limitation Review"
date: 2026-05-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice info %}}
**Week 5 Worklog** - 15/05/2026 - 21/05/2026
{{% /notice %}}

### Weekly Overview
Week 5 shifted attention to monitoring and scaling concepts while the AI1 dataset concern became more visible. I reviewed EC2 Auto Scaling and CloudWatch Alarm behavior, then returned to the public dataset experiments to examine why attack labels such as Port Scan and Brute Force could not directly prove an AI1 deployment path.

### Weekly Objectives
* Review EC2 Auto Scaling concepts and scaling signals.
* Create or inspect CloudWatch Alarm conditions.
* Continue cleaning public dataset fields and reviewing labels.
* Document why attack categories in public datasets are not the same as deployable AI1 evidence.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 15/05/2026 | 4h | Studied EC2 Auto Scaling groups and scaling policies. | Understood how capacity can change based on conditions. | It was easy to focus on scaling mechanics without linking them to monitoring. | Read scaling together with CloudWatch metrics. | Review alarms next. |
| Day 2 | 16/05/2026 | 4h | Reviewed CloudWatch metrics and alarm states. | Learned how thresholds can trigger operational reactions. | Alarm thresholds are simple to configure but can be noisy. | Treat threshold choice as something that needs evidence. | Return to dataset labels. |
| Day 3 | 18/05/2026 | 4h | Continued cleaning public dataset columns and checking label values. | Found more inconsistencies in naming and category granularity. | Some labels are too specific for unsupervised anomaly detection. | Keep label analysis separate from model scoring. | Study attack categories carefully. |
| Day 4 | 19/05/2026 | 5h | Reviewed Port Scan and Brute Force categories as behavior references. | Understood the behaviors better without claiming generated Zeek traffic. | Public labels do not equal a real deployment data source. | Use them for learning, not for proving the final pipeline. | Write dataset limitation notes. |
| Day 5 | 20/05/2026 | 3h | Documented dataset limitation findings for the report. | Created a clearer argument for why another data strategy may be needed. | The limitation discussion needed careful wording. | State observations as limitations, not failures of the project. | Prepare for security monitoring services. |

### Technical Implementation
CloudWatch and Auto Scaling showed how operational decisions depend on measured signals. That was a useful parallel for AI1: a threshold is easy to write down, but it should not be trusted without understanding noise and context.

The AI1 work stayed with public datasets, especially label review and cleaning. Port Scan and Brute Force were treated as behavior categories for study, not as evidence that a Zeek-based attack lab had already been built.

### Challenges & Solutions
* **Challenge:** Attack labels in public datasets looked useful but did not directly translate to a deployable AI1 pipeline.
* **Solution:** I described them as references for behavior analysis and kept the deployment limitation visible in the report.

### Internship Reflection
This week changed how I viewed public datasets. They are valuable for learning vocabulary and attack behavior, but they should not be used mechanically. CloudWatch also helped me think about false alarms and thresholds from an operational angle. That made the AI1 limitation discussion feel more practical, not just theoretical.
