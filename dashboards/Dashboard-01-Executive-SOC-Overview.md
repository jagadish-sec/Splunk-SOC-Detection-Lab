# Dashboard 01 — Executive SOC Overview

## Overview

The Executive SOC Overview dashboard centralizes network telemetry, IDS alerts, Windows Security logs, and HTTP traffic to provide rapid situational awareness and help prioritize security events.

## Panels

| Panel | Source | Purpose |
| --- | --- | --- |
| Total Events | `Total_Events.csv` | Overall indexed event volume. |
| IDS Alerts | `Total_IDS_Alerts.csv` | Total Suricata alert count. |
| Windows Events | `Windows_Events.csv` | Windows Security log volume. |
| HTTP Requests | `HTTP_Requests.csv` | Total observed HTTP traffic. |
| Event Timeline | `Events_Timeline.csv` | Activity over time. |
| Top Source IPs | `Top_Source_IPs.csv` | Most active systems. |
| Top Destination IPs | `Top_Destination_IPs.csv` | Most targeted assets. |
| Alert Severity | `alert_severity.csv` | IDS severity distribution. |
| Top IDS Signatures | `Top_IDS_Signatures.csv` | Most frequent IDS detections. |
| Authentication Overview | `Authentication_Overview.csv` | Windows authentication activity. |

## Key metrics

| Metric | Value |
| --- | ---: |
| Total Events | 955,807 |
| IDS Alerts | 1,360 |
| Windows Events | 87,430 |
| HTTP Requests | 23,936 |

## Analyst assessment

This is the project’s primary monitoring interface. It highlights elevated IDS activity, suspicious web requests, and authentication events so analysts can pivot into Dashboard 02 for detection context and Dashboard 03 for investigation evidence.

## Skills demonstrated

- Splunk dashboard development and SPL query design
- Executive security reporting and monitoring
- Windows event, IDS, network, and HTTP log analysis
- Security visualization and analyst triage

## Install in Splunk

Upload the eight additional CSVs in `dashboards/lookups/`, together with the shared IDS lookup files already used by Dashboard 02. Then create a dashboard from source using `dashboards/executive_soc_overview_dashboard.xml` and save it as **Executive SOC Overview**.
