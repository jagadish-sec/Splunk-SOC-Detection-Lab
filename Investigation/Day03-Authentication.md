## User to Host Mapping

### Query

```spl
index=botsv1 sourcetype=wineventlog:security
| stats count by Account_Name, host
| sort -count
```

### Findings

- `bob.smith` authenticated primarily to `we9041srv` with a smaller number of events on `we8105desk`.
- `Administrator` authenticated to multiple hosts.
- Machine accounts authenticated to their corresponding systems.
- `IUSR` appears only on `we1149srv`.
- `joomla` appears only on `we1149srv`.

### Analysis

The evidence suggests that `we1149srv` hosts IIS and a Joomla application. The relationship between `bob.smith` and `we9041srv` warrants further investigation to determine whether it represents normal administrative activity or something more significant.

### SOC Relevance

Mapping user accounts to hosts establishes normal authentication patterns. This baseline is essential for identifying lateral movement, unusual logons, and compromised accounts during incident investigations.
