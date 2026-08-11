# Day 06 - Findings Summary

## Phase 2 Overview

Phase 2 focused on correlating network-based detections with Windows authentication telemetry to determine whether IDS alerts resulted in observable host activity.

---

## Investigations Completed

- Authentication Analysis
- Network Threat Hunting
- IDS Alert Review
- High Severity Alert Analysis
- Cross-Source Correlation
- Traffic Source Analysis

---

## Key Findings

### Authentication

- Successful Windows logons reviewed.
- Activity primarily consisted of standard administrator, machine, and service accounts.
- No suspicious authentication patterns identified.

### Network

- Suricata generated numerous IDS alerts.
- Two internal systems received the majority of alerts.
- HTTP and TCP traffic represented the largest portion of related network activity.

### Correlation

- Windows authentication events were compared against network detections.
- No direct evidence linked IDS alerts to unauthorized Windows logons.
- Available evidence supports attempted attacks but does not confirm successful compromise.

---

## Skills Demonstrated

- Splunk SPL Query Development
- Windows Event Log Analysis
- Suricata IDS Investigation
- Network Threat Hunting
- Cross-Source Correlation
- Incident Investigation Methodology
- Evidence-Based Security Analysis

---

## Conclusion

Phase 2 established a structured investigation workflow by combining network and authentication telemetry.

While repeated IDS alerts indicated persistent attack activity, the available Windows evidence did not support a conclusion of successful compromise.

Future phases will focus on building reusable detections, dashboards, MITRE ATT&CK mappings, and detection engineering techniques using the knowledge gained during these investigations.
