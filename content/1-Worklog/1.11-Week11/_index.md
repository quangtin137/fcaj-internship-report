---
title: "Week 11 - End-to-End Testing and AI1 Technical Documentation"
date: 2026-06-26
weight: 11
chapter: false
pre: " <b> 1.11. </b> "
---
{{% notice info %}}
📋 **Week 11 Worklog** — 26/06/2026 – 02/07/2026
{{% /notice %}}

### Weekly Overview
This week was about rigorous verification and documentation. I conducted end-to-end tests of the entire data lifecycle across the AWS pipeline and drafted the comprehensive technical documentation for the AI1 module, strictly defining its input and output contracts.

### Weekly Objectives
* Perform end-to-end pipeline testing: Zeek -> S3 -> SQS -> AI1 -> Fusion Layer -> Dashboard.
* Draft the AI1 technical documentation, specifying the exact input and output contracts.
* Document the model limitations, including dependencies on the Zeek schema and the normal baseline.
* Prepare scenarios for the final live attack demo.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 26/06/2026 | 5h | Ran full end-to-end tests by firing Nmap scans at the lab EC2 instances. | Traced the data flow from `conn.log` creation, through SQS batch polling, into the AI1 adapter, and out to the Fusion Layer. | Dashboard visualization was slightly delayed. | Optimize the batch polling frequency to reduce latency before the demo. | Document AI1 contracts. |
| Day 2 | 27/06/2026 | 4h | Participated in FCAJ Community Day - Data Driven, AI Risen. | Gained a broader view of how AI agents can support enterprise operations and security workflows. | Needed to separate high-level event insights from the actual AI1 implementation. | Keep the event report in Event Participated and use only relevant lessons in Worklog reflection. | Apply the AI-driven operations mindset when reviewing the end-to-end pipeline. |
| Day 3 | 29/06/2026 | 5h | Drafted the AI1 Technical Documentation, detailing the `feature_manifest.json` and the 30-feature schema requirement. | Established a clear input contract for any future engineers interacting with AI1. | Need to explicitly define the error handling. | Emphasize the `fail_if_missing` policy as a feature, not a bug, ensuring data integrity. | Document output contract. |
| Day 4 | 30/06/2026 | 4h | Documented the output contract: `label`, `confidence`, and `selected_threshold` (0.398066). | Clearly defined how the Fusion Layer must interpret the `higher_is_more_anomalous` score. | No major issue. | Proceed to document known limitations. | Draft AI1 limitations section. |
| Day 5 | 01/07/2026 | 4h | Documented AI1 limitations regarding its dependency on Zeek updates and the need to retrain if the normal baseline topology changes significantly. | A transparent review of the model's operational boundaries. | No major issue. | Prepare demo scripts. | Finalize demo preparation. |

### Technical Implementation
I focused heavily on end-to-end testing, firing live attacks and tracing the resulting logs through the SQS queue, into the Feature Router, through the AI1 inference adapter, and finally to the Fusion Layer and RDS/Dashboard. Once the pipeline stability was confirmed, I authored the AI1 Technical Documentation. I explicitly defined the input contract: the model strictly requires a 30-feature numeric vector exactly matching the `feature_manifest.json` derived from Zeek `conn.log`, enforcing a `fail_if_missing` policy. I outlined the output contract, specifying that downstream services will receive a JSON payload containing `label` (NORMAL/ANOMALY), `confidence`, and the `selected_threshold`. 

### Challenges & Solutions
* **Challenge:** Articulating the limitations of AI1 without undermining the work was a delicate balancing act for the documentation.
* **Solution:** I framed the limitations as operational prerequisites. For example, rather than saying "the model breaks if the network changes," I documented that "AI1 assumes a stable lab topology baseline; significant architectural changes require re-establishing the normal baseline to prevent elevated false positive rates." This provides clear guidance for future maintainers.

### Internship Reflection
The FCAJ Community Day event highlighted the future of AI-driven operations, which made me appreciate the automated pipeline we've built even more. Writing the technical documentation forced me to step into the shoes of a future developer who might inherit this system. Ensuring that the contracts are strict and the limitations are transparent is the hallmark of professional software engineering, far beyond just running a machine learning script.
