# Dashboard 01 – Executive SOC Overview

## Overview

The Executive SOC Overview dashboard provides analysts with a centralized view of security activity across the monitored environment. It combines network telemetry, IDS alerts, Windows Security logs, and HTTP traffic into a single operational dashboard, enabling rapid situational awareness and prioritization of potential security events.

---

## Dashboard Components

| Panel | Description |
|-------|-------------|
| Total Events | Overall indexed events in Splunk |
| IDS Alerts | Total Suricata IDS alerts |
| Windows Events | Windows Security log volume |
| HTTP Requests | Total observed HTTP traffic |
| Event Timeline | Activity over time |
| Top Source IPs | Most active systems |
| Top Destination IPs | Most targeted assets |
| Alert Severity | Distribution of IDS severities |
| Top IDS Signatures | Most frequent IDS detections |
| Authentication Overview | Windows authentication activity |

---

## Key Metrics

| Metric | Value |
|---------|------:|
| Total Events | **955,807** |
| IDS Alerts | **1,360** |
| Windows Events | **87,430** |
| HTTP Requests | **23,936** |

---

## Key Observations

### Event Volume

Nearly one million events were indexed, demonstrating the ability of Splunk to aggregate and analyze telemetry from multiple security sources.

### IDS Activity

A total of **1,360 Suricata alerts** were observed. Alert analysis indicates that web application attacks and malformed network traffic account for a significant portion of detections.

### HTTP Traffic

The environment generated over **23,000 HTTP requests**, with Joomla-related resources representing the majority of web activity. Administrator page requests were investigated further during the threat hunting phase.

### Network Activity

Source and destination IP analysis identified a small number of systems responsible for most network traffic, allowing investigations to focus on high-value targets.

### Authentication Events

Windows Security logs recorded over **87,000 security events**, providing sufficient telemetry to correlate authentication activity with network and web events during investigations.

---

## Analyst Assessment

The Executive SOC Overview dashboard serves as the primary monitoring interface for the project. It provides a concise summary of the environment while highlighting areas requiring further investigation, including elevated IDS activity, suspicious web requests, and authentication events.

The dashboard enables analysts to quickly pivot into detailed detection engineering and threat hunting workflows developed during subsequent project phases.

---

## Skills Demonstrated

- Splunk Dashboard Development
- Executive Security Reporting
- Security Monitoring
- Windows Event Analysis
- IDS Monitoring
- Network Traffic Analysis
- HTTP Log Analysis
- SPL Query Development
- Security Visualization

---

## Related Project Sections

- Phase 1 – Environment Baseline
- Phase 2 – Detection Engineering
- Phase 3 – Threat Hunting
