# Detection: Suricata IDS Alert Analysis

## Purpose

Analyze Suricata IDS alerts to identify suspicious network activity, prioritize high-risk events, and correlate findings with endpoint telemetry.

## Data Source

- Suricata IDS

## MITRE ATT&CK

- T1190 – Exploit Public-Facing Application
- T1595 – Active Scanning
- T1071 – Application Layer Protocol

## Queries

### Top Signatures

```spl
index=botsv1 sourcetype=suricata event_type=alert
| top limit=15 alert.signature
```

### Alert Categories

```spl
index=botsv1 sourcetype=suricata event_type=alert
| top alert.category
```

### Severity

```spl
index=botsv1 sourcetype=suricata event_type=alert
| stats count by alert.severity
```

### Top Source IPs

```spl
index=botsv1 sourcetype=suricata event_type=alert
| stats count by src_ip
| sort -count
```

### Top Destination IPs

```spl
index=botsv1 sourcetype=suricata event_type=alert
| stats count by dest_ip
| sort -count
```

## Findings

- DNS malformed request alerts were the most common IDS signature.
- Web application attack signatures indicated repeated XSS-related attempts.
- Most alerts were low severity, but several high-severity alerts were present.
- Primary alert sources were `192.168.250.20` and `40.80.148.42`.
- Primary targets were `192.168.250.100` and `192.168.250.70`.

## SOC Relevance

These detections provide visibility into web attacks, malformed network traffic, and suspicious communications. Correlation with Windows authentication, process creation, and SMB activity improves confidence before escalating an incident.
