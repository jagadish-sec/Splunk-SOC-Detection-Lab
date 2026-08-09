# Detection: Network Share Access (Event ID 5145)

## Purpose

Monitor access to Windows network shares to identify potential lateral movement or unauthorized administrative access.

---

## Data Source

Windows Security Log

---

## Event ID

5145

---

## MITRE ATT&CK

- T1021.002 – SMB/Windows Admin Shares

---

## SPL

### Top Shares

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=5145
| stats count by Share_Name
| sort -count
```

### Share Access by Account

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=5145
| stats count by Account_Name, Share_Name
| sort -count
```

### Share Access by Host

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=5145
| stats count by host, Share_Name
| sort -count
```

---

## Findings

- Administrative shares (`C$` and `IPC$`) were actively used.
- Administrator accessed `C$`.
- Machine accounts accessed `IPC$`.
- No unexpected user accounts accessed administrative shares.

---

## Analysis

The observed activity is consistent with normal enterprise administration and Windows networking. No evidence of malicious lateral movement was identified based solely on Event ID 5145.

---

## Detection Tuning

Consider alerting only when:

- Non-administrative accounts access `ADMIN$`, `C$`, or `IPC$`.
- Administrative shares are accessed from unfamiliar source IP addresses.
- Administrative share access correlates with suspicious process creation or privileged logons.

---

## SOC Relevance

Monitoring administrative share access provides visibility into remote administration and lateral movement. Correlation with authentication, process creation, and IDS telemetry is required before concluding malicious activity.
