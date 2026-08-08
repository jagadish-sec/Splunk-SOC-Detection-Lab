## Successful Logon Analysis

### Query

```spl
index=botsv1 sourcetype=wineventlog:security EventCode=4624
| stats count by Account_Name, host
| sort -count
```

### Findings

- Administrator successfully authenticated to both `we8105desk` and `we9041srv`.
- `bob.smith` recorded successful logons on `we9041srv` and `we8105desk`.
- Machine accounts authenticated to their respective hosts.
- Anonymous logons were present but limited.
- Some events contained no extracted account name.

### Analysis

The environment exhibits expected Windows authentication activity. No evidence of widespread or unusual successful logons is visible from this high-level view. Additional context, such as logon types, source IP addresses, and timestamps, is required to identify suspicious authentication behavior.

### SOC Relevance

Reviewing successful logons establishes a baseline of expected authentication patterns. This helps analysts recognize unusual account usage, privileged access, or lateral movement during an incident investigation.
