---
title: "Week 11 - Well-Architected Review and AI1 Technical Documentation"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice info %}}
**Week 11 Worklog** - 26/06/2026 - 02/07/2026
{{% /notice %}}

### Weekly Overview
Week 11 moved from experiments toward explanation and documentation. I reviewed the AWS Well-Architected Framework, kept notes from community activities where relevant, and drafted the AI1 technical documentation. The main goal was to describe the expected data flow, input/output assumptions, Zeek-based preprocessing, and model limitations without claiming full end-to-end testing.

### Weekly Objectives
* Review Well-Architected pillars related to reliability, security, and cost.
* Draft AI1 technical documentation for the report website.
* Document input/output assumptions and Zeek preprocessing limitations.
* Prepare a demo-flow description using expected data flow wording.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---|---|---|---|---|---|
| Day 1 | 26/06/2026 | 4h | Studied AWS Well-Architected Framework pillars. | Connected reliability, security, and cost ideas to the internship project. | The framework is broad and easy to summarize too generally. | Select only points relevant to this report. | Review community/event notes. |
| Day 2 | 27/06/2026 | 4h | Recorded useful Community Day or team discussion notes. | Captured a few practical architecture reminders. | Not every event detail belonged in the worklog. | Keep the event summary light. | Draft AI1 documentation. |
| Day 3 | 29/06/2026 | 5h | Drafted AI1 technical documentation: scope, input fields, preprocessing, and output idea. | Created a clearer explanation for readers outside the AI1 task. | Some assumptions were expected rather than fully validated. | Label assumptions clearly in the document. | Add limitations. |
| Day 4 | 30/06/2026 | 4h | Documented limitations around feature extraction, sample size, and threshold review. | Made the AI1 section more defensible. | It was tempting to write a stronger integration story. | Avoid full end-to-end claims. | Prepare demo-flow text. |
| Day 5 | 01/07/2026 | 3h | Prepared expected demo-flow and website content notes. | Aligned report pages with the current project status. | The flow needed to be understandable without overpromising. | Use prepared/reviewed wording for integration steps. | Move to final website review. |

### Technical Implementation
The Well-Architected review helped me frame the project beyond code. Reliability, security, and cost considerations were useful when writing about AWS services and report decisions.

The AI1 documentation focused on clarity: what input is expected, how Zeek-based preprocessing is described, what the model attempts to detect, and where its limitations remain. I described the demo flow as prepared and expected, not as a fully validated production path.

### Challenges & Solutions
* **Challenge:** Documentation could easily sound stronger than the actual integration status.
* **Solution:** I marked assumptions and limitations directly in the AI1 technical notes.

### Internship Reflection
This week showed me the value of documentation as engineering work. A model that is not explained clearly can be hard for teammates to use or review. Writing limitations felt uncomfortable at first, but it made the report more honest. I also learned that architecture frameworks are useful when they guide concrete decisions, not when they are copied generically.
