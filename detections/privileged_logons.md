# Detection: Privileged Logons (Event ID 4672)

## Purpose

Identify logons that receive special administrative privileges.

---

## Data Source

Windows Security Log

---

## Event ID

4672

---

## SPL

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4672
| stats count by Account_Name, host
| sort -count
```

---

## Findings

- Administrator received elevated privileges on `we8105desk` and `we9041srv`.
- Computer accounts (`WE9041SRV$`, `WE8105DESK$`, `WE1149SRV$`) generated expected privileged events.
- SYSTEM, LOCAL SERVICE, and NETWORK SERVICE appeared as expected.
- No evidence that `bob.smith` received special privileges.

---

## Analysis

The observed privileged logons are consistent with expected administrative and operating system behavior. No anomalous privileged account activity was identified during this stage of the investigation.

---

## MITRE ATT&CK

- T1078 – Valid Accounts

---

## Detection Tuning

Future improvements:

- Alert only on unexpected privileged accounts.
- Exclude known service and computer accounts.
- Correlate with process creation (4688) and network share access (5145) for higher-confidence detections.

---

## SOC Relevance

Monitoring Event ID 4672 helps identify administrative activity and can reveal unexpected privileged logons that may indicate credential compromise or privilege escalation.
