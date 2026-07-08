---
title: "Week 7 - Transition to Zeek conn.log and AI1 Preprocessing Redesign"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---
{{% notice info %}}
**Week 7 Worklog** - 29/05/2026 - 04/06/2026
{{% /notice %}}

### Weekly Overview
Week 7 marked the actual transition from public dataset exploration to Zeek conn.log. Alongside Lambda and event-driven concepts, I began reading the Zeek schema and identifying fields that could support AI1 preprocessing. The work was about redesigning the data preparation path, not integrating a finished inference pipeline.

### Weekly Objectives
* Review Lambda and event-driven processing concepts.
* Read Zeek conn.log structure and common field meanings.
* Identify usable AI1 fields such as duration, orig_bytes, resp_bytes, proto, service, and conn_state.
* Define initial handling for missing values and categorical fields.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 29/05/2026 | 4h | Studied Lambda and event-driven design patterns. | Understood why small functions can react to events in a pipeline. | It was not yet the right time to wire AI1 into events. | Keep Lambda study conceptual this week. | Start Zeek schema reading. |
| Day 2 | 30/05/2026 | 5h | Read Zeek conn.log documentation and sample rows. | Recognized the fields most relevant to connection-level behavior. | The schema was different from public dataset columns. | Build a new preprocessing view instead of reusing public dataset assumptions. | Inspect key numeric and categorical fields. |
| Day 3 | 01/06/2026 | 4h | Reviewed duration, orig_bytes, resp_bytes, proto, service, and conn_state. | Separated numeric features from categorical fields. | Some values appeared as '-' rather than normal missing values. | Treat '-' explicitly during parsing. | Draft missing-value rules. |
| Day 4 | 02/06/2026 | 4h | Defined initial missing-value and categorical encoding notes. | Created a more realistic starting point for Zeek-based preprocessing. | Categorical handling could easily introduce train-serving mismatch if inconsistent. | Keep transformation rules simple and documented. | Prepare feature extractor plan. |
| Day 5 | 03/06/2026 | 3h | Outlined a Zeek-based preprocessing plan for AI1. | Prepared concrete tasks for feature extraction in Week 8. | The plan still needed validation with actual samples. | Mark it as a draft design. | Begin feature extraction work. |

### Technical Implementation
Lambda was studied as part of event-driven architecture, but the week did not claim SQS-to-AI1 integration. The main value was understanding how future components might be triggered once the data contract becomes clearer.

The Zeek work focused on parsing and feature planning. Fields such as duration and byte counts can become numeric inputs, while proto, service, and conn_state require categorical handling. Missing '-' values were recorded as a preprocessing concern from the start.

### Challenges & Solutions
* **Challenge:** Zeek uses '-' and several categorical fields, which are easy to mishandle in preprocessing.
* **Solution:** I wrote explicit parsing notes and delayed model claims until the feature extractor could be tested.

### Internship Reflection
The move to Zeek made the AI1 pipeline feel closer to a real system. Instead of adapting the project to a public dataset, I started adapting preprocessing to the logs the project expected to use. That also made the work more demanding because every field needed a clear meaning. I learned that a better data source still requires careful engineering before modeling.
