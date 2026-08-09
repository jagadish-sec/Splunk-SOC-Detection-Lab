# Detection: Command Prompt Execution

## Purpose

Identify executions of `cmd.exe` for investigation.

---

## SPL

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4688
New_Process_Name="*cmd.exe"
| table _time host Account_Name Creator_Process_Name Process_Command_Line
```

---

## Findings

36 executions of `cmd.exe` were observed.

Most executions were associated with:

- Nessus vulnerability scanning
- Splunk Universal Forwarder scripts
- Administrative Windows commands

---

## Analysis

Although `cmd.exe` is commonly used by attackers, the observed executions were consistent with legitimate administrative and monitoring activity.

This highlights the importance of contextual analysis rather than relying solely on process names.

---

## MITRE ATT&CK

- T1059.003 – Windows Command Shell

---

## Detection Tuning

Consider excluding:

- Splunk Universal Forwarder parent processes
- Approved vulnerability scanners
- Scheduled administrative scripts

to reduce false positives.
