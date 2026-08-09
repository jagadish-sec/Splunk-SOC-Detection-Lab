# Detection: Unusual Command Prompt Execution

## Purpose

Identify executions of `cmd.exe` while reducing known benign activity.

---

## SPL

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4688
New_Process_Name="*cmd.exe"
NOT Creator_Process_Name="*splunkd.exe"
| table _time host Account_Name Creator_Process_Name Process_Command_Line
```

---

## Findings

The majority of executions were associated with:

- Nessus vulnerability scanning
- Joomla application activity
- Windows administrative operations

No clear evidence of malicious command execution was identified.

---

## MITRE ATT&CK

- T1059.003 – Windows Command Shell

---

## Detection Tuning

Current exclusions:

- Splunk Universal Forwarder (`splunkd.exe`)

Future tuning may include approved vulnerability scanners or other trusted parent processes based on the environment.

---

## SOC Relevance

Process names alone are insufficient to identify malicious activity. Effective detections require contextual analysis, including parent process, command line, host, and account.
