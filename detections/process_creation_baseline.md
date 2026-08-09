# Process Creation Baseline

## Objective

Identify the most frequently executed processes in the environment to establish a baseline for future process-based detections.

---

## Query

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4688
| top limit=20 New_Process_Name
```

---

## Findings

The most frequently executed processes belong to the Splunk Universal Forwarder and standard Windows components.

Notable observations include:

- Splunk Universal Forwarder monitoring processes
- Windows service processes
- `cmd.exe`
- `php-cgi.exe`

---

## Analysis

The process baseline indicates that the environment is actively monitored by the Splunk Universal Forwarder. Standard Windows processes dominate execution, while `cmd.exe` and `php-cgi.exe` were identified as candidates for deeper investigation.

The presence of `php-cgi.exe` aligns with previous evidence suggesting that one of the servers hosts a Joomla web application.

---

## SOC Relevance

Process baselining enables analysts to distinguish normal operating system and monitoring activity from unusual process execution. Establishing this baseline reduces false positives and improves the effectiveness of future detections.
