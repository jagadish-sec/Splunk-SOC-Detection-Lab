Day 1 – Lab Setup
Environment
Splunk Enterprise Version: 10.4.2
Operating System: Windows 11
Dataset: BOTS v1 Attack Only
Dataset Index: botsv1
Installation
Installed Splunk Enterprise.
Imported the BOTS v1 Attack Only dataset.
Restarted Splunk.
Verified dataset ingestion.
Verification
Query:
| eventcount summarize=false index=*
Result:
botsv1 index detected.
Approximately 955,807 events indexed.
Data Exploration
Query:
index=botsv1
| stats count by sourcetype
| sort -count
Observed log sources:
Sysmon
Windows Security Logs
Suricata IDS
TCP Stream
SMB Stream
HTTP Stream
DNS Stream
IIS Logs
Nessus Scan
FortiGate Logs
Outcome
Successfully deployed a functional Splunk SIEM lab and verified ingestion of an enterprise-scale attack dataset for future detection engineering and threat hunting exercises.
