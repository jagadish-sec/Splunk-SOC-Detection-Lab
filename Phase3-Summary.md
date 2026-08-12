# Phase 3 – Threat Hunting

## Overview

Phase 3 focused on proactive threat hunting using the Splunk **BOTS v1** dataset. Rather than relying on alerts alone, the investigation correlated multiple telemetry sources—including Suricata IDS alerts, HTTP traffic, IIS web logs, and Windows Security events—to determine whether suspicious network activity resulted in a successful compromise.

The objective was to follow a structured analyst workflow: identify suspicious activity, pivot across data sources, validate findings, and document evidence before reaching a conclusion.

---

## Hunting Methodology

The investigation followed a progressive workflow similar to a real Security Operations Center (SOC):

```
Suricata IDS Alerts
        │
        ▼
Identify Targeted Hosts
        │
        ▼
Investigate Web Application Activity
        │
        ▼
Correlate Windows Authentication
        │
        ▼
Analyze Web Server Telemetry
        │
        ▼
Assess Evidence & Determine Impact
```

---

## Threat Hunts Completed

| Hunt | Objective | Status |
|------|-----------|--------|
| Hunt 01 | Pivot from IDS alerts to identify targeted systems | ✅ |
| Hunt 02 | Investigate Joomla administrator interface activity | ✅ |
| Hunt 03 | Correlate Windows authentication events | ✅ |
| Hunt 04 | Analyze web server telemetry and HTTP activity | ✅ |
| Hunt 05 | Consolidate findings into a final investigation report | ✅ |

---

## Telemetry Sources

Throughout this phase, the following data sources were correlated:

- Suricata IDS
- HTTP Stream Logs
- IIS Web Logs
- Windows Security Event Logs
- Splunk Indexed Events

---

## Key Findings

### Network Activity

- Identified multiple Suricata IDS alerts targeting internal web infrastructure.
- Pivoted from IDS alerts to investigate affected systems.
- Identified repeated activity originating from external IP addresses.

### Web Investigation

- Joomla administrator interface was repeatedly accessed.
- GET and POST requests dominated administrator activity.
- HTTP analysis revealed a mixture of successful responses, redirects, client errors, and server errors.

### Authentication Analysis

- Reviewed Windows Event IDs 4624 and 4625.
- Observed machine account authentication events.
- No evidence of interactive administrator logons.
- No failed Windows authentication attempts during the investigated timeframe.

### Correlation

Evidence from IDS alerts, web logs, and Windows Security logs was correlated to reconstruct attacker activity and determine whether the observed web activity resulted in host compromise.

---

## MITRE ATT&CK Coverage

| Tactic | Technique | ATT&CK ID |
|---------|-----------|-----------|
| Reconnaissance | Active Scanning | T1595 |
| Initial Access | Exploit Public-Facing Application *(Potential)* | T1190 |
| Credential Access | Brute Force *(Investigated)* | T1110 |
| Persistence | Valid Accounts *(Investigated)* | T1078 |
| Command and Control | Application Layer Protocol | T1071.001 |

---

## Skills Demonstrated

- Threat Hunting Methodology
- Multi-Source Log Correlation
- Splunk SPL Development
- Windows Event Analysis
- Network Traffic Analysis
- HTTP Log Investigation
- IDS Alert Validation
- IOC Pivoting
- Timeline Reconstruction
- MITRE ATT&CK Mapping
- Security Investigation Documentation

---

## Final Assessment

The investigation confirmed sustained suspicious activity targeting a Joomla web application. Network and web telemetry consistently showed repeated interaction with administrative resources; however, correlation with Windows Security events and authentication logs found no evidence of successful authentication or host compromise.

The activity is assessed as **suspicious web reconnaissance and repeated application interaction** rather than a confirmed security breach.

---

## Deliverables

```
Threat-Hunts/
├── Hunt-01-Web-Attack-Pivot.md
├── Hunt-02-Joomla-Admin-Access.md
├── Hunt-03-Authentication-Correlation.md
├── Hunt-04-Web-Server-Investigation.md
└── Hunt-05-Threat-Hunting-Summary.md
```

---

## Outcome

Phase 3 demonstrates an end-to-end threat hunting workflow by progressing from initial IDS alerts through evidence collection, cross-source correlation, and final analyst assessment. The documentation mirrors a real SOC investigation process and highlights the importance of validating alerts with supporting telemetry before concluding whether a compromise has occurred.
