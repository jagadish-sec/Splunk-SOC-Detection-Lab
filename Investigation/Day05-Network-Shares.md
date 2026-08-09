# Day 05 – Network Share Investigation

## Objective

Investigate Windows administrative share access to identify potential lateral movement.

---

## Data Source

Windows Security Logs

Event ID 5145

---

## Queries

### Top Shares

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=5145
| stats count by Share_Name
```

### Share Access by User

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=5145
| stats count by Account_Name, Share_Name
```

---

## Findings

Observed administrative shares:

- IPC$
- C$

Primary accounts:

- Administrator
- Machine accounts

Administrative share access originated from expected enterprise systems.

---

## Analyst Notes

Administrative shares are commonly used for legitimate Windows administration. Their presence alone does not indicate malicious lateral movement. Additional evidence such as suspicious logons or malicious process execution would be required before escalating.

---

## Conclusion

No evidence of unauthorized SMB-based lateral movement identified during this investigation.
