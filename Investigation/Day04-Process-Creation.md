# Day 04 – Process Creation Investigation

## Objective

Investigate process creation events to identify administrative activity, scripting engines, and potential Living-off-the-Land (LOLBins).

---

## Data Source

- Microsoft Sysmon
- Event ID 1

---

## Queries

### Most Common Processes

```spl
index=botsv1 sourcetype=xmlwineventlog:microsoft-windows-sysmon/operational EventCode=1
| top Image
```

### Command Prompt Usage

```spl
index=botsv1 sourcetype=xmlwineventlog:microsoft-windows-sysmon/operational EventCode=1 Image="*cmd.exe"
```

---

## Findings

- Process creation telemetry was successfully collected.
- Common administrative processes were identified.
- Command Prompt execution was observed.
- No obvious malicious command-line activity was identified during baseline analysis.

---

## Analyst Notes

Process creation events provide valuable visibility into user and system activity. While command-line tools are commonly used by administrators, they are also frequently abused by attackers. Future investigations should correlate process creation with authentication events and network activity.

---

## Conclusion

No confirmed malicious execution observed. Process telemetry will be used during later correlation analysis.
