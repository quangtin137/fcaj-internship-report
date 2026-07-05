---
title: "Week 5 - Attack Traffic Collection and Labeling Strategy"
date: 2026-05-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---
{{% notice info %}}
📋 **Week 5 Worklog** — 15/05/2026 – 21/05/2026
{{% /notice %}}

### Weekly Overview
This week was dedicated to generating and labeling attack traffic. Since AI1 is an unsupervised anomaly detector, this attack data is not used for training but is strictly required to evaluate the model and properly freeze the decision threshold.

### Weekly Objectives
* Simulate active network attacks (Port Scan, Brute Force) in the lab.
* Create a strict attack diary to track timestamps.
* Develop a labeling strategy based on timestamp windows and session UIDs.
* Use the labeled attack data to evaluate AI1 without turning it into a supervised attack classifier.

### Daily Writing
| Day | Date | Time Spent | Work Completed | Result | Issue | Decision | Next Step |
|---|---|---:|---|---|---|---|---|
| Day 1 | 15/05/2026 | 5h | Executed Nmap Port Scans and Hydra Brute Force attacks against the lab servers. | Generated realistic attack traffic interwoven with normal traffic. | Attack traffic alone is unrealistic. | Inject attacks while normal HTTP/DNS traffic is also actively running. | Create the attack diary. |
| Day 2 | 16/05/2026 | 4h | Compiled the attack diary detailing the start/end timestamps of every attack vector. | A precise record of when anomalies occurred in the network. | Slight clock desynchronization between attacker and server. | Use a ±2 second timestamp window for labeling to account for drift. | Implement labeling script. |
| Day 3 | 18/05/2026 | 5h | Wrote a Python script to merge the attack diary with the Zeek `conn.log` to assign labels. | Dataset now contains explicit labels for evaluation purposes. | Some long-lived normal connections overlapped with the attack window. | Refine labeling using source IPs and destination ports in addition to timestamps. | Evaluate AI1 against labeled data. |
| Day 4 | 19/05/2026 | 6h | Evaluated the AI1 Isolation Forest against the newly labeled dataset. | Observed how the `anomaly_score` reacts to actual attacks vs. normal spikes. | AI1 does not classify the attack type, only the anomaly presence. | Re-emphasize that AI2A is the network attack classifier, not AI1. | Prepare to freeze AI1 artifacts. |

### Technical Implementation
I generated synthetic attacks using tools like Nmap and Hydra. To mimic a real production environment, these attacks were fired while normal baseline traffic was continuously running. I kept a meticulous attack diary and wrote a script to label the resulting `conn.log` data based on timestamp windows, source IPs, and session UIDs. Once labeled, I ran the baseline Isolation Forest model (which only trained on normal data) against this new dataset. I paid special attention to how derived features like `ssh_non_success_60s` reacted during Brute Force attempts, observing the subsequent spikes in the `anomaly_score`.

### Challenges & Solutions
* **Challenge:** It was tempting to use the newly generated attack labels to train a supervised classifier, blurring the line between AI1 and AI2A.
* **Solution:** I strictly adhered to the architecture outline. AI1 must remain an unsupervised anomaly detector. The attack labels were exclusively used to evaluate the False Positive Rate and Detection Rate, ensuring AI1 only outputs `NORMAL` or `ANOMALY` + `confidence`. This maintains a clean separation of concerns before the decision-level fusion.

### Internship Reflection
This week clarified the distinction between anomaly detection and threat classification. Seeing the `anomaly_score` spike precisely when my Nmap scans hit the server was highly satisfying. It proved that our Zeek-first dataset strategy and rigorous feature engineering were paying off.
