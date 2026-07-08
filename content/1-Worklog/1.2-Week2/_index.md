---
title: "Week 2 - AWS VPC/EC2 Practice and Public Dataset Exploration"
date: 2026-04-24
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---
{{% notice info %}}
**Week 2 Worklog** - 24/04/2026 - 30/04/2026
{{% /notice %}}

### Weekly Overview
Week 2 moved from onboarding into basic AWS networking practice. I reviewed VPC, subnet, route table, security group, EC2, SSH, and IAM Role concepts while also starting a public cybersecurity dataset survey. The AI1 work was still exploratory, centered on understanding CICIDS/CICIDS2018 metadata, labels, and feature meanings.

### Weekly Objectives
* Review the relationship between VPC, subnet, route table, and security group.
* Launch or inspect an EC2 instance and connect through SSH safely.
* Read CICIDS/CICIDS2018 dataset descriptions, labels, and attack categories.
* Create a first comparison note between public dataset fields and AI1's anomaly detection need.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 24/04/2026 | 4h | Reviewed VPC, subnet, route table, and security group concepts. | Understood how network isolation is represented in AWS. | Some routing concepts were easy to mix up at first. | Draw a small VPC diagram before changing settings. | Practice EC2 access next. |
| Day 2 | 25/04/2026 | 4h | Launched or inspected an EC2 instance and reviewed SSH access requirements. | Connected the AWS networking theory with a real compute resource. | Security group inbound rules needed careful checking. | Allow only the required SSH access for the lab context. | Move to public dataset reading. |
| Day 3 | 27/04/2026 | 5h | Read CICIDS/CICIDS2018 metadata, label descriptions, and attack categories. | Built a first understanding of the dataset structure. | The dataset has many engineered flow features whose origin is not always obvious. | Record unclear fields instead of assuming they map to the project pipeline. | Compare features with AI1 needs. |
| Day 4 | 28/04/2026 | 4h | Compared public dataset fields with a network anomaly detection use case. | Noted that public dataset labels are useful for learning but may not fit serving-time logs. | Feature definitions differed from the log-based direction the project might use later. | Keep public datasets as exploration material, not final pipeline input. | Continue with storage and preprocessing in Week 3. |

### Technical Implementation
The AWS work centered on how a small cloud network is assembled. VPC and subnet concepts gave me the boundary of the environment, route tables explained traffic direction, and security groups showed how access rules are enforced around EC2.

For AI1, I began with dataset documentation instead of model training. CICIDS/CICIDS2018 provided useful examples of flow-based security data, but I also noticed that public dataset fields are not automatically compatible with a future log-based pipeline.

### Challenges & Solutions
* **Challenge:** Public dataset documentation had many feature names, but not all of them mapped cleanly to the project's expected data flow.
* **Solution:** I created a comparison note and marked uncertain fields for later review instead of forcing a schema match.

### Internship Reflection
This week made cloud networking feel less abstract. EC2 access depends on VPC design, routing, and security rules, so security monitoring cannot be separated from infrastructure knowledge. On the AI1 side, I learned to read dataset documentation with caution. A dataset can look rich but still be difficult to use if its features do not match the deployment context.
