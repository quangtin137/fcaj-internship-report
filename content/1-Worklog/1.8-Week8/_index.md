---
title: "Week 8 - Zeek Feature Extraction and Containerization Study"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---
{{% notice info %}}
**Week 8 Worklog** - 05/06/2026 - 11/06/2026
{{% /notice %}}

### Weekly Overview
Week 8 continued the Zeek direction with concrete feature extraction work. I also studied Docker and ECS to understand how an AI Engine might later be packaged. The model work stayed experimental: I created basic and derived Zeek-based features, ran an initial Isolation Forest retraining attempt, and reviewed the expected output format with the team.

### Weekly Objectives
* Review Docker basics and ECS concepts for containerized AI components.
* Implement or adjust Zeek feature extraction for selected conn.log fields.
* Create basic derived features from byte counts, duration, and categorical fields.
* Run an initial retraining experiment and review expected AI1 output format.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 05/06/2026 | 4h | Studied Docker images, containers, and why services are packaged. | Understood the direction for a future AI Engine container. | Containerization details could distract from unfinished feature work. | Keep Docker notes high-level for now. | Continue Zeek extraction. |
| Day 2 | 06/06/2026 | 5h | Started extracting selected fields from Zeek conn.log samples. | Created a first feature table from connection records. | Some rows had missing service or byte values. | Apply the parsing rules drafted in Week 7. | Add simple derived features. |
| Day 3 | 08/06/2026 | 4h | Created derived features such as byte ratios and duration-related values. | Improved the feature set beyond raw columns. | Derived values could break when denominator fields were zero or missing. | Add guard logic for unsafe calculations. | Run a small retraining test. |
| Day 4 | 09/06/2026 | 5h | Ran an initial Isolation Forest retraining attempt on Zeek-based features. | Got early score outputs for inspection. | The sample size and feature stability were still limited. | Call the result an initial experiment only. | Review output expectations with the team. |
| Day 5 | 10/06/2026 | 3h | Reviewed expected AI1 output fields with the team. | Aligned on the kind of label/score information downstream components may need. | The integration contract was not fully finalized. | Prepare an output format draft rather than claiming final integration. | Organize experiment notes. |

### Technical Implementation
Docker and ECS were studied to understand deployment shape, not to claim a finished container. This helped me see what files, dependencies, and runtime assumptions an AI Engine would eventually need.

The Zeek feature extractor began turning conn.log records into model inputs. Basic numeric fields were combined with guarded derived features, while categorical fields were kept explicit so their encoding could be reviewed later.

### Challenges & Solutions
* **Challenge:** Derived features worked in notebooks but were not automatically safe for integration.
* **Solution:** I added simple guards and kept the output format as a draft for team review.

### Internship Reflection
This week made feature engineering feel like the center of AI1. The model could only be as useful as the fields and transformations behind it. Docker/ECS also reminded me that reproducibility matters once code leaves a notebook. I became more careful about writing down assumptions, especially for derived features.
