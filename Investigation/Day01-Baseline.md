# Day 01 - Baseline & SIEM Deployment

## Objective

Deploy Splunk Enterprise, ingest a real-world attack dataset, and verify that the SIEM is operational before beginning any investigation.

---

## Lab Environment

| Component | Value |
|-----------|-------|
| SIEM | Splunk Enterprise 10.4.2 |
| Operating System | Windows 11 |
| Dataset | Splunk BOTS v1 Attack Only |
| Index | botsv1 |

---

## Tasks Completed

- Installed Splunk Enterprise.
- Configured the local administrator account.
- Imported the BOTS v1 Attack Only dataset.
- Restarted Splunk services.
- Verified successful ingestion.

---

## Verification Query

```spl
| eventcount summarize=false index=*
```

### Result

| Index | Events |
|--------|--------|
| botsv1 | 955,807 |

---

## Timeline Discovery

### Query

```spl
index=botsv1
| stats earliest(_time) as First_Event latest(_time) as Last_Event count
| convert ctime(First_Event) ctime(Last_Event)
```

### Findings

| Metric | Value |
|--------|-------|
| First Event | 10 Aug 2016 08:58:51 |
| Last Event | 24 Aug 2016 23:57:44 |
| Total Events | 955,807 |

---

## Analysis

The dataset spans approximately fifteen days of enterprise activity and contains nearly one million events from multiple security data sources.

This establishes the investigation window and confirms that sufficient log coverage exists to perform meaningful threat hunting.

---

## Evidence Collected

- Splunk installation completed successfully.
- Dataset successfully indexed.
- Investigation timeline established.

---

## SOC Relevance

Before investigating any alert, analysts first verify that:

- Logs are available.
- The investigation timeframe is known.
- Data ingestion completed successfully.

Without this baseline, later findings cannot be trusted.

---

## Next Objective

Identify every asset inside the environment and establish an inventory of hosts before beginning authentication and threat analysis.
