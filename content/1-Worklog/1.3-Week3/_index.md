---
title: "Week 3 - S3/RDS Study and Initial Data Preprocessing Experiments"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---
{{% notice info %}}
**Week 3 Worklog** - 01/05/2026 - 07/05/2026
{{% /notice %}}

### Weekly Overview
This week connected storage concepts with the first hands-on data checks. I studied S3 for organizing experiment files, reviewed where RDS/PostgreSQL could fit in storing structured results, and started loading CICIDS/CICIDS2018 samples with pandas. The main AI1 lesson was that preprocessing quality matters before any model result can be trusted.

### Weekly Objectives
* Understand how S3 can store raw datasets and experiment outputs.
* Review the role of RDS/PostgreSQL for structured security or model results.
* Load CICIDS/CICIDS2018 samples with pandas and inspect columns.
* Check missing values, label distribution, and feature types before modeling.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 01/05/2026 | 4h | Studied S3 bucket concepts and organized local experiment folders. | Separated raw data, cleaned data, and notes more clearly. | File naming was inconsistent at first. | Use simple folder prefixes for raw, processed, and reports. | Review structured storage options. |
| Day 2 | 02/05/2026 | 4h | Reviewed RDS/PostgreSQL concepts for storing records and results. | Understood why relational storage may help with queryable detection history. | It was not yet clear what final result schema would be needed. | Keep RDS notes conceptual for now. | Load dataset samples in Python. |
| Day 3 | 04/05/2026 | 5h | Loaded CICIDS/CICIDS2018 samples with pandas and inspected column names. | Confirmed that the dataset has many numeric flow features. | Some columns had spaces and naming differences that could cause script errors. | Normalize column names only inside exploratory notebooks. | Check missing values and labels. |
| Day 4 | 05/05/2026 | 4h | Checked missing values, feature types, and label distribution. | Found imbalance between benign and attack-related rows. | Raw metrics could be misleading if class imbalance is ignored. | Document imbalance before evaluating any model. | Summarize preprocessing limitations. |
| Day 5 | 06/05/2026 | 3h | Wrote notes about early preprocessing limitations and dataset assumptions. | Prepared a clearer basis for Week 4 model experiments. | Not every public dataset feature looked reusable for serving. | Treat cleaning code as exploratory until the data direction is clearer. | Try a small Isolation Forest experiment. |

### Technical Implementation
S3 was useful to think about the lifecycle of experiment files: raw input, processed samples, notebooks, and report evidence. RDS/PostgreSQL was reviewed as a possible place for structured outputs, but I did not assume the final schema yet.

The pandas checks showed that public cybersecurity datasets require careful preparation. Missing values, label imbalance, and feature type conversion can change how later model outputs are interpreted.

### Challenges & Solutions
* **Challenge:** Label imbalance and mixed feature types made early results easy to misread.
* **Solution:** I documented data issues first and postponed strong model conclusions until preprocessing was clearer.

### Internship Reflection
I used to think a large dataset would make the modeling step easier. This week showed the opposite: a large dataset can create more confusion if its labels, feature types, and missing values are not understood. The AWS storage topics also made me think about evidence organization, not just training code. Good preprocessing notes became part of the report, not a side task.
