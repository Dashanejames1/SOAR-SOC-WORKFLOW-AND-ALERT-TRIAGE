# SOAR-SOC-WORKFLOW-AND-ALERT-TRIAGE



**Author:** Dashane James  
**Lab Environment:** VMware Workstation | Kali Linux | Metasploitable 2
**Purpose:** The goal of this repository is to build the SOC workflow reasoning an analyst needs — how SOAR playbooks automate response, how detection and response time are measured, how alerts are triaged, and how work flows across SOC tiers.
**Status:** 🔵 Completed

---

## 📋 Overview

This repository documents how a modern SOC operates and where automation fits. It covers SOAR playbooks and what triggers them, the two core SOC effectiveness metrics (MTTD and MTTR), the four-way alert triage classification, the roles of SOC Tiers 1–3, and a worked design of a brute-force response playbook from detection through notification.

---


---

## 🔬 Tasks / Assessments Performed

### 1. Reviewing SOAR playbooks, what triggers them, and what they automate.

SOAR stands for Security Orchestration, Automation, and Response. A playbook is a predefined, automated workflow that executes a series of response actions when a specific trigger condition is met, removing the need for a human to manually perform repetitive Tier 1 tasks.

What triggers a playbook: An alert from a SIEM, EDR, IDS, or threat intel feed that matches a defined condition — a failed login threshold, a known malicious IP hitting the firewall, a phishing email reported by a user, or a file hash matching a threat intel indicator.

What a playbook automates:

Enrichment — querying threat intel platforms (VirusTotal, Shodan) to add context to an alert automatically
Containment — isolating an endpoint, blocking an IP at the firewall, disabling a user account
Ticketing — creating an incident ticket in the ITSM system with all relevant context pre-populated
Notification — alerting the on-call analyst or escalating to Tier 2 with a summary

Exam tip: “500 alerts/day, analyst overwhelmed — what should they implement?” → SOAR with automated playbooks. This is a guaranteed question.


# Output


**Findings:**


### 2. Reviewing MTTD (Mean Time to Detect) and MTTR (Mean Time to Respond).

These are the two primary metrics used to measure SOC effectiveness.

MTTD — Mean Time to Detect
The average time between when an attack begins and when the SOC identifies it. A high MTTD means attackers are dwelling in the environment undetected for too long. Reduced by better detection coverage (SIEM rules, EDR, NDR), threat hunting, and improved log ingestion.

MTTR — Mean Time to Respond
The average time between detection and full containment/remediation. A high MTTR means the SOC is slow to act after detecting an incident. Reduced by SOAR automation, pre-approved response playbooks, and clear escalation paths.

Why both matter: An attacker who dwells for 30 days before detection causes far more damage than one detected in 30 minutes. MTTD measures how fast you see it; MTTR measures how fast you stop it.

Exam tip: If a question asks how to reduce analyst response time or scale a SOC without adding headcount → SOAR reduces MTTR. Better detection rules and log coverage reduce MTTD.


# Output



**Findings:**


### 3. Review of alert triage: True positive, false positive, true negative, and false negative.

Every alert a SOC analyst reviews falls into one of four categories:

Classification	What It Means	Example
True Positive (TP)	Alert fired and the threat is real	Brute force alert triggers — attacker is actually attempting login
False Positive (FP)	Alert fired but no real threat exists	Port scanner alert fires because the IT team ran an authorized scan
True Negative (TN)	No alert fired and there was no threat	Normal web traffic passes through — no alert, no attack
False Negative (FN)	No alert fired but a real threat occurred	Attacker exfiltrates data over an allowed port — no rule catches it

The two dangerous ones: False Positives waste analyst time and cause alert fatigue. False Negatives are worse — the attack happened and nobody knew. Reducing FNs requires better detection coverage; reducing FPs requires tuning alert thresholds and adding context.



# Output


```

**Findings:**



### 4. Reviewing SOC tiers: Tier 1 = triage, Tier 2 = investigation, and Tier 3 = threat hunting.

Tier 1 — Triage Analyst
First responder to alerts. Reviews incoming alerts, classifies them (TP/FP), performs initial enrichment, and either closes false positives or escalates confirmed incidents to Tier 2. High volume, time-pressured work. SOAR automates most of this tier.

Tier 2 — Incident Responder / Investigator
Receives escalated incidents from Tier 1. Performs deeper investigation — timeline reconstruction, log correlation, malware analysis, attacker TTP mapping. Drives containment and remediation decisions.

Tier 3 — Threat Hunter / Senior Analyst
Proactively searches the environment for threats that haven’t triggered any alerts. Works from threat intelligence and hypothesis-driven hunting. Also handles the most complex incidents escalated from Tier 2, and tunes detection rules to reduce FPs and FNs across the SOC.

Exam tip: SOAR automates Tier 1 so human analysts can focus on Tier 2 investigation. A question about analyst burnout or scaling the SOC → implement SOAR.

# Output



**Findings:** 

### 5. Designing a SOAR playbook for brute force: detect -> isolate -> ticket -> notify.

Trigger: SIEM detects 10+ failed login attempts against a single account within 5 minutes.

Step 1 — Detect
SIEM rule fires. Alert is sent to SOAR with the source IP, target username, timestamp, and failed attempt count.

Step 2 — Enrich
SOAR automatically queries threat intel (VirusTotal, AbuseIPDB) for the source IP. Checks if the targeted account is privileged. Pulls geolocation of the source IP.

Step 3 — Isolate / Contain
If IP is flagged as malicious or attempt count exceeds threshold → SOAR automatically blocks the source IP at the firewall and temporarily locks the targeted account.

Step 4 — Ticket
SOAR creates an incident ticket in the ITSM system with all enrichment data, the SIEM alert, and the automated actions already taken — pre-populated for the Tier 2 analyst.

Step 5 — Notify
SOAR sends an alert to the on-call Tier 2 analyst via email/Slack with a summary and a link to the ticket.

### Output



## 🔑 Key Takeaways
SOAR is the default answer for alert fatigue, analyst burnout, or scaling a SOC without headcount — it automates Tier 1 and reduces MTTR.
MTTD vs. MTTR split: detection coverage lowers MTTD; automation lowers MTTR. Exam questions hinge on which one the scenario is asking about.
False Negatives are the most dangerous triage outcome — a real attack no one saw. Alert tuning fixes FPs; detection coverage fixes FNs.
```

---

## 📌 About This Project

This repository is the SOC-operations module of my cybersecurity portfolio. Where the hands-on labs build individual detection and response skills, this one frames how those skills fit into a real SOC.


**Related repositories:**

Incident-Response-Report — Worked a confirmed compromise through the full IR lifecycle

Full-Kill-Chain-Red-and-Blue-Teams — Ties attack and detection together end to end

Vulnerability-Management-Deep-Review — Scoring, validating, and prioritizing vulnerabilities

Zero-Trust-Architecture-Identity-Security — Architecture and identity controls that prevent lateral movement

---

## 👤 Author

**Dashane James**  
Senior Field Service Technician → Cybersecurity Analyst  
📍 Yonkers, NY  
🎓 B.S. Information Technology — SUNY Canton  
🏆 CompTIA Security+ | CySA+ (In Progress)  
🔗 [GitHub](https://github.com/Dashanejames1) | [Zero Trust Cyber Security Brand](https://www.instagram.com/zerotrust_cybersecurity)

---

*This repository is part of an active portfolio demonstrating hands-on cybersecurity skills. All lab work performed in isolated environments for educational purposes.*0
