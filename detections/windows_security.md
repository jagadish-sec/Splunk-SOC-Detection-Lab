# Windows Security Event Analysis

## Objective

Identify the Windows Security Event IDs available within the BOTS v1 dataset to determine which detections can be implemented.

---

## Query

```spl
index=botsv1 sourcetype=wineventlog:security
| stats count by EventCode
| sort -count
```

---

## Key Findings

| Event ID | Description | Count |
|----------|-------------|------:|
| 4703 | Token Privilege Adjustment | 28,014 |
| 5145 | Network Share Access Check | 24,430 |
| 4656 | Handle to an Object Requested | 8,464 |
| 4688 | Process Creation | 4,041 |
| 4689 | Process Termination | 3,968 |
| 4624 | Successful Logon | 3,209 |
| 4672 | Special Privileges Assigned | 3,081 |
| 5140 | Network Share Access | 3,047 |
| 4776 | Credential Validation | 2,384 |

---

## Detection Opportunities

- Process Creation Monitoring
- Privileged Logons
- Network Share Monitoring
- Authentication Monitoring
- Credential Validation
- Privilege Escalation Monitoring

---

## Conclusion

The dataset provides sufficient Windows Security telemetry to build practical detections for authentication, privilege usage, process execution, and lateral movement. Event ID 4688 (Process Creation) was selected as the first focus area due to its value in identifying attacker activity.
