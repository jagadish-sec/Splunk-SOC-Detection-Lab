# Day 05 - Correlation Timeline

## Objective

Correlate network alerts with Windows authentication events to determine whether IDS activity coincided with suspicious host activity.

---

## Investigation 1 – Windows Authentication Review

### Query

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4624
| stats count by host Account_Name
| sort -count
```

### Purpose

Review successful Windows logons occurring on monitored systems.

### Findings

Authentication activity was observed across multiple Windows hosts.

Frequently observed accounts included:

- Administrator
- SYSTEM
- Machine Accounts (ending with $)
- Standard Windows service accounts

No unusual authentication patterns were identified during this review.

---

## Investigation 2 – Attempt Correlation

The targeted IP addresses identified by Suricata could not be directly mapped to Windows hostnames using the available telemetry.

Because the network and endpoint logs use different identifiers, direct event-to-host correlation was not possible within the available dataset.

---

## Correlation Summary

### Network Evidence

- High volume of IDS alerts
- Repeated attacks against internal systems
- Significant HTTP and TCP activity

### Endpoint Evidence

- Windows authentication logs reviewed
- Successful logons observed
- No obvious evidence of suspicious authentication

---

## Timeline

```
Network Attack
        │
        ▼
Suricata IDS Alert Generated
        │
        ▼
HTTP/TCP Activity Observed
        │
        ▼
Windows Authentication Reviewed
        │
        ▼
No Suspicious Logon Activity Identified
```

---

## Analyst Assessment

The investigation identified sustained network attacks against internal systems.

Authentication telemetry did not reveal evidence of unauthorized access associated with the observed IDS alerts.

Based on the available data, the activity is consistent with attempted attacks rather than confirmed host compromise.

Additional endpoint telemetry (process creation, PowerShell activity, command execution, or file modifications) would be required to validate successful exploitation.
