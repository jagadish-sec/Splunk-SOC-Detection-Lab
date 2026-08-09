# Day 06 – Suricata IDS Investigation

## Objective

Analyze Suricata IDS alerts to identify suspicious network activity.

---

## Data Source

Suricata IDS

---

## Queries

### Top Alert Signatures

```spl
index=botsv1 sourcetype=suricata event_type=alert
| top alert.signature
```

### Alert Categories

```spl
index=botsv1 sourcetype=suricata event_type=alert
| top alert.category
```

### Severity Distribution

```spl
index=botsv1 sourcetype=suricata event_type=alert
| stats count by alert.severity
```

---

## Findings

- DNS malformed request alerts were the most common signature.
- Web application attack signatures were identified.
- Most alerts were low severity.
- High-severity alerts were limited but present.

Primary source systems:

- 192.168.250.20
- 40.80.148.42

Primary targets:

- 192.168.250.100
- 192.168.250.70

---

## Analyst Notes

The IDS detected a mixture of malformed DNS traffic, web attack attempts, and network anomalies. No conclusions can be made from IDS alerts alone; endpoint and authentication logs are required for confirmation.

---

## Conclusion

Network activity warrants continued monitoring. Correlation with Windows logs will be performed during the next investigation phase.
