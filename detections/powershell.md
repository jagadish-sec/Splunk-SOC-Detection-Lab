# PowerShell Detection

Identify PowerShell execution for review, prioritising encoded commands and unusual parent processes.

## Starting Splunk search

```spl
index=botsv1 (sourcetype=XmlWinEventLog:Microsoft-Windows-PowerShell/Operational OR EventCode=4104)
| search CommandLine="*powershell*" OR ScriptBlockText="*"
| table _time, host, user, CommandLine, ScriptBlockText
```
